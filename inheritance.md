---
title: "クラスの継承"
layout: page
---
ツアーを終えたあとに最低限読んで欲しい所を読んでいくと、継承の所で詰まるはずです。

ここでは[リファレンスの継承](https://karino2.github.io/kotlin-web-site-ja/docs/inheritance.html)を読んでくのに必要な解説を提供します。

## 継承とはどういうものか

まずは継承というのがどういうものか、という解説動画です。

<iframe width="560" height="315" src="https://www.youtube.com/embed/JWYWnsQ9Rko?si=0-3JVVZmmc640nen" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

動画では　`class B: A{}`と書いていましたが、Aのあとにはカッコが必要でした。
以下が正解です。

```kotlin
class B : A() {
  //...
}
```

### overrideの実例

{% capture override-ex1 %}

open class A {
  open fun hoge() {
    println("Aのhoge")
  }

  fun ika() {
    println("Aのikaだ。これからhogeを呼ぶぜ")
    hoge()
    println("Aのikaが終わるぜ")
  }
}

class B : A() {
  override fun hoge() {
    println("Bが横取りしたhogeだぜ")
  }
}

fun main() {
  println("aのika: ")
  val a : A = B()
  a.ika()

  println("---")
  println("")
  println("bのika: ")
  val b = B()
  b.ika()

  println("---")
  println("")
  println("cのika: ")
  val c = A()
  c.ika()
}
{% endcapture %}
{% include kotlin_quote.html body=override-ex1 %}

## ArrayAdapterを継承してみよう

継承を実際に学ぶには使ってみるのが一番！という事で、
[ListViewに挑む！表示編、二回目用](listview_disp_second.md)と同じ内容を、ArrayAdapterを継承する事で書いてみましょう。

以下に取り掛かる前に、上のListViewの表示編を一回復習としてやってから以下をやりましょう。
なお、表示編で必要な作業のいくつかは自分でやる必要がありますがその手順は書きません。
自分で考えて必要と思ったらやってください。

作業の手順は以下です。

- ステップ1: MyAdapter.ktファイルにMyAdapterクラスを作る
- ステップ2: ArrayAdapterを継承する
- ステップ3: MyAdapterのコンストラクタに引数を足し、ArrayAdapterのコンストラクタにも渡す
- ステップ4: MainActivityでMyAdapterをListViewにセットする
- ステップ5: MyAdapterでgetViewをoverrideする

### ステップ1: MyAdapter.ktファイルにMyAdapterクラスを作る

New＞Kotlin Class/FileでClassを選び、名前にMyAdapterと入れてEnter。

### ステップ2: ArrayAdapterを継承する

上のステップで以下のファイルが出来ると思うので、

```kotlin
class MyAdapter {
}
```

これを、

```kotlin
class MyAdapter : ArrayAdapter<String>() {
}
```

として、赤くなったらalt+EnterしてImportする

これでもまだ赤の下線が引かれた状態となると思います。
そこにマウスカーソルをホバーすると、`<init>(うんちゃら)`みたいなのがいっぱい出てくると思います。

これはArrayAdapterのコンストラクタはこれらしか無いのに、引数無しのコンストラクタを呼んでいる、という意味になります。
そこで次にステップでArrayAdapterのコンストラクタをちゃんと呼びます。

### ステップ3: MyAdapterのコンストラクタに引数を足し、ArrayAdapterのコンストラクタにも渡す

ArrayAdapterのコンストラクタはいくつかありますが、我らが使うのは以下のものです。

```kotlin
ArrayAdapter<String>(context: Context, resId: Int, data: List<String>)
```

こういうものを見る時は、パラメータの型に着目します。１つ目が `Context`、２つ目が `Int`、３つ目が `List<String>`です。

これは、ListViewの表示編のコードの、以下のコードに対応します。

```kotlin
val adapter by lazy { object: ArrayAdapter<String>(this, R.layout.list_item, listData) {
```

の、

```kotlin
ArrayAdapter<String>(this, R.layout.list_item, listData)
```

の部分です。先頭がthisとなっていますが、これはActivityを表します、二番目がレイアウトのリソースIDでIntになっていました。
３つ目が`List<String>`ですね。

これと同じ事を、今回もやらなくてはいけません。
イメージとしては以下のような事をやりたいのですが、

```kotlin
class MyAdapter : ArrayAdapter<String>(this, R.layout.list_item, listData) {
}
```

このファイルでは（というか[コードの置き場所入門](code_location_intro.md)の第一区画では）、
`this`とか`listData`は存在しません。

そこでこれをMyAdapterのコンストラクタの引数に追加して、呼び出す側から渡す事にします。
呼び出す方を見ないとここは意味が分からないと思うので、まずは以下のヒントの中をそのまんま書いてください。

二回目からは以下の情報を元に自分で考えてください。

以下の２つのパラメータをコンストラクタに追加する

- `MainActivity`型のパラメータ
- `List<String>`型のパラメータ

そしてそれらをArrayAdapterのコンストラクタ呼び出しに渡します。
[継承 - Kotlin 日本語リファレンス](https://karino2.github.io/kotlin-web-site-ja/docs/inheritance.html)あたりにやり方が書いてあるはずです。
（[クラス - Kotlin 日本語リファレンス](https://karino2.github.io/kotlin-web-site-ja/docs/classes.html)や[ツアー副読本：クラス入門](tour_class_intro.md)あたりも参考になるかも）

{% capture constructor-hint %}
```kotlin
class MyAdapter(activity: MainActivity, listData: List<String>) : ArrayAdapter<String>(activity, R.layout.list_item, listData) {  
}
```
{% endcapture %}
{% include collapse_quote.html body=constructor-hint title="コンストラクタのヒント" %}

### ステップ4: MainActivityでMyAdapterをListViewにセットする

MainActivityの方のonCreateで、以下のようなコードを書く。

```kotlin
findViewById<ListView>(R.id.listView).adapter = MyAdapter(this, listData)
```

listDataは適当に作ってください。

### ステップ5: MyAdapterでgetViewをoverrideする

MyAdapterの中でoverrideと打ってみてgetViewを探して選んでください。

この中身はListViewの表示編と同じ中身にしないといけないのですが、

- layoutInflaterが無い
- listDataが無い

と言われるはずです。

layoutInflaterは、一番簡単なのはactivityから取る事です。

以下のように書いていた所を、

```kotlin
layoutInfalter.inflate(...)
```

以下のように書き直せばいいのですが、

```kotlin
activity.layoutInflater.inflate(...)
```

今度はactivityがさわれん、と言われるはずです。

ここでactivityを触る方法とlistDataを触る方法を考えてみてください。
[ツアー副読本：クラス入門](tour_class_intro.md)にやり方が書いてあります。

{% capture get-view-member-access %}
```kotlin
// 最初のカッコの中にvalをつけるのが正解。以下のようなコードになる
class MyAdapter(val activity: MainActivity, val listData: List<String>) : ArrayAdapter<String>(activity, R.layout.list_item, listData) {  
    override fun getView(position: Int, convertView: View?, parent: ViewGroup): View {

        val view = if(convertView == null) activity.layoutInflater.inflate(R.layout.list_item, null) else convertView

        val data = listData[position]
        view.findViewById<TextView>(R.id.itemLabel).text = data
        return view
    }
}
```
{% endcapture %}
{% include collapse_quote.html body=get-view-member-access title="解答例" %}

以上で作業は終わりました。実行して動く事を確認してください。

### コードを見直す

この作業は、試行錯誤しているとなんとなく出来るが良くわからん、みたいな状態になると思うので、
やった事を見直してみましょう。

たぶん分かりにくいのは以下の二点だと思います

1. MyAdapterのコンストラクタにパラメータを足して、ArrayAdapterのコンストラクタにわたす所
2. valをつける所

一番目に関しては、[継承 - Kotlin 日本語リファレンス](https://karino2.github.io/kotlin-web-site-ja/docs/inheritance.html)の冒頭の
「もし継承したクラスがプライマリコンストラクタをもつなら、 基底の型をプライマリコンストラクタの引数を使用して、プライマリコンストラクタで初期化できる（し、しなければいけません）。」という文の意味とその前のサンプルコードを見比べて良く考えてみてください。
プライマリコンストラクタに関してはリファレンスのクラスの方にも記述がありますが、
ArrayAdapterのプライマリコンストラクタは以下です。

```kotlin
ArrayAdapter<String>(context: Context, resId: Int, data: List<String>)
```

二番目のvalをつける所は、[ツアー副読本：クラス入門](tour_class_intro.md)の該当するあたりと見比べつつ、
その解説の意味する事が実際にどういう感じなのかを理解してもらうといいと思います。

### 何度かこの作業をやってみる

無事最後まで動くものが出来たら、3〜4回くらいこの作業をやってみてください。

### ContextとActivityについて

ArrayAdapterのコンストラクタの第一パラメータはContext型となっています。
ですが、我々はMainActivity型のインスタンスを渡しています。

これは、ContextはActivityの親クラスだからです。

つまり継承の関係としては以下のようになります。

```
Context ＜ Activity ＜ MainActivity
```

継承関係として親の型には子供のインスタンスを渡す事が出来るので、
Contextを要求される所にはいつでもMainActivityのインスタンスを渡す事が出来ます。

なお、細かい事ですが、

```
List<String>　＜ MutableList<String>
```

となっているので、Listを要求する所にはMutableListのインスタンスを渡す事が出来ます。