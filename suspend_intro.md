---
title: "使うだけに絞ったsuspend関数入門"
layout: page
---


## はじめに

このkotlin lessonではhttpにはfuelを使ってもらっている。
だからコルーチンというかawaitResponseResultとかを使ってもらっている。

で、コルーチンの解説なんて死ぬほどあるだろうから適当にググって適当に書いてください、と言った所、みななんか全然分かってない感じのコードを書いてくる。

何故だ？解説とかあるだろ？と思ってググると、「確かにこれじゃ入門者は使えないな…」というのばかり。
という事で、あまり分かってない人がとりあえず使うのに必要となる事だけを解説してみます。

hello worldとかでは無くて、そういうのを動かした後に読む文書です。

### この文書を書こうと思ったきっかけ

こんなコードが来ました（多少解説の為に変更しています）。

詳細はいいとして、putContentInnnerはsuspend関数で中にawaitResponseResultなどを呼んでいる。
そしてそれをputContentという関数でラップしていて、この中でlaunchが呼ばれている。
awaitResponseResultというのはsuspend関数です。

```
class ContentSender {
    // ... 中略 ...

    private suspend fun putContentInner(apiUrl: String, branchName: String, fname: String,
     base64Content: String, accessToken:String) : Response {
        val (_, _, result) = "$apiUrl?ref=$branchName".httpGet()
            .header("Authorization" to "token ${accessToken}")
            .awaitResponseResult(Content.Deserializer(), Dispatchers.IO)

        val contParam = arrayListOfContentParameter(branchName, fname, base64Content)

        result.fold(
            { cont -> contParam.add("sha" to cont.sha) },
            { _ /* err */ -> {} }
        )


        val json = jsonBuilder {
            val obj = beginObject()
            contParam.map { (k, v) -> obj.name(k).value(v) }
        }

        val (_, resp, _) = apiUrl.httpPut()
            .body(json)
            .header("Authorization" to "token ${accessToken}")
            .header("Content-Type" to "application/json")
            .awaitStringResponseResult(scope = Dispatchers.IO)
        return resp
    }

    fun putContent(apiUrl: String, branchName: String, fname: String, base64Content: String, accessToken: String) {
        job = Job()
        launch {
            val resp = putContentInner(apiUrl, branchName, fname, base64Content, accessToken)

            val sendCode: Int = when(resp.statusCode) {
                200, 201 -> 2
                else -> 1
            }
            prefs.edit().putInt("success_post",sendCode).commit()
        }
    }
}
```

そしてこれをActivityから呼んでいた。

```
class MainActivity : AppCompatActivity() , CoroutineScope {
    // ... 中略 ...
    fun onClick() {
        val base64Content = readBase64(fileName)
        val sender = ContentSender(this)
        sender.putContent(apiUrl, "master", fileName, base64Content, accessToken)
        finish()
    }
```

これではputContentが全てを送信する前にfinishが呼ばれてしまうのでまずい。
これがなぜ悪いのか、どう直すべきなのか、を理解する程度の説明をするのがこの文書の目的です。

なお、答えだけなら最後をみればわかると思うので答えだけ知りたい人は説明を飛ばして最後の答えを見て下さい。

### この文書で解説する事、しない事

