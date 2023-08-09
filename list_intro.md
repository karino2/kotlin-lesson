---
title: "List入門"
layout: page
---
for文が使えるといろいろと出来る事が増えるので、ここでListをもうちょっと詳しく見ていきます。

ここでは以下のメソッドなどを見ていきます。

- list.size
- list.last()
- list.withIndex()

また、MutableListにも最後に軽く触れます。

{% capture comment1 %}
**メソッドって何よ？**  
メソッドという言葉が唐突に出てきたかもしれないけれど、これはクラスというものを説明しないと何なのか説明出来ないため、現時点ではメソッドとは何かを説明する事は出来ません。
ですが、呼び名が無いと説明が出来ないので「メソッド」という名前は出させてもらってます。

何か、というと、`.`をつけた後に呼ぶ奴、くらいに覚えてもらっておくと良いです。
厳密にはsizeなどはメソッドでは無くてプロパティなのですが、そもそもメソッドとは何かを説明してないので、メソッドじゃないとか言っても知らんがな、って感じと思います。
という事でここでは`.`の後に続く呼びだす系のものをふわっと「メソッド」と呼ぶ事にします。
{% endcapture %}
{% include myquote.html body=comment1 %}

## 個数は.sizeで取る

リストの個数は.sizeで取れます。

{% capture size_code %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  println(youbi.size)
}
{% endcapture %}
{% include kotlin_quote.html body=size_code %}

個数は7だけど、`youbi[7]`は最後「の次」の要素にアクセスしようとしてエラーになるので注意。`youbi[6]`までしかアクセス出来ない。
つまり以下のようなコードはエラー。

{% capture size_code2 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  // エラー
  val hoge = youbi[youbi.size]
  println(hoge)
}
{% endcapture %}
{% include kotlin_quote.html body=size_code2 %}

`youbi.size`は7なので、`youbi[youbi.size]`は`youbi[7]`になってしまって、範囲外アクセスとなってしまう。
youbiの最後の要素は`yougi[6]`なので、1を引く以下のコードは動く。

{% capture size_code3 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  // これはオーケー。最後の要素。
  val hoge = youbi[youbi.size-1]
  println(hoge)
}
{% endcapture %}
{% include kotlin_quote.html body=size_code3 %}

ややこしいですね。
なお、最後の要素を取り出すのに1を引くのを忘れるバグが良くあるので、最後の要素を取る専用の手段、`.last()`というのがあります。

{% capture size_code4 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  val hoge = youbi.last()
  println(hoge)
}
{% endcapture %}
{% include kotlin_quote.html body=size_code4 %}

こちらの方がミスは少ないね。

## 添字をRangeで回す

以下のようなものがあったとします。

{% capture index_code1 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in youbi) {
    println(a)
  }
}
{% endcapture %}
{% include kotlin_quote.html body=index_code1 %}

ここで、この出力を「月曜、水曜、金曜、日曜」と一日おきにするにはどうしたらいいかを考えます。

もちろんここまでの知識を使って、Booleanのフラグを使って以下のように書けば書けそうですが

{% capture index_code2 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")
  var toggle = true

  for(a in youbi) {
    if(toggle)
      println(a)
    toggle = !toggle
  }
}
{% endcapture %}
{% include kotlin_quote.html body=index_code2 %}

これはちょっとややこしい。
あれ？`!toggle`は初めてかな？ビックリマークは反転させる、という機能で、trueがfalse、falseがtrueになります。
まぁいい。

こういうのはそれよりも、添字で回す方が素直に書けます。

[forループ入門](for_loop.md)でやったRangeとsizeを使えば以下のように書けます。

{% capture index_code2_1 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in 0..<youbi.size) {
    if(a%2 == 0)
      println(youbi[a])
  }
}
{% endcapture %}
{% include kotlin_quote.html body=index_code2_1 %}

`..<`は、大きい方の境界を含まない、というrangeでした。これで0, 1, 2, 3, 4, 5, 6が順番にaに入ります。

これと同じ事をするindicesというのもありますが、入門時は覚える事を少なくして知っているものを組み合わせる練習をした方がいいと思うのでここでは扱いません。
という事で、いくつか課題をやってみましょう。

