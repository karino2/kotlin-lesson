---
title: "Handlerとタイマー"
layout: page
---
今回はHandlerと、それを使ったタイマー処理を行います。

以下では、HelloIchiNiSanというプロジェクトで、label1というTextViewが一つあるとします。

## Handlerとその使い方

タイマー処理にはHandlerというものを使います。

まずメンバ変数に以下のように定義します。

```kotlin
val handler = Handler()
```

Handlerが赤くなった時のimportは二つ選択肢があると思いますが、android.osの方を選びます。

そうするとHandler()の所に打ち消し線が出ると思いますが気にしないでください。（`Handler()`の代わりに`Handler(Looper.getMainLooper())`とすると消せます、気になる人はこっちを使ってもOK）

そしてタイマー処理するのは、以下のようにpostDelayedを使います。

```kottlin
handler.postDelayed({ /* ここに何か書く */ }, 5000)
```

postDelayedの１つ目の引数は時間が来た時に実行する処理を、２つ目の引数が、何msec秒後かを書きます。
msecなので5000msecで5秒です。100で0.1秒。
なお、ぴったりその時間後に呼んでくれる訳ではなく、ちょっとズレたりします。

処理を書くのは最初の引数の中のカッコですが、だいたいエンターキーで改行を入れる方がいいでしょう。以下みたいになります。

```kottlin
handler.postDelayed({
      /* ここに何か書く */
}, 5000)
```

例えばいつものようにonCreate（いつもsetOnClickListenerとか書いている所）に以下を書いてみましょう。

```kotlin
handler.postDelayed({
    findViewById<TextView>(R.id.label1).text = "ごー"
}, 5000)
```

こうすると、アプリが起動した5秒後に、"ごー"と出ます。

### 二回タイマーを仕掛けるには？

タイマーの最後に次のタイマーを仕掛けると、二回目のタイマーを実行出来ます。

```kotlin
handler.postDelayed({
    findViewById<TextView>(R.id.label1).text = "いーち"
    handler.postDelayed({ /* ここに二回目の処理 */ }, 1000)
}, 1000)
```

二回目の処理の所にも改行を入れると以下のようになる。

```kotlin
handler.postDelayed({
    findViewById<TextView>(R.id.label1).text = "いーち"
    handler.postDelayed({
       /* ここに二回目の処理 */
    }, 1000)
}, 1000)
```

2秒後に「にー」と表示するには以下。

```kotlin
handler.postDelayed({
    findViewById<TextView>(R.id.label1).text = "いーち"
    handler.postDelayed({
      findViewById<TextView>(R.id.label1).text = "にー"
    }, 1000)
}, 1000)
```

3秒後に「さーん」とするにはさらに以下になります。

```kotlin
handler.postDelayed({
    findViewById<TextView>(R.id.label1).text = "いーち"
    handler.postDelayed({
      findViewById<TextView>(R.id.label1).text = "にー"
      handler.postDelayed({
        findViewById<TextView>(R.id.label1).text = "さーん"
      }, 1000)
    }, 1000)
}, 1000)
```

こういうコードは読むのは難しいですね。書く方は毎回以下みたいにしたあとに、

```kotlin
  findViewById<TextView>(R.id.label1).text = "ほげ"
  handler.postDelayed({}, 1000)
```

中括弧の所でEnter押すだけなので見た目よりも簡単なのですが、読むのが辛い。

こういう風になんちゃらの中になんちゃらがあって、その中にさらになんちゃらがあって〜というのを、「ネスト」といいます。
入れ子の構造ですね。
この階層が深い事を「ネストが深い」などと言います。

ネストが深いコードは読みづらいのでどうにかした方がいいのですが、その前に少し練習問題をやってみましょう。

### 課題： 「いーち、にー、さーん、ダー！」せよ

1. 1秒後に「いーち」
2. 2秒後に「にー」
3. 3秒後に「さーん」
4. 4秒後に「ダー！」

と表示せよ。

## 関数を使ってネストを減らす