[公式の入門](https://kotlinlang.org/docs/reference/coroutines/coroutines-guide.html)に書いてあるような事は説明しません。
全体像なども解説はしません。
内部の仕組みも解説しません。

その代わり、呼び出しのフローがどうなって何がどこのスレッドで実行されるか、という事を理解して、
上記のコードがなぜ悪いのか、どう直すべきなのかを理解出来る程度の到達点を目指します。
良くある普通のケースでどうなるのか、を話すので、「dispatcherとか使えばそうじゃないケースも作れるよ」とかそういうのは書きません。

suspend関数は柔軟に作られていていろいろな使い方が出来るのですが、
基本的な非同期の使い方を理解してしばらく使っていたら、自然とそれ以外の使い方も理解出来るようになります。
だから入門する時に大切なのは、正しく全てを理解するのでは無く、非同期の一番普通の使い方を正しく理解する事です。

そこでこの文書では、非同期の普通のユースケースに絞ってどうなっているのかを説明します。
一般的には言えない事でもこのケースで言える事なら断言してしまっています。

あくまで最初に見様見真似で使い始める時に必要な最低限の事だけ書きます。
ある程度使って慣れてきたら、ちゃんと内部の仕組みを理解するドキュメントに進んでください。

次のステップに進むには、[菊田さんが挙げている文書](https://yoheikikuta.github.io/study_kotlin_coroutine/) でだいたいいいと思います。

解説する事をまとめると以下になります。

- なんとなくlaunchでくくればいいや、という理解から卒業する
- suspend関数を利用者の視点で理解する（仕組みは知らん）
- suspend関数を自分で書いて公開出来るようにする（ただし中では他のsuspend関数呼ぶだけ）
- 呼び出し時にどこがどのスレッドで動くかを理解する


# 非同期なAPIとは何か

suspend関数とは、「非同期のAPIをラップしたもの」です。
そこでsuspend関数とは何かを知る為には、
「非同期のAPIとは何か？」という事を理解するのが前提となります。

そこで非同期のAPIについて簡単に説明します。

## 非同期なAPIの具体例

例として、Httpというクラスがあって、これにpostAsyncというメソッドとpostBlockというメソッドがあるとします。
postAsyncが非同期にHTTPのpostを行うAPIで、postBlockが非同期でない、同期のブロッキングする形のAPIとします。

### 非同期でない場合(postBlockの場合)のシーケンス

まずは非同期じゃない、ブロッキングのAPIの場合のシーケンスを考えます。以下のようなコードです。
(疑似コードなので詳細はつっこまないでください)。

```
class MainActivity {
    fun onCreate() {
        funcA()
        Http.postBlock(url, some_content)
        funcB()
    }
}
```

これのシーケンスは以下のようになります。

![同期の場合のシーケンス図](./imgs/postBlock_seq.png)

ここで、postBlockはonCreateの中でその実行を最後まで終える（シーケンスの中でonCreateの箱の中に全部入っている）というのが同期APIの基本です。

funcBを呼ぶ時点では既にサーバーへのポストが終わっている、というのが同期APIの特徴です。


### 非同期なAPIでのシーケンス

上記のコードを少し変えて、postAsyncを使うように変えてみましょう。(疑似コードなので詳細はつっこまないで！)

```
class MainActivity {
    fun onCreate() {
        funcA()
        Http.postAsync(url, some_content, onCallback)
        funcB()
    }

    fun onCallback() {
        funcC()        
    }
}
```

この場合のシーケンス図は以下のようになります。

![postAsyncのシーケンス図](imgs/postAsync_seq.png)

ごちゃごちゃしてますね。いくつか重要な所があるので、以下の説明と突き合わせてみてみてください。

まず、postAsyncは呼ぶと、すぐ終わって帰ってくる。これは一瞬です。
そして実際のサーバーへのpostはここでは行われない。謎の誰かが「別スレッド」で行います。

funcBの実行は、実際にサーバーへのpostが行われるよりも前に実行されます。
普通はonCreate自身もサーバーへのpostが行われるよりも前に終わってしまいます。

そして別スレッドで実際にサーバーへのpostが終わると、謎のメカニズムでシステムに通知がいって、
システムからonCallbackが呼ばれます。
このonCallbackは「主スレッド」で呼ばれるというのは重要なポイントですが、
funcBと違ってサーバーへの実際のpostが終わった後である事が保証されているタイミングで呼ばれる所が違います。

以上の比較を元に、非同期APIの基本的な前提を次にしていきたいと思います。

## 非同期APIと実行されるスレッド

非同期APIを理解する為には、コードのどこの部分がどこのスレッドで実行されるのか、という呼び出しシーケンスをちゃんと理解するのが大切です。
ここで細かい所を曖昧にしないでちゃんと厳密に考えるのが非同期APIやsuspend関数を理解するコツです。

ではその辺の説明をしていきましょう。

### 非同期なAPIには別のスレッドが前提とされている

非同期のAPIというのは、暗黙のうちに「別のスレッド」が前提とされています。
そこでこの記事でも、この呼び出しの元である「主スレッド」と、
非同期APIが実際の仕事を行う「別のスレッド」の二つを考えていきます。

なお、初心者のうちはこの二つだけ考えれば十分です。
イキリついったらーが一般的に語りたがる事がありますが、うるせー、とか言っておきましょう。

### postAsyncは「主スレッド」で実行される（そしてすぐ返ってくる）

ちょっと細かい事になるのですがはっきりさせておきたい事として、postAsyncという呼び出し自体は「主スレッド」で行われます。
そしてすぐ戻ってきます。
あくまで「主スレッド」で呼び出して戻ってくる、というのは大切なポイントです。

### コールバックはどこのスレッドで呼ばれるか

postAsyncにはコールバックを渡すのが良くある形式です。
コールバックはどこのスレッドで呼ばれるかというと、これも「主スレッド」です。
ただし、先ほどのシーケンス図からも分かるように、現在のメソッドの中ではなくて、あとでシステムから呼ばれます。
この場では呼ばれない、というのも重要なポイントです。

ただし、このコールバックもあくまで「主スレッド」で呼ばれます。

### では「別スレッド」で実行されるのはどこなのか？

という事でここまで、コード上に出てくるものは全部主スレッドで実行されます。
では「別スレッド」で実行されるのはコードのどこなのか？というと、「コードの上には」どこにもありません。

これは重要なポイントです。書かれているコードは、全部「主スレッド」で実行されます。

では、「別スレッド」で実行されるのは何か？というと、「書かれてない何か」が実行されるのです。
非同期APIを考える上で大切なのは、「書かれてない何かの処理がある」という事、
そしてそれが「書かれてない暗黙の別スレッドで実行される」という事を意識する事です。

書かれてない物がどこで実行されるか、という事をイメージするのが非同期APIのコード、
ひいてはsuspend関数のコードの実行を理解する秘訣です。

書かれてない何かの処理が、別のスレッドで実行されて、そこでpostの処理が行われます。
そして終わったら初めて「書かれている」コードである、コールバックに「主スレッドで」戻ってくるのです。

### 非同期APIまとめ

- 非同期APIには、暗黙の別スレッドがあってそこで処理が実行される
- 非同期APIの暗黙の別スレッドでは、書かれてない処理が実行される
- postAsyncの呼び出しもコールバックの実行もあくまで「主スレッド」で行われる

このようなイメージを持っておけば初心者は十分です。

# suspend関数とは

次にようやく本題のsuspend関数の説明に入ります。
suspend関数は実装側と呼ばれた時の振る舞いが関連してて、一度に考えるのがちょっと難しいので分けて説明していきたい。

まずsuspend関数とは

1. 非同期APIをラップしたものである
2. (body側) suspend関数のbodyでは、他のsuspend関数呼び出しのあるところでコードがブロックに分かれてキューに入れられる
3. (呼ぶと何が起こるか）1つ目のブロックだけ実行して返る非同期APIのような物として振る舞う

という話となります。

また、ポイントとしては、見た目と実際の実行され方が大きく違う、という事があげられます。
「実際の実行のされ方」の方が単純なので、見た目よりも先に、
実際の実行のされ方のほうを理解してしまうのが急所となります。

## suspend関数は非同期なAPIをラップしたものである

一番最初でありながら一番大切な事として、suspend関数とは、少し特殊な「非同期APIである」という事があります。
一見そうは見えないけれど非同期APIです。この事をしっかり心に刻み込むのがsuspend関数マスターの第一歩です。


非同期APIという事は、先程説明したような以下のような前提が成立します。

1. 暗黙に、「別のスレッド」というのがある
2. 謎の誰かがいて、この「別のスレッド」で行われる(コードには現れない)謎の処理がある
3. 呼び出し自体はすぐ返ってくるが実際の処理はこの時点では行われていない
4. 実際の処理の終了は、呼び出した関数が終わった結構あとに、コールバックで返ってくる

これにプラスしてもうひとつ、「書かれている処理は全て主スレッドで行われる」というのも加えれば、
suspend関数を実際に使う時に一番重要な「どこの処理はどのスレッドで実行されるか」は理解出来たも同然です。

以下、少し上記の事を補足してみましょう。

### 主スレッド（関数の内部の実行スレッド）

少し特殊とはいえ所詮は非同期APIなので、このsuspend関数を呼び出すと、
ここに書かれている処理はその呼び出されたスレッドで実行されます。
これを主スレッドといいました。

suspendというキーワードのついた関数のbody部にかかれている事は、全部この主スレッドで実行されます。
ActivityのonCreateとかでlaunchとかの中で呼び出せば、それはUIスレッドという事になります。

launchの中は、同じスレッドで実行されて、べつだん新たなスレッドが作られる訳じゃない、というのは大切なポイントです。
これはpostAsyncの呼び出し自体は主スレッドで行われていたのと同じ事です。


### 別のスレッド

ですが、非同期APIなので、書かれていない何かの処理が裏にはあるはずです。
postAsyncでは実際にサーバーにpostする処理はpostAsyncの中とは別のどこかで、「謎の誰か」が「別のスレッド」で行っていました。
非同期APIはこの「書かれてない事を想定する」のが大切で、それはsuspend関数でも同様です。

suspend関数を呼び出すと、その実行は「主スレッド」で行われてますが、
それと同時に書かれてない「何かの処理」が「別スレッド」で行われます。
それはそのsuspend関数の本体となるような何かの処理ですが、それが何かはsuspend関数の名前から想像するしかありません。
それが非同期APIという物です。

suspend関数でも、書かれてない何かの処理が謎の誰かによって別スレッドで行われます。

「書かれていない処理」が実行される、というのはすごく大切なところなので三回朗読したい（別にしなくていいです）。
逆にいうと、「書かれている処理は全部主スレッドで実行される」。これもすごく大切なところです。
suspend関数のbodyにかかれているコードは、すべて主スレッドで実行されます。
これはすごく大切な事なので三回朗読、、、はしなくていいですが重要なポイントです。

分かってない人と話していると、皆がここを誤解している模様。
裏のスレッドで実行されるのは「書かれてないコード」で、非同期APIには必ずそれがあるはずなのです。


### 別のスレッドが終わったあとの通知

非同期APIでは、呼び出した結果はすぐ戻ってきてしまいます。
だから呼び出した関数、例えばonCreateの実行はそのままずんずん進んでしまい、サーバーに実際にリクエストがpostされるよりも前に最後まで実行を終えてreturnしてしまいます。

そして、別スレッドの処理が終わると、謎のメカニズムを通じて主スレッドで「あとから」コールバックという関数が呼ばれるのでした。

susupend関数でもほぼこのメカニズムは同じです。
suspend関数を呼んだ時にはすぐに返っきて、「あとから」終わったという通知が主スレッドに何かしらの形でやってきます。

ただ、普通のコールバックとは違ってsuspend関数の結果の呼び出しは「直接はコードの上では見えない」。ここがsuspend関数がちょっと難しく感じるところです。

ですが見た目の事はおいといて、内部の実行としては非同期APIと変わりません。

- 主スレッドで呼び出されたら何か短い処理が走ってすぐ返ってくる
- 別スレッドで謎の誰かに通知がいって、コードには書いてない何かの処理が裏で行われる
- 別スレッドの処理が終わると、謎の通知メカニズムで「主スレッドに」、「あとから」呼び出しで通知が来る

これがsuspend関数の実行されるスレッドを考える時の基本になります。


## suspend関数のbody: コードの分割とキューによる実行

この文書の重要な目的の一つに、あなた自身がsuspend関数を作って他のクラスに公開する、
という事を自由に出来るようになるというのがあります。（これは最初にあげたコードをどう直すべきか、というっ直接の答えになっています）

そこであなたがsuspend関数を実装する場合を考えましょう。

suspend関数を実装する場合は、その内部で「別のsuspend関数を呼び出す」という事をやります。
すると「コードの分割」というのが行われて、「分割されたコードがキューに詰められて順番に実行」されます。
これらの事を順番に見ていきましょう。

### suspend関数のbodyでは、別のsuspend関数を呼び出す

あなたがsuspend関数を作る時。
このsuspend関数のbodyのところでは、中で別のsuspend関数を呼び出す事になります。
これらはプラットフォームから提供されている何かしらのプリミティブとなります。

普通の関数の実装でも最終的には+とか-とかforとか、とにかくプラットフォームで提供されているプリミティブを呼び出しますよね。同じ事で、suspend関数を実装する時は、最終的にはプラットフォームから提供されるsuspend関数のプリミティブを実行する事になります（自分でも作れますが、初心者のうちは提供されるプリミティブがある、と考える方がわかりやすい）。

suspend関数とは、基本的には「非同期な事をやりたい」時に使う仕組みです。
だからあなたは非同期で実行したい事を持っている事になります。

例えばサーバーに何かをpostする場合、いろいろ処理をした上で最終的にはサーバーにpostします。
このpostは非同期APIになっている訳です。

これがプラットフォームから提供されるプリミティブ、という事になります。
とにかくsuspend関数は別のsuspend関数を呼び出していて、これをずーっとたどっていくと最後にはプリミティブのsuspend関数にいきつきます。

このsuspend関数を自分で書く時に、body部には「必ず別のsuspend関数呼び出しがある」というのは重要なポイントです。
「そうじゃない場合はどうなの？」とか初心者のうちは考えてはいけません。
「必ず別のsuspend関数を呼び出す」と強く信じ込むのが大切です。
良くある正常なケースをちゃんと理解する。まずはこれが大切です。

### suspend関数のbody部は、別のsuspend関数呼び出しを区切りにブロックに分割される

suspend関数が特殊で、知らないと理解出来ない事の一つに、書かれているコードが変形（コンバート）される、という事情があります。
これは使うだけの人も理解しておく必要がある。

suspend関数のbodyは、必ず別のsuspend関数を呼び出す、と言いました。
この別のsuspend関数の呼び出しのところを区切りに、コードがブロックに分割される、
というのが特殊なところです。

例えば、以下のような関数があったとします。
susFuncXXXはsuspend関数、funcXXは普通の関数とします。

```
suspend fun susFuncA() : Int{
    funcB()
    susFuncC()

    funcD()
    funcE()
    susFuncF()

    return funcG()    
}
```

このコードは、3つのブロックに分かれる

![bodyはブロックに分割される](imgs\body2block.png)

このブロックは、定義により「最後のブロック以外はすべてsuspend関数呼び出しで終わる」というのが重要です。


### 分割されたブロックはキューに詰められて順番に実行される

suspend関数を呼び出すと、このブロックをキューに詰めて、まずキューの先頭のブロック、つまり一つ目のブロックを実行して返ります。
実行される時はキューから取り除かれる。

上記の例の場合、funcB()とsusFuncC()の2つが実行されて、その時点で返ります。
この実行は「主スレッド」です。

susFuncC()は非同期APIなので、呼ばれるとすぐ返ってくる。
ただ非同期APIなので、そこで暗黙の別スレッドで、謎の処理が走り始めます。

![suspend関数は非同期APIなので暗黙の処理と別スレッドがある](imgs/suspend_as_async.png)

susFuncA()の呼び出しはfuncB()とsusFuncC()を呼んだところですぐ終わって戻るのですが、
やがてsusFuncC()に関連した別スレッドの謎の処理が終わると通知が来て、
「主スレッドで」コールバックが呼ばれます。

このコールバックが、「キューの次のブロック」を、呼びます。
このコールバックは裏で生成されるのでコード上は見えません。
ですが、キューの次のブロックが実行される、という事、そしてそれが「主スレッドである」という事を理解するのが大切です。

さて、2つ目のブロックが実行されると、funcD()、funcE()、susFuncF()が呼ばれます。
そして最後のsusFuncFは非同期APIなので、呼ばれるとすぐ戻ってくる。
ここでsusFuncCのコールバックが終わります。

そしてここで、暗黙の別スレッドで、対応する謎の処理が走ります。

ここまでで、最初と最後以外のブロックのパターンが見えてきたので、まとめてみます。

- 前のブロックの非同期APIのコールバックとしてブロックは実行される
- これは主スレッドである
- 現在のブロックは、いつも最後の非同期APIを呼んで終わる（ここでコールバックは終わる）
- あとからどっかのタイミングで、最後に呼んだ非同期APIのコールバックが呼ばれて、そこで次のブロックの実行を始める

これが間のブロックの基本となります。

### 最後のブロックを考える

最後のブロックは少し特殊なので別に考える必要があります。
この最後のブロックでこのsuspend関数の呼び出しが終わりますが、そのあとは何が起こるのでしょうか？
そのあとは、「呼び出しもとの、次のブロック」が実行されます。

このため、この最後のブロックを考えるには、susFuncAを呼び出す側を考える必要があります。

例えば以下のようなコードだったとしましょう。

```
suspend fun susFuncZero() : Int {
   funOne()
   val res = susFuncA()

   funTwo()
   funThree()
   return susFunHi(res)

}
```

この一つ上の関数も、ブロックに分けてキューに詰められています。
そして、susFuncA()の実行が行われている時には、このsuspend関数呼び出しのキューには二番目のブロック、つまりfuncTwoから始まるブロックが先頭に入っています。

susFuncA()の最後のブロックの呼び出しが終わったら、そのままこの次のブロックが実行されます。
つまりsusFuncAの最後から二番目のコールバックでは、

- 最後のブロックのコードが実行される
- 呼び出し元の次のブロックが実行される

の2つが実行されます。

つまり最後のブロックというのは、一つ上のブロックに分割された時の次のブロックの前にくっつく、前処理のようなものな訳ですね。
上の例でいけば、susFuncAの最後のブロックは、susFuncZeroのfunTwoから始まるブロックの前にくっつく。

実は最後のブロックだけじゃなくて最初のブロックも同様に呼び出し元のブロックの最後にくっつきます。

だからイメージとしては、上記の関数は以下のように分けられる訳です。

![最初と最後のブロックは、呼び出し側のブロックにくっつく](first_last_block.png)


### susFuncA()を呼ぶ側の視点で考える

さて、細かい挙動は上の通りなのですが、一度こうした事を理解したら、
実際に読む時にはいちいち中の事は考えずに呼ぶsuspend関数全体を一つの非同期APIのように考えれば良い、
というのがこのメカニズムのセールスポイントです。
どんな感じにイメージするか、というのを以下に書いてみます。

susFuncA()というのは、一回呼ぶとすぐに返ってきます。
で、中でいろいろコールバックのチェーンが呼ばれるのですが、最後にはコールバックが呼ばれて続きが呼ばれる訳です。

間のチェーンは外からは考えなくてよろしい。
とにかく、susFuncA()を呼ぶと、途中で返ってくる。
そして間で何かいろいろ起こるらしいが、結局最終的には最後に続きのブロックを実行する。（ただししばらくたった後に）

だから使う側からは、この最初と最後だけを意識して、
「この関数を呼ぶとすぐ返ってきて、やがてあとからコールバックが呼ばれる」
という普通の非同期APIのように考えればいい訳です。

脳内のイメージとしては以下のようになります。

TODO: 図

だから呼ぶ側として先程の関数を見直すと、

```
suspend fun susFuncZero() : Int {
   funOne()
   val res = susFuncA()

   funTwo()
   funThree()
   return susFunHi(res)

}
```

susFuncA()は非同期APIで、コールバックにresより先が呼ばれる、というふうに読める。

だから心の中でこのコードをブロックに分割して、ブロックの最後の非同期APIのコールバックで次が呼ばれる、と考えると、実際の挙動とコードの見た目がだいたい一致します。

TODO: 図

### 全部の呼び出しは、展開すると一本のキューに（だいたい）なる

さて、suspend関数のbodyの最初は呼び出し元の前のブロックに、bodyの最後のブロックは呼び出し元の次のブロックにくっつく、という事を踏まえて、もっと深いネストの場合を考えます。

suspend関数からsuspend関数が呼ばれてさらにそこから別のsuspend関数が呼ばれて、、、とつらなっている事を考える。

これも結局はそれぞれがブロックに分けられて、一番最初のブロックは呼び出し元のブロックにくっつく。
呼び出し元のブロックが最初のブロックなら、さらに呼び出しもとの呼び出しもと、つまり2つ上のブロックにくっつく。

こうして順番に考えていくと、結局suspend関数の呼び出しのツリーは、一本のブロックのリストになります。
厳密には内部に分岐とかループがあるので一本のリストじゃないのですが、そういうのがなければ一本のリストになるので、気分的には全部ブロックに分けられて一本のキューに詰められる、と思っておけばよろしい。

例えば先程の2つの関数呼び出しを合わせて見ると、

```
suspend fun susFuncA() : Int{
    funcB()
    susFuncC()

    funcD()
    funcE()
    susFuncF()

    return funcG()    
}

suspend fun susFuncZero() : Int {
   funOne()
   val res = susFuncA()

   funTwo()
   funThree()
   return susFunHi(res)

}
```

このsusFuncZeroの呼び出しは、以下の図のような一本のキューになります。

TODO: 図

そして各ブロックの最後は必ず非同期APIの呼び出しになっていて、そのコールバックがあとから呼ばれる。
そのコールバックでは次のブロックが実行される。

そう考えておくと、だいたいのフローは正しく理解出来る事になります。

### suspend関数まとめ

- suspend関数は非同期APIをラップしたものである
- suspend関数のbodyには、別のsuspend関数の呼び出しがある
- suspend関数のbodyは、この別のsuspend関数呼び出しで区切られてブロックに分けられる
- 最初のブロックは呼び出し元のブロックの最後にくっつき、最後のブロックは呼び出し元の次のブロックの頭にくっつく
- 全ブロックは（気分的には）一本のキューに詰められて順番に実行される

このくらいの理解で十分と思います

# 問題のコードをどう直すか

ここまででsuspend関数の使う上での基本を説明してきました。

以上を踏まえて元のコードをどう直すべきか、という話をしていきます。

## launchで何が起こるか

suspend関数を呼ぶコードを書くと、なんかエラーになって仕方ないので良く分からないけどlaunchでくくる、というのが良くやられる事と思う。
ただここまでの説明をもとにすればもうちょっとマシな理解は出来ると思うので簡単に説明しておきます。

### launchはキューをセットアップして実行する

launchというのは、ここまで説明したキューをセットアップして、中をブロックに分割してキュ=に詰めて、最初のブロックを実行する、という事をやるものと思うとだいたい正しい。
launchの中はsuspend関数のbodyと同様のルールでコンバートされます。

launchは別のスレッドを作り「ません」。
ここは誤解してはいけないところで、呼び出しスレッドと同じスレッドで実行されます。

ただし、その場では（最初のブロックしか）実行されない。
あとからコールバックで実行されます。
launchは非同期APIなので、実行すると、1つ目のブロックを実行するだけですぐに返ってきます。

細かい事ですが、何がどこのスレッドで実行されるかは使うだけでも正確に理解している必要がある。
「launchが別のスレッドを作って内部を別スレッドで実行する」というのは完全に間違っているので、
これがなぜ間違えているのいかは正確に理解している必要があります。


### launchの処理が終わったあとに何かをやりたい！という場合

さて、launchを実行すると、最初のブロックだけ実行して返ってきてしまう。

これは非同期APIなので暗黙の別スレッドがあり、そこでいろいろと実行される。
いろいろ裏で動いてしばらくたったあとに（最後の）コールバックが呼ばれるのですが、
このlaunchの最後のコールバックをlaunchを呼び出した人が受け取る方法はありません。

最初に挙げた問題のコードに戻ってみましょう。


```
class MainActivity : AppCompatActivity() , CoroutineScope {
    // ... 中略 ...
    fun onClick() {
        val base64Content = readBase64(fileName)
        val sender = ContentSender(this)
        sender.putContent(apiUrl, "master", fileName, base64Content, accessToken)
        finish()
    }
```

このsender.putContentは中にlaunchが書いてあるので、このコードはこう書かれているのと気分的には同じです。

```
class MainActivity : AppCompatActivity() , CoroutineScope {
    // ... 中略 ...
    fun onClick() {
        val base64Content = readBase64(fileName)
        launch {
           funcA()
           susFuncB(base64Content)
           funcC()
           susFuncD()
        }
        finish()
    }

```

このfinish()は、susFuncBの呼び出しが終わったあとに実行されてしまいます。
susFuncBの暗黙に想定する別スレッドでの処理はまだ終わっていません。もちろんfunCやsusFuncDはまだ呼び出されてもいません。

その事はここまでの説明でわかると思います。launchを呼び出すonClick()の中では、このlaunchの終わりを知る事は出来ない。

「出来ない、といっても、俺はサーバーにpostが終わったあとにActivityをfinishしたいんだよ！どうしたらいいの！？」

というのが良くあるシチュエーションです。

上記のsusFuncCのあとにfinishを実行するには、launchの中に入れるしか無い。


```
class MainActivity : AppCompatActivity() , CoroutineScope {
    // ... 中略 ...
    fun onClick() {
        val base64Content = readBase64(fileName)
        launch {
           funcA()
           susFuncB(base64Content)
           funcC()
           susFuncD()

           // NEW!!!
           finish()
        }
    }

```

こうすればこのfinishは、ブロックに分割される最後のブロックに入るので、最後に実行されます。
つまり、suspend関数のブロックが実行された最後に何かを実行したい場合、このキューに入れるために自身もブロックに入るしか無いのです。

### ではどうやって関数にくくりだすのか？

そうはいっても元のコードのように、ContentSenderのような送信の部分を別のクラスにしたい、という場合はあります。
こうやってファイルをまたぐ場合、どうやってlaunchの中に入れたらいいんだ？となる。

ここで発想の転換が必要になる。

「ContentSenderのputContentの中にlaunchを入れてはいけない」

launchは一番上の、このメソッドを呼ぶ人がつけるものです。
なぜならそうでないと、このメソッドを終わったあとに何かをしたい、という処理をブロックに出来ないからです。

でもputContentにlaunchをつけないと、「suspend関数の呼び出しは出来ない」ってコンパイラに怒られるんだよ！と言われそうですが、先に答えを言うと、putContentをsuspend関数にすればコンパイラには怒られませんし、それが正解となります。

その事をもうちょっと詳しく見てみましょう。

まず、元のコードは以下でした。

```
class ContentSender {
    // ... 中略 ...

    private suspend fun putContentInner(apiUrl: String, branchName: String, fname: String,
     base64Content: String, accessToken:String) : Response {
        val (_, _, result) = "$apiUrl?ref=$branchName".httpGet()
            .header("Authorization" to "token ${accessToken}")
            .awaitResponseResult(Content.Deserializer(), Dispatchers.IO)

        val contParam = arrayListOfContentParameter(branchName, fname, base64Content)

        result.fold(
            { cont -> contParam.add("sha" to cont.sha) },
            { _ /* err */ -> {} }
        )


        val json = jsonBuilder {
            val obj = beginObject()
            contParam.map { (k, v) -> obj.name(k).value(v) }
        }

        val (_, resp, _) = apiUrl.httpPut()
            .body(json)
            .header("Authorization" to "token ${accessToken}")
            .header("Content-Type" to "application/json")
            .awaitStringResponseResult(scope = Dispatchers.IO)
        return resp
    }

    fun putContent(apiUrl: String, branchName: String, fname: String, base64Content: String, accessToken: String) {
        job = Job()
        launch {
            val resp = putContentInner(apiUrl, branchName, fname, base64Content, accessToken)

            val sendCode: Int = when(resp.statusCode) {
                200, 201 -> 2
                else -> 1
            }
            prefs.edit().putInt("success_post",sendCode).commit()
        }
    }
}
```

このputContentが間違えている。
このputContentは、以下のようにしておくのが正しい。

```
    suspend fun putContent(apiUrl: String, branchName: String, fname: String, base64Content: String, accessToken: String) {
        val resp = putContentInner(apiUrl, branchName, fname, base64Content, accessToken)

        val sendCode: Int = when(resp.statusCode) {
            200, 201 -> 2
            else -> 1
        }
        prefs.edit().putInt("success_post",sendCode).commit()
    }
```

違いは2つ。

1. putContentの先頭にsuspendがついている
2. launchとjobがなくなっている

簡単にまとめておきましょう。
「終了を待つ必要がある非同期APIを作る場合は、自身をsuspend関数として公開する」これが基本となります。

だから、Activityなどの一番上じゃないところで、


```
   fun someFunc() {
       launch {
          ....
       }
   }
```

という感じのコードがあったら、だいたい間違いです。
launchは使う側にやらせるのが正しい。そうしないと終わったあとを受け取るコードをブロックに出来ないから。

だからこういうコードはだいたいは以下のように直すべき

1. suspendをつける
2. launchを取る

つまりこうなる。

```
   suspend fun someFunc() {
      ....
   }
```

### 呼ぶ側のコードの修正

ここまでくれば自分でもわかると思いますが、Activity側のコードの正解も見ておきましょう。
まずは元のコードは以下でした。

```
class MainActivity : AppCompatActivity() , CoroutineScope {
    // ... 中略 ...
    fun onClick() {
        val base64Content = readBase64(fileName)
        val sender = ContentSender(this)
        sender.putContent(apiUrl, "master", fileName, base64Content, accessToken)
        finish()
    }
```

このputContentがsuspend関数になったので、これはlaunchの中で呼ばないといけない。
だからこうなる。

```
class MainActivity : AppCompatActivity() , CoroutineScope {
    // ... 中略 ...
    fun onClick() {
        val base64Content = readBase64(fileName)
        val sender = ContentSender(this)
        launch {
            sender.putContent(apiUrl, "master", fileName, base64Content, accessToken)
            finish()
        }
    }
```

このように、呼ぶ側がlaunchをつけるようにしておけば、
そのあとにfinish()をしたい、という場合にも、ただlaunchの中に呼び出したあとにfinish()を置けばいい訳です。

逆にputContentの中にlaunchが置かれてしまうとこのブロックの最後に出す方法はもう無い。
だからこれは間違い、という事になります。

### 自分でsuspend関数を積極的に書こう！

このシリーズの重要なメッセージとしては、suspend関数をAPIとして積極的に外にだそう、という事です。

suspend関数とか良く分からないので、中でlaunchでラップして、外からはsuspend関数が使われているかどうか分からないようにする、というのはだいたいの場合に間違っています。

suspend関数と外からわかるようになっていてはじめてブロック分割のメカニズムが使い手側にも使えるようになるので、クラスのAPIは積極的にsuspend関数として公開するべきです。

そのためにはコード分割のメカニズムやlaunchで何が起こるかをある程度は理解して、
コンパイラが怒ってきたので適当にlaunchつけました、とせずに、ちゃんと設計しましょう。