**課題: 何番目の要素の値がいくつか、と出力せよ**

以下のように出力せよ

- 0番目の要素は「月曜」
- 1番目の要素は「火曜」
- ...中略...
- 6番目の要素は「日曜」

{% capture index_code3 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  // TODO: 以下を書け
}
{% endcapture %}
{% include kotlin_quote.html body=index_code3 %}


**課題: 以下のコードを変更し、曜日を2個飛ばしに出力せよ**

曜日を二つ飛ばし、つまり月曜、木曜、日曜と出力されるようにしてください。

{% capture index_code6 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  // TODO: 以下を変更
}
{% endcapture %}
{% include kotlin_quote.html body=index_code6 %}

### 逆順にしてみる

インデックスの一覧を回すのを応用すると、逆順に出力したりも出来ます。

{% capture revindex_code1 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in 0..<youbi.size) {
    println(youbi[6-a])
  }
}
{% endcapture %}
{% include kotlin_quote.html body=revindex_code1 %}

6から引けばいい。6というのはyoubiの個数-1です。なんで1引くかというと0から始まるから。

リストの個数はsizeで取れるので、以下のようにも書ける。

{% capture revindex_code2 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in  0..<youbi.size) {
    println(youbi[youbi.size-1-a])
  }
}
{% endcapture %}
{% include kotlin_quote.html body=revindex_code2 %}

ちょっとややこしいですね。
最初はわかりにくくても、こういう`-1`は結構出てくるので、何度か見ると慣れると思います。

少し読みにくいので変数を作ってもいいかもしれない。

{% capture revindex_code3 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in  0..<youbi.size) {
    val revIndex = youbi.size-1-a 
    println(youbi[revIndex])
  }
}
{% endcapture %}
{% include kotlin_quote.html body=revindex_code3 %}

少し練習問題をやってみましょう。

**課題: 日曜、金曜、水曜...と逆順の一つ飛ばしでprintlnせよ**

以下のコードを変更して、逆順でさらに一つ飛ばしになるようにしてみましょう。
一つ飛ばしは[forループ入門](for_loop.md)を参考に。

{% capture revindex_code4 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in  0..<youbi.size) {
    // TODO: 以下を修正せよ
    println(a)
  }
}
{% endcapture %}
{% include kotlin_quote.html body=revindex_code4 %}

## 添字と要素を同時に取る、withIndex()

添字を取る時はだいたい要素も使うので、最初から両方渡ってくる方が便利な事が多い。
ということで両方渡ってくる`withIndex()`というのはなかなか便利です。
というかsizeでrangeを手書きする機会はほとんどなく、だいたい`withIndex()`を使う。

{% capture withindex_code1 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for((index, elem) in youbi.withIndex()) {
    println("${index}番目の曜日は${elem}です");
  }
}
{% endcapture %}
{% include kotlin_quote.html body=withindex_code1 %}

for文で二つのものを受け取る例はこれが初めてですね。少し注意してみてみましょう。

二つのものを受け取る場合はカッコでくくってカンマで区切ります。

```
for((変数1, 変数2) in コレクション) {

}
```

二つのものを返すコレクションはwithIndex()以外にもあるので、とりあえず上のように覚えておくと良いでしょう。

**課題: withIndex()を使って偶数の曜日だけprintlnせよ**

月、水、金、日曜が出力されれば正解です。

{% capture withindex_code2 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")
}
{% endcapture %}
{% include kotlin_quote.html body=withindex_code2 %}

### 電卓の課題をfor文化出来ないか考えてみる

[簡単な電卓を作ってみよう](simple_calc.md)では、ボタンの0から9までが、ほとんど同じ処理になると思います。
違うのは数字とボタンのidだけ。

こういうのはうまい事for文に出来そうです。

ようするに以下のようなのがあれば、

```kotlin
val labels = listOf(R.id.button0, R.id.button1, R.id.button2, R.id.button3, R.id.button4, R.id.button5, R.id.button6, R.id.button7, R.id.button8, R.id.button9)
```

これをwithIndexを使って、何番目のボタンか、という事と合わせてどうにかなりそう？