関数を使ってネストを減らすように工夫するのが普通です。
どう工夫するかは問題ごとに考える必要がありますが、
まずは今回の以下のコードをどうするかを見ていきます。

```kotlin
handler.postDelayed({
    findViewById<TextView>(R.id.label1).text = "いーち"
    handler.postDelayed({
      findViewById<TextView>(R.id.label1).text = "にー"
      handler.postDelayed({
        findViewById<TextView>(R.id.label1).text = "さーん"
      }, 1000)
    }, 1000)
}, 1000)
```

これは、「1秒ごとに次のメッセージを表示する」という事をやっているので、そういう関数を作れば良い事になる。

具体的にはメンバ変数に「現在何番目か」を表す変数を用意して、タイマーごとにその変数を一つ進めてメッセージを表示すれば良い。
イメージとしてはこんな感じ。

メンバ変数として以下を定義して、

```
val messages = listOf("いーち", "にー", "さーん")
var index = 0
```

関数、次のラベルという事でtsuginoLabelという関数を作るとすると、だいたい以下みたいな感じ

```
fun tsuginoLabel() {
  val text = messages[index]
  findViewById<TextView>(R.id.label1).text = text
  index += 1 // 次に進める
  handler.postDelayed( { tsuginoLabel() }, 1000)
}
```

これだとindexが3になった1秒後にクラッシュしてしまいますが、それをどう直すかは課題としましょう。

まずはこのコードを理解します。関数の中は以下のようになっています。

```kotlin
  val text = messages[index]
  findViewById<TextView>(R.id.label1).text = text
  index += 1 // 次に進める
  handler.postDelayed( { tsuginoLabel() }, 1000)
```

最初の行はindex番目のメッセージを取っています。そしてそれを二行目でセットする。ここまではいいでしょう。

三行目はちょっと新しいですね。

```kotlin
index += 1
```

`+=` は文字列の連結でやったと思いますが、以下と同じ意味になるのでした。

```kotlin
index = index + 1
```

つまり、indexを現在の値より1増やす、という意味になります。
indexが0なら1になり、1だったら2になります。

最後の行は以下です。

```kotlin
  handler.postDelayed( { tsuginoLabel() }, 1000)
```

これで1秒後に`tsuginoLabel()`を実行します。関数を呼ぶだけとかなら、わざわざ中括弧で改行しないでもいいでしょう。

tsuginoLabelを実行すると、1秒後にtsuginoLabelを実行します。
この1秒後のtsuginoLabelを実行すると、そのさらに1秒後にtsuginoLabelを実行します。
これは、このままでは永遠に続いてしまいます。（そして4回目にはmessagesの存在しない要素を取り出そうとしてクラッシュする）

「さーん」を表示した所で止めるには、この最後のpostDelayedを3回目のtsuginoLabelでは呼ばないようにすれば良い訳です。

ただその課題に行く前に、最後にこの関数をonCreateで呼ぶようにします。

```kotlin
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // ここを追加
        handler.postDelayed({ tsuginoLabel() }, 1000)
    }
```

これで大枠は完成です。4秒後にクラッシュするのを確認して、次の課題をやりましょう。

### 課題：4秒後に落ちるのを直せ

indexが3になったらhandler.postDelayedを呼ばなければ良いので、if文でどうにかしてください。

### 課題：Retryボタンをつけろ

「さーん」まで行ったらRetry出来るRetryボタンをつけてください。

なお、handlerのタイマーはキャンセルとかは出来ません。
キャンセルはいろいろと難しいので、なるべく「キャンセルが必要無い」ように振る舞いを決めるのがオススメです。

という事でRetryボタンもenabledをfalseにしておいて、「さーん」まで行ったらenabledにしましょう。

### 課題：繰り返し「いーち」、「にー」、「さーん」、「いーち」、「にー」…と表示し続けるアプリを作れ

indexを0に戻してもいいし、`index % 3`を使ってもいいです。
どちらの解き方も分かるのが理想ですね。
