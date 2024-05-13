---
title: "invalidateで四角を動かそう"
layout: page
---
ここでは、毎フレームちょっとずつ四角を横にずらしていって、パラパラアニメ的なアニメーションを実現する方法を学びます。
とりあえず100msecごとに表示を更新する（秒間10フレーム）、という感じでやってみましょう。

まずは左端から毎フレームごとに5px右に赤い四角が移動していく、という事をやってみましょう。

## Handlerとタイマー関連を思い出す

まず100msecに一回何かをする、というのは、これまでHandlerの所でやりました。
またその後のストップウォッチのあたりは、かなり今回と似た処理になります。

- [Handlerとタイマー](handler_timer.html)
- [ストップウォッチを作ろう](stopwatch.html)

基本的には、

```kotlin
val handler = Handler()
```

としておいて、処理用の関数を作り、

```kotlin
fun tsugiNoSyori() {
  // 各フレームの処理をここに書く、省略

  // 最後にもう一回100msec後に自分を呼ぶ
  handler.postDelay( { tsugiNoSyori() }, 100)  
}
```

最後にonCreateで最初の一回を呼ぶのでした。

```kotlin
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // ここを追加
        handler.postDelayed({ tsugiNoSyori() }, 100)
    }
```

この辺の話題は上にも貼った、[Handlerとタイマー](handler_timer.html)に書いてあります。

という事で、基本的には上で述べたこの`tsugiNoSyori()`という関数の中で四角を少し右にずらして書けば良さそうです。

### カスタムビューでActivityと同じ事をやる時の細かな違い

上記のHandlerとかストップウォッチの話はすべてActivityで処理していました。
一方、今回はすべて同じ処理をカスタムビューでやりたい。

すると、幾つか細かい違いがあります。

1. カスタムビューにはhandlerが既にある
2. 最初の一回を開始していたonCreateに相当するものがカスタムビューにはない

まず１つ目は小さな違いなので簡単に。
Activityでは以下のようなコードを書いて使っていました。

```kotlin
val handler = Handler()
```

ですが、カスタムビューではこれと同じ事を既に親クラスのViewでやっているので、それを継承しているカスタムビューではこのコードを書かずにhandlerという変数がそのまま使えます。
あまり深く考えずに「カスタムビューの場合は上の一行はいらない」と思ってしまって良いです。

２つ目の方がコードに与える影響は大きい問題です（といっても些細な話ですが）。
onCreateがないので、タイマーの最初の一回目をどうやるか、という問題があります。

ぱっと思いつくのは以下の選択肢でしょうか。

1. onDrawで初回だけ呼んでしまう
2. 外（おそらくActivity）から最初の一回は呼んでもらう
3. コンストラクタで呼ぶ
4. タッチされたらスタートする（これは次回以降）

4番目の選択肢はタッチの処理を次回以降にやる予定なので現時点では使えません。
3番目は、これでも構わないとは思いますが、
ちょっとkotlinの継承の知識を多く要求します。

とりあえず現時点では、1か2が良いでしょう。
1が一番簡単ですが、Activityとやりとりするのは他でも使うので、ここでは2を採用する事にします。

だいたい以下のような手順になります。

- カスタムビュー: beginAnimationという関数を作る
- Activity: onCreateでカスタムビューを取り出し、このbeginAnimationを呼ぶ

カスタムビューのコードは以下のような感じになります。

```kotlin
fun tsugiNoSyori() {
  // 各フレームの処理をここに書く、この後に具体的には考える

  // 最後にもう一回100msec後に自分を呼ぶ
  handler.postDelay( { tsugiNoSyori() }, 100)  
}

// 最初の開始の関数を作る
fun beginAnimation() {
  tsugiNoSyori()
}
```

具体的な事は以下で見ていきましょう。

## 基本的な方針と新しさ

さて、100msecごとにnanikaNoSyori関数が呼ばれる所までは良いとします。
やりたい事は、この中で四角を描きたい。

四角を描くのは[はろーCustomView](hello_customview.html)でやった通り、
onDrawをオーバーライドしてcanvas.drawRectというのを呼ぶのでした。

前回のコードを再掲すると以下です。

```kotlin
    override fun onDraw(canvas: Canvas) {
        val r = Rect(10, 10, 200, 200)
        val p = Paint()
        p.color = Color.RED
        p.style = Paint.Style.FILL
        canvas.drawRect(r, p)
    }
```

さて、このRectの所の最初の10というのと、三番目の200というのをちょとずつ増やしていけば右に行くのは良い。

例えばこんな感じです。

```kotlin
    var frame = 0 // tsugiNoSyoriでこのframeを更新する

    override fun onDraw(canvas: Canvas) {
        val r = Rect(10+frame, 10, 200+frame, 200)
        val p = Paint()
        p.color = Color.RED
        p.style = Paint.Style.FILL
        canvas.drawRect(r, p)
    }
```

ここまではいいとして、問題はtsugiNoSyoriで何を書くべきか、という事です。

```kotlin
    fun tsugiNoSyori() {
      // ここに何を書けばいい？


      // 以下は良い。
      handler.postDelay( { tsugiNoSyori() }, 100)  
    }
```

なお、この話は[ストップウォッチを作ろう](stopwatch.html)と似ているので、ここまででさっぱりついてこれない人はストップウォッチの所を復習してみてください。

さて、まずframeを更新するのは良いでしょう。厳密にはHandlerは100msecより遅れる事があるので、ストップウォッチの時と同様、Dateのtimeプロパティを使って計算すべきです。
ですがここでは手抜きとして、単にこの関数が呼ばれる都度1ずつ増やす事にしましょう（Dateを使ってフレームをちゃんと計算するのは課題とします）。


```kotlin
      frame += 1
```

そしてこの後に、onDrawを呼びたい。

```kotlin
    fun tsugiNoSyori() {
      frame += 1
      // ここでonDrawを呼びたい

      handler.postDelay( { tsugiNoSyori() }, 100)  
    }
```

この話はこの説明だけを読んでもたぶん意味が分からないので、一旦ここまでの情報を元に自分でtsugiNoSyoriを書こうとしてみてください。

さて、自分で書こうとした場合、onDrawを呼ぼう、という事になったと思います。
で、`onDraw(`まで書いた所で、キャンバスを渡す必要がある、と気づくはずです。
でもキャンバスなんてどこにもない。どうしたらいいんだろう？
となります。

そこでここでonDrawを呼ぶ何かの仕組みが必要になります。

### onDrawは呼ぶものでは無くて「呼ばせる」もの

onDrawというのは、Androidから「呼ばれる」ものであって、自分で呼ぶ事が出来ない、というのがこの問題の難しさです。

onDrawは、AndroidがViewを画面に描く必要がある時に呼ぶものであって、
自分が呼ぶものではありません。
ですが自分は100msecに一回onDrawを呼びたい。
そこで困ってしまう訳です。

ここは発想の転換が必要な所で、onDrawを100msecに一回呼ぶのでは無く、100msecに一回「AndroidにonDrawを呼んでと頼む」必要があります。

この「AndroidにonDrawを呼んで！」と頼む命令が`invalidate()`です。

### `invalidate()`でonDrawを呼んでもらう

invalidateというのはちょっと難しい英単語ですね。invalidが「無効」みたいな意味で、invalidateで「無効にする」みたいな意味になります。
何がどう無効なのか、みたいなのは最初のうちは少しむずかしいので、
「とりあえずonDrawを呼んで欲しい時は`invalidate()`を呼ぶ、onDrawを直接呼ぶのはなんだか知らんが出来ないらしい」と思っておいてください。

この`invalidate()`を100msecに一回呼ぶと、基本的には100msecに一回onDrawが呼ばれる事になります。
ただ、他の処理がたくさんあるとかで追いつかない時（いわゆる処理落ち）には、onDrawの呼び出しが一回スキップされてしまう場合もあります。