```kotlin
for((num, buttonId) in labels.withIndex()) {
  findViewById<Button>(buttonId).setOnClickListener { 
    findViewById<TextView>(R.id.result).text += num.ToString()
  }
}
```

ちょと動くか自信無いので試して動いたら教えてください。

これはたまたまボタンが0から始まっていてちょうどインデックスと一致させられるように並べられるからですが、
ボタン0はちょっと他と違うので特別扱いしたいかもしれない。

そういう風にindexとずれちゃう場合はちょっと小細工がいるかも。
例えば以下みたいな感じか？

```kotlin
val labels = listOf(R.id.button1, R.id.button2, R.id.button3, R.id.button4, R.id.button5, R.id.button6, R.id.button7, R.id.button8, R.id.button9)

for((index, buttonId) in labels.withIndex()) {
  val num = index+1
  findViewById<Button>(buttonId).setOnClickListener { 
    findViewById<TextView>(R.id.result).text += num.ToString()
  }
}
```

idと対応するのが数字じゃない時はPairなどを使う必要がありますが、そういうのはもっと先に進んだ後にやります。

## 追加をしたい時はMutableList

kotlinのコレクションは、Mutableなものとそうでないものの二つがある。

- ListとMutableList
- MapとMutableMap

MutableXXは要素の追加や削除などの変更が出来る、というところ以外はMutableの無いXXと同じ。
つまりMutableListは要素の追加と削除が出来る以外はListと同じ。

以下ではMutableListについて見ていく。

### mutableListOfとaddとclear

MutableListを作るのはmutableListOfで、listOfと同じように、以下のように使える。

```kotlin
mutableListOf("abc", "def", "ghi")
```

なのだけれど、わざわざMutableListを使いたいケースの場合、最初に空のリストを作ってそこに追加していく、というケースが多い。
そして空のmutableListOf呼び出しでは、要素の型を推測しようが無いので、`<>`で指定してやる必要がある。

という事で文字列のListを作って足していく場合は以下のようになる。

{% capture mlist_code1 %}
fun main() {
  val mlist = mutableListOf<String>()
  mlist.add("一つ！")
  mlist.add("二つ！")
  mlist.add("三つ！")

  for((index, elem) in mlist.withIndex()) {
    println("${index}番目の要素は「${elem}」です");
  }
}
{% endcapture %}
{% include kotlin_quote.html body=mlist_code1 %}

`mutableListOf<String>()`で空のmutableListを作り、そこに`add()`で要素を追加している。

なお、中を空にするのはclear。

{% capture mlist_code2 %}
fun main() {
  val mlist = mutableListOf("ついにねんがんのアイスソードをてにいれたぞ", "そうかんけいないね", "ころしてでもうばいとる")

  println("クリアする前:")
  println(mlist)

  mlist.clear()
  println("")
  println("クリアしたあと：")
  println(mlist)
}
{% endcapture %}
{% include kotlin_quote.html body=mlist_code2 %}

**課題: 要素100個のリストをfor文で作れ**

要素が以下の百個のリストを作れ。最初が「0番目」じゃなくて「1番目」なのに注意。

"1番目", "2番目", "3番目", ... , "100番目"

なお、出力は「100番目」になるのが正解。

{% capture mlist_100 %}
fun main() {
  // 以下を書き換える
  val mlist = emptyList<String>()

  // ここでfor文を使って100個の文字列をmlistに入れる。


  // 以下はいじらない
  println(mlist[99])
}
{% endcapture %}
{% include kotlin_quote.html body=mlist_100 %}

### MutableListはユーザーからの入力を追加していくようなアプリで使う

ユーザーにテキストを入力していくとリストに追加されていくようなアプリ、例えば[てきすとでっき - Google Play のアプリ](https://play.google.com/store/apps/details?id=io.github.karino2.textdeck&hl=ja)みたいなアプリを作る時には、
MutableListを使う事になる。

この後にListViewを使う時にはMutableListの事も思い出してあげてください。

## 公式ドキュメントへのリンク

英語だけど公式ドキュメントへのリンクも貼っておく。（主に自分用）

- [List - Kotlin Programming Language](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-list/)
- [MutableList - Kotlin Programming Language](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-list/)
- [mutableListOf - Kotlin Programming Language](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/mutable-list-of.html)