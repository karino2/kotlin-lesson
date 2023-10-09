---
title: "ツアー副読本：制御フロー"
layout: page
---

[制御フロー（ツアー） - Kotlin 日本語リファレンス](https://karino2.github.io/kotlin-web-site-ja/docs/kotlin-tour-control-flow.html)を読んでいく時の補助教材です。

## 制御フローのページの大まかなトピック

大きくは以下のような構成になっている。

- 条件分岐
  - if
  - when
- ループ
  - for
  - while

このうちifとforはここまで使ってきたので割と分かると思う（具体的には[JS入門: if文で分岐しよう](https://karino2.github.io/js-introduction/ch04.html)や[forループ入門](for_loop.md)でやってきた）。
whenとwhileが新しい物になるので、それを中心に勉強していくのが正しい姿勢と思うが、
その前に制御フローとか条件式について説明をしてみたい。

## 制御フローとはなんぞや？

制御フローという事を考えるには、文、というものを考えるのがわかりやすい。

### 文

文というのは、プログラムの実行単位で、基本的にはプログラムの１行に相当する。

例えば以下のようなコードがあったとする。

```kotlin
val a = "Hello"
println(a + " world")
println("ほげほげ")
```

この場合、`val a = "Hello"`とか`println(a + " world")`とか`println("ほげほげ")`とかが文に相当する。

行、と言い切れないのは、例えば以下みたいなコードもkotlinでは許されていて、

```kotlin
val a = 3 +
   2
```

この場合はこの二行まとめたものが文となるから。でもだいたいはプログラム的に一行っぽいのを文と思ってだいたい正しい。

### プログラムの実行順序（フロー）

プログラムというのは「基本的には」文が並んでいて、上から順番に1文1文実行していく。
この実行していく流れを実行フローと呼ぶ。
イメージとしては小川の流れのようなものをイメージすると良い。
これに、柵とかを作って小川の流れをわけたり、池の方に流したり、といったような事をしていくのがプログラムと似ている訳だ。

似ているかどうかはおいといて、とにかく文の実行する順番の事をフローという。
プログラムというのは、何も無ければ上から順番に１文１文実行していく。

この順番を変更するのが「制御フロー」だ。

制御フローには大きく分けて

- 条件分岐
- ループ

の２つがある。
以下それぞれ、どの辺がフローを変更するのかを見ていく。

### if文に見る、条件分岐とフロー

例えば以下のようなコードを考えてみよう。

{% capture if-flow-ex1 %}
fun main() {
//sampleStart
  val a = "ほげ"

  if(a == "いか") {
    println("いかだよ！")
  } else {
    println("いかじゃないよ！")
  }
//sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=if-flow-ex1 %}

まず`val a = "ほげ"`が実行される。これは良い。
次にその次の文である`if(a == "いか") { ... }` が実行される。
ここまでは通常のフローになるのだが、
このifというのがその次に実行する文を変更する、という機能を持っている。

フローとしては、以下の２つのフローに分かれる。

**条件がtrueのフロー**

- `val a = "ほげ"`
- `if(a == "いか")`
- `println("いかだよ！")`

**条件がfalseのフロー**

- `val a = "ほげ"`
- `if(a == "いか")`
- `println("いかじゃないよ！")`

このように、上から順番に1文ずつ実行していく、というフローが、ifというものに出会うと２つに分かれるのだ。
これはifの所で川が２つに分かれるのに似ている。

こういう風に流れが分かれるものを川の分岐と同様に、分岐と言う。
ifは条件がtrueかfalseかによって分岐するので、条件分岐、と言う。

### for文に見る、ループとフロー

ifやelseは分かれるだけで、上から下に進む事には変わらない。フローを戻す能力は条件分岐には無い。
前に戻るのはループ、と呼ばれる制御フローだ。
ループの例でおなじみなのはfor文になる。

以下のような例を考えてみよう。

{% capture flow-for-1 %}
fun main() {
//sampleStart
  val list = listOf("いち", "にー", "さん")
  println("ループの前")
  for(l in list) {
    println("ほげほげ：")
    println(l)
  }
  println("おしまい")
//sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=flow-for-1 %}

実行の順番を考えると、以下までは一つずつ普通に実行される。

- `val list = listOf("いち", "にー", "さん")`
- `println("ループの前")`
- `for(l in list) { `

要素がある場合は、その下もそのまま同じように実行される。

- `println("ほげほげ：")`
- `println(l)`

さて、この後通常のフローなら`println("おしまい")`が実行される所だが、
for文はこのフローを変更して、次に実行されるのをforの先頭まで戻す。

だから`println(l)`の次に実行されるのは`for(l in list) {`になるのだ。
実行順序が前に戻る。
この実行の順序が上に戻る、というのがループという構造の特徴になる。

ただ、戻るにせよ分岐するにせよ、まっすぐ次の文に進む訳じゃない、
という点で、条件分岐とループは仲間と思われていて、
このように実行順序を変更するもの全体を「制御フロー」と呼んでいる。

### whenとwhileについて考える

先程の分類をもう一度見てみよう。

- 条件分岐
  - if
  - when
- ループ
  - for
  - while

whenとwhileというのが何かについては後で見ていくが、この分類だけで分かる事もある。

whenというのは条件分岐の一種なので、実行の流れが複数に分かれて、そのうちのどれかを通る、というものだ。

whileというのはループの一種なので、実行を進めていくとどこかで次に実行する文が前に戻るような構造となっている、という事だ。

こういう大分類をまずは理解したい。

## 条件式って何？

次に条件式について見ていきたい。

for文はちょっと違うのだけれど、ifとwhenとwhileは条件式というものを使う所に共通する部分がある。
わかりやすいのでif文を中心に考えよう。

ifというのは、条件にしたがってどちらを実行するか分岐するものだった。
具体的には、以下のコードがあった時に、

```kotlin
if(a == "いか") {
  println("いかだよ!")
} else {
  println("いかじゃないよ！")
}
```

ifの条件の所、つまり以下の`if(a == "いか") {`のうちの`a == "いか"`の部分が、
ifが分岐する条件になっている。

この `a == "いか"` の部分の事を条件式という。

式というのは、実行をすると値がかえってくるコードの一部分の事をそう言うが、
if文の場合は実行をするとBooleanが返ってくる式が条件式となる。
whileも同様。

けれどwhenは条件式はBooleanである必要は無い。IntとかStringでも良い。
けれどとにかく、分岐をする為の条件というのがあって、
その条件を表すのは式になっている。

具体的には以下のようなコードがあった時に、

{% capture when-cond-1 %}

fun main() {
//sampleStart
  val obj = "ほげ"

  when (obj) {
      "いか" -> println("いかだよ！")
      "ほげ" -> println("ほげだよ！")
      else -> println("なんかしらんの来た…")     
  }
//sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=when-cond-1 %}

条件式、というのは`when(obj) {` の`obj` の所の事です。
今回は変数一つなので式って感じにも見えないけれど、ここには別にもっと違うものが入ってもいい。
例えば以下みたいなコードがあった時：

{% capture when-cond-2 %}

fun main() {
//sampleStart
  val obj = "ほげ"

  when (obj.length+3) {
      3 -> println("合計3！")
      5 -> println("合計５！")
      else -> println("それ以外！")     
  }
//sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=when-cond-2 %}

条件式というのはwhenのカッコの中、つまり、`obj.length+3`になります。

ちなみにwhileで以下みたいなコードがあったら、

{% capture while-cond-1 %}

fun main() {
//sampleStart
  var kurikaesi = 0

  while(kurikaesi < 3) {
    println("大事な事なので三回言います")
    kurikaesi++
  }
//sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=while-cond-1 %}

条件式というのは、ループの条件として使う式で、この場合は`kurikaesi < 3`になります。

以上が条件式というものです。

## ifを式として使うか文として使うかの話

ツアーのif文の所に以下のような説明があり、突然三項演算子とかいう言葉が出てきます。

> Kotlinには三項演算子、condition ? then : elseがありません（訳注：C言語などの言語にはあるがKotlinには無い機能）
>
> 三項演算子の代わりに、Kotlinはifを式として使う事が出来ます。 ifを式として使う時は、中括弧{}がありません：（訳注：中括弧なしでも構わない、という意味だと思う）

この三項演算子という言葉に動揺して本当に伝えたい所がわかりにくいので注意が必要です。
この文で本当に伝えたいのは以下の所です。

> Kotlinはifを式として使う事が出来ます。

式としても使える、というのが重要な所です。では式として使えるとはどういう意味か？というのを説明します。
これは以下のwhenなどでも同様なので、ifでしっかりと理解しておくのが良いでしょう。

まず、式というのはそれを評価すると値が返ってくるコードの部分の事です。
値が返ってくる、というのは、「変数作る時のイコールの右に置けるもの」と考えると良いでしょう。
これは関数の引数に使う事が出来るという事でもあります。

なお、式を実行して結果を得る事を**評価する**と言います。プログラム用語です。

式として使える、というのは、ツアーのページで例にあげてあるコードで良いでしょう。

{% capture if-expression %}
fun main() {
//sampleStart
    val a = 1
    val b = 2

    println(if (a > b) a else b) // 結果の値: 2
//sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=if-expression %}

pirntlnの引数にして使っている例ですが、イコールの右に置く例に変更してみましょう。

{% capture if-expression-2 %}
fun main() {
//sampleStart
    val a = 1
    val b = 2

    val c = if (a > b) a else b
    println(c)
//sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=if-expression-2 %}

このように、ifはイコールの右に置いても使える、というのが言いたい事です。
これはもう少し掘り下げて観てみる価値のある所なので、もうちょっと掘り下げてみたいと思います。

### 式として「では無く」使う例

式として使える、というだけだと、何が式では何のかが少しわかりにくいと思うので、
ここで式では無い例（専門用語では「文として使う」という事になる）を見てみます。

以下の例は式では無い例です。

{% capture if-statement-1 %}
fun main() {
//sampleStart
    val a = 1
    val b = 2

    if (a > b)
      println("aの方が大きい！")
//sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=if-statement-1 %}

さて、以下の部分を考えてみましょう。

```kotlin
    if (a > b)
      println("aの方が大きい！")
```

これをイコールの右に置けるか？という事を考えます。

そもそもprintlnはイコールの右に置けません。
これまでもprintlnをイコールの右に置いた例は無いはずです。
なぜならprintlnは文字を出力する、という動作をするだけで、値は返さないからです。

さらにprintlnが値を返さないだけでは無く、そもそもifが成立しない場合、つまりaの方が小さい場合は、
このコード片が値を返さないのは明らかでしょう。

このように、ifの片方しか値を返さない場合などは、全体としては値を返さない、と解釈されます。

まとめると、ifが文の例というと、

- printlnを実行するだけのもの
- elseが無いもの

などが考えられます。

### 返す値は最後の式

ifを式として使う、というのは、評価をすると値になるものです。
では、以下の式の値は何になるでしょうか？

{% capture if-expression-3 %}
fun main() {
//sampleStart
    val a = 1
    val b = 2

    val c = if (a > b){ 
              println("aの方が大きい！")
              a
            } else{ 
              println("bの方が大きい！")
              b
            }
    println(c)
//sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=if-expression-3 %}

この場合、bの方が大きいので、elseが実行されます。
elseは以下のようになっています。

```kotlin
  else{ 
    println("bの方が大きい！")
    b
  }
```

printlnは値を返さないと言っていたのにイコールの右で使ってる！と思うかもしれませんが、
それは間違いではありません。

elseを評価した結果は、「一番最後の式の値」となります。
つまり、printlnの結果は値無しなのですが、その次に評価したbがelseの結果になるのです。

ifも同様で、ifの後の中括弧で書かれた、一番最後の式の値がifの値となります。
この中括弧で囲われた範囲、というのを「ブロック」と言います。これもプログラム用語です。

式として使う時には、どのif-else if -elseのブロックも、最後は値を返す式を置く必要があります。
これがifを式として使う為の条件でもあります。

「ifの式としての値は、各ブロックの最後の式の値」

### ifを式として使うまとめ

- 全部の分岐で値を返さないといけない
- printlnなどは値を返さないのでダメ
- 複数の行がブロックに入っている時は最後の行の結果がifの結果

ここが理解出来ているか不安だったら、「式では無い例はどんなだろう？」と考えて分かればOKです。

## 範囲、downToとかstepとか

範囲はこれまでも使ってきたので割と分かると思いますが、
なんか知らないのも幾つか出ています。
難しくは無いけれど、例が無いと分からないかもしれないので例を貼っておきます。

{% capture range-downto %}
fun main() {
//sampleStart
    for (i in 4 downTo 1) println(i)
//sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=range-downto %}

stepの例も貼っておきます。

{% capture range-step %}
fun main() {
//sampleStart
    for (i in 0..8 step 2) println(i)
    println("---")
    for (i in 0..<8 step 2) println(i)
    println("---")
    for (i in 8 downTo 0 step 2) println(i)
//sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=range-step %}

こういうこれまで出てきてない事がさらっと解説されて先に進んでしまっていて、
なんか良く分からないな、と思ったら、
実際にこうして自分で使ってみるのがオススメです。

## forの所にあるイテレータってなに？

forなんてこれまで使ってきたから分かるぜ、と思いながら読むと、
以下のような説明があります。

> カッコ　()　の中にイテレータかrangeを、キーワードの　in　とともに置きます。 そして行いたいアクションを中括弧 {} の中に置きます：

ん？イテレータって何？ってなって全然分からん、とかなってしまう人も居るかもしれないので、簡単に説明しておきます。
まず、このイテレータとrangeはなんの事かというと、
forの以下の「ここ！！！」と書いた所の話です。

```kotlin
for(a in 「ここ！！！」) {
  println(a)
}
```

inの右側には、rangeかイテレータのどちらかを置ける、と言っている。
そしてこのイテレータというのは、現時点では「コレクションの事」と思っておいてOKです。
具体的にはリストかセットかマップの事です。

イテレータというのはそれ以外も含むし、そもそも厳密にはここに置けるのはイテレータでは無く`iterator()`という関数を持った何かでこれがイテレータを返すのでこの説明自体が厳密には誤りなので、
この段階では「イテレータとはコレクションのことだ」と思っておいてOKです。

元の解説はようするに、「for文ではカッコのinの右にrangeとかコレクションを置く」という意味という事が分かればOKです。

## `while`文

`while`は簡単で大した話じゃないのだけれど、突然do-whileと混ぜて説明したりとわかりにくくなっているので、
ここで説明しておく。

まず`while`文と`do-while`文という２つのものがあって、この２つは分けて考えた方がいいでしょう。
という事で最初に`while`文の方を考えます。

### `while`の例

最初は例を見る所から始めます。

{% capture while-ex1 %}
fun main() {
//sampleStart
  var kurikaesi = 0

  while(kurikaesi < 3) {
    println("大事な事なので三回言います")
    kurikaesi++
  }
//sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=while-ex1 %}

whileはこのように使います。
だいたい以下のようになっている。

```
while( 「条件式」 ) {
  「コードブロック」
}
```

whileは後ろにカッコをつけて条件式を置く。
そしてその後ろに中括弧を置いて、その後にコードブロックを置きます。

先程の例に当てはめて考えると、

```kotlin
  while(kurikaesi < 3) {
    println("大事な事なので三回言います")
    kurikaesi++
  }
```

条件式は `kurikaesi < 3` の事です。

コードブロックは

```kotlin
    println("大事な事なので三回言います")
    kurikaesi++
```

の事です。

このように、どういう風に書くか、という事のルールを「シンタックス」と言います。
最初のうちは何が「シンタックス」で何が「シンタックスで無い」かの区別が分からないと思うけれど、
説明で使っているのを見ていくうちにだんだんと「なるほど、これがシンタックスか」と分かっていくと思うので、
以後は「シンタックス」という言葉を使っていきます。

### コードブロックって何？

コードブロックというのは、ひとまとまりのコードの事で、
この例では中括弧で囲まれた範囲のコードの事です。

ifの中括弧の中もコードブロックだし、
setOnClickListenerの後の中括弧の中もコードブロックです。

まぁそんなに難しく考えないで「whileの後の中括弧の中をコードブロックと呼ぶ」くらいで良いでしょう。

### インクリメント演算子って何？

唐突に以下のような説明が出てきます。

> 以下の例では インクリメント演算子 ++ を、 cakesEaten変数の値をインクリメントするのに使っています。

`++`というのをインクリメント演算子と言うらしいですが、そのさらに先のリンク先を見ても意味が分からず、そしてインクリメントが何かについては説明がありません。
「インクリメント演算子はインクリメントをするものです」とか説明になってないやん、とか思う人も居るかもしれない。
という事でここで簡単に説明しておきます。

インクリメントとは「1増やす事」です。デクリメントは「1減らす事」です。
`++`は変数名の後に置くと、その変数を1増やします。

以下のコードは

```kotlin
a++
```

以下のコードと同じ意味です。

```kotlin
a += 1
```

増やした結果をaに代入するので、aは`var`じゃないといけません。`val`では使えません。

以下例を見てみましょう。

{% capture increment-ex1 %}
fun main() {
//sampleStart
  var a = 0
  println(a)
  a++
  println(a)
  a++
  println(a)
  a++
  println(a)
//sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=increment-ex1 %}

また、`a++`は式としては一つ増やす「前の」値を返します。

{% capture increment-ex1 %}
fun main() {
//sampleStart
  var a = 0
  println(a++)
  println(a)
  println("---")
  println(a++)
  println(a)
  println("---")
  println(a++)
  println(a)
//sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=increment-ex1 %}

- aが1の時に`a++`を実行すると、実行結果は1でaに2が入ります。
- aが2の時に`a++`を実行すると、実行結果は2でaに3が入ります。

この辺はややこしくなるので、あまり使わないで`a += 1`を使っている方が無難かもしれません。

### whileの動作

whileは

- ステップ1： 条件式を評価する
  - true: ならステップ1へ
  - false:ならステップ2へ
- ステップ2: コードブロックを実行しステップ1に戻る
- ステップ3: whileを終了し、whileの次の文に実行を移す

という挙動をします。

以下の例を考えます。

{% capture while-ex2 %}
fun main() {
//sampleStart
  var kurikaesi = 0

  while(kurikaesi < 3) {
    println("大事な事なので三回言います")
    kurikaesi++
  }

  println("whileの次の文")
//sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=while-ex2 %}

先程の3の、whileの次の文、というのは、`println("whileの次の文")`に相当します。

この式を、先程の3ステップを繰り返してどう動くのかを考えてみます。

1. ステップ1: 条件式を評価する。kurikaesiが0なので、 `0 < 3`はtrueを返すのでステップ2に行く
2. ステップ2: コードブロックを実行してステップ1に戻る。コードブロックはprintlnをして`kurikaesi++`する。printlnして、kurikaesは1になる。
3. ステップ1の二回目: 条件式を評価する。kurikaesが1なので、`1 < 3` はtrueを返すのでステップ2に行く
4. ステップ2の二回目: printlnをして、`kurikaesi++`する。kurikaesiは2になる。ステップ1に戻る。
5. ステップ1の三回目: 条件式を評価する。kurikaesiが2なので、`2 < 3`はtrueを返すのでステップ2に行く
6. ステップ2の三回目：printlnをして、`kurikaesi++`する。kurikaesiは3になる。ステップ1に戻る。
7. ステップ1の四回目：条件式を評価する。kurikaesiが3なので、`3 < 3`はfalseを返すので、ステップ3に行く
8. ステップ3の一回目：whileを終了し、次の文に実行を移す。この場合は`println("whileの次の文")`を実行する

このようになります。

**課題：上の8行の箇条書きを自分でコードを見ながら三回書け**

ステップ1からステップ3までの説明と上のサンプルコードの両方を見て、
実際にどう動くか（上に書いた8行の箇条書き）を紙とペンで自分で一つずつ書け。

三回くらいやりましょう。

## do-while文

whileを理解するとdo-whileは割と簡単です。

### do-whileの例

まずは例を見ます。

{% capture dowhile-ex1 %}
fun main() {
//sampleStart
  var kurikaesi = 0

  do {
    println("大事な事なので三回言います")
    kurikaesi++
  } while(kurikaesi < 3)

  println("do-whileの次の文")
//sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=dowhile-ex1 %}

一緒じゃん、と思うかもしれないけれど、最初の変数が3の時の挙動が違う。

{% capture dowhile-ex2 %}
fun main() {
//sampleStart
  var kurikaesi = 3

  do {
    println("do-whileは一回実行される")
    kurikaesi++
  } while(kurikaesi < 3)

  kurikaesi = 3
  while(kurikaesi < 3) {
    println("whileは実行されない")
    kurikaesi++
  }

  println("最後の文")
//sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=dowhile-ex2 %}

ちなみにkurikaesiを最初から4にしても結果は一緒です（自分で試してみよう）。

### do-whileのシンタックス

do-whileのシンタックスは以下になります。

```
do {
  「コードブロック」
} while( 「条件式」 )
```

先程の例に当てはめると、

```kotlin
  do {
    println("do-whileは一回実行される")
    kurikaesi++
  } while(kurikaesi < 3)
```

このうち、コードブロックは

```kotlin
    println("do-whileは一回実行される")
    kurikaesi++
```

となり、条件式は`while(kurikaesi < 3)`の中の`kurikaesi < 3`の部分になります。

### do-whileの動作

do-whileは以下のように動作します。

- ステップ1： コードブロックを実行する
- ステップ2：条件式を評価する。
  - trueならステップ1に戻る
  - falseならステップ3に進む
- ステップ3: do-while文を終了して次の文に実行を移す

whileとの違いは、条件式の評価をコードブロックを実行した「後に」行う事です。
条件式に何が書かれていようと最初のステップ1は必ず実行されます。

do-whileはあまり使わないので、そういうのがある、という事を知っておいて使う時が来たら見直せば十分でしょう。
whileだけちゃんと理解しておけばOK。