細かい話はおいといて、ひとまずtsugiNoSyoriでこれを呼んでみましょう。
以下みたいなコードです。

```kotlin
    fun tsugiNoSyori() {
      frame += 1
      // ここでonDrawを呼んでね！と頼む
      invalidate()

      handler.postDelay( { tsugiNoSyori() }, 100)  
    }
```

これでtsugiNoSyoriが呼ばれる都度onDrawを呼んでくれます。

{% capture comment1 %}
**onDrawは少し後で呼ばれる**  
invalidateを呼ぶとonDrawが呼ばれる、という話をしましたが、このonDrawはちょとだけ後に呼ばれます。
だから以下のようなコードがあった時に、1の時点ではまだonDrawは呼ばれていません。

```kotlin
    fun tsugiNoSyori() {
      invalidate()
      // 1. この時点ではまだonDrawは呼ばれていない

      handler.postDelay( { tsugiNoSyori() }, 100)  

      // この関数を抜けた後に呼ばれる
    }
```

tsugiNoSyoriが終わった少し後で呼ばれます。
どのくらい後になるかはわかりませんが、この時点で呼ばれない事だけは確実です。
{% endcapture %}
{% include myquote.html body=comment1 %}

### Activityからカスタムビューを取り出してbeginAnimationを呼ぶ

さて、最後にActivityからカスタムビューの関数を呼び出す方法を説明します。

まずxmlにidをつけて、その後onCreateでfindViewByIdで取り出します。

xmlのidはいつもの通りのやり方です。以下では`mycustom`というidをつけています。

```xml
    <io.github.karino2.hellocustomview2.MyCustomView
        android:id="@+id/mycustom"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
```

次にonCreateで取り出すのは以下のような感じです。

```kotlin
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val custom = findViewById<MyCustomView>(R.id.mycustom)
        // ここでcustomのbeginAnimationを呼ぶ
    }
```

さて、コメントに`// ここでcustomのbeginAnimationを呼ぶ`と書いた部分がありますが、
ここで単に`custom.beginAnimation()`を呼ぶと、まだViewの初期化が終わっていないためにhandlerが無くて落ちてしまいます。

そこでViewのpostというのを使うのが良さそうです。

```kotlin
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val custom = findViewById<MyCustomView>(R.id.mycustom)
        custom.post { custom.beginAnimation() }
    }
```

この辺はこういうもの、と深く考えずにコピペしてしのぎましょう。

## 実際に全部を作ってみよう

では以上の知識を用いて、赤い四角が右にちょっとずつ動くアプリを作ってみましょう。
プロジェクトの名前などはどうでもいいですが、とりあえず以下にしましょうか。

- アプリ（Activity）の名前: UgokuSikaku
- カスタムビューの名前: UgokuSikakuView

以下、ヒントとして全体的な手順を簡単に述べます。

1. いつものようにUgokuSikakuというプロジェクトを作る
2. 前回と同じ手順で赤い四角が描かれるまで作業する
   - UgokuSikakuViewというファイルを追加
   - コンストラクタを生成
   - onDrawを生成
   - onDrawの中を書き換える
   - レイアウトのxmlを書き換える
     - ConstraintsLayoutをLinearLayoutに
     - TextViewをUgokuSikakuViewに
     - layout_widthとlayout_heightをmatch_parentに
3. UgokuSikakuViewにtsugiNoSyori関数を追加
   - frameメンバ変数を作る
   - tsugiNoSyoriの中で
     - frameを更新
     - invalidateを呼ぶ
     - 次のtsugiNoSyoriをhandlerのpostDelayで呼び出す
4. UgokuSikakuViewにbeginANimation関数を作る
5. 2で作ったonDrawをframeを使って少し右にずらすように変更
6. レイアウトのxmlでUgokuSikakuViewにidを振って、onCreateからfindViewByIdで取り出してbeginAnimationを呼ぶ

ちょっと難しいですが、ここまで来たらもうこの位でいいでしょう。