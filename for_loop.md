---
title: "forループ入門"
layout: page
---
コレクションの説明を書こうと思ったら、for文が無いときつい事に気づいたのでfor文を解説する事に。

Listの要素を一つずつ取り出したりするのがfor文です。

{% capture for_basic1 %}
fun main() {
  val items = listOf("ひとつ!", "ふたーつ!", "みっつ！")

  for(a in items) {
    println(a)
  }
}
{% endcapture %}
{% include kotlin_quote.html body=for_basic1 %}

文法としては以下のようになります。

```
for(変数名 in コレクション) {

}
```

変数名というところでつけた名前が、このforの中括弧で囲まれた範囲（ブロックといいます）でだけ使えるようになります。
新しい変数を作るのにvalやvarを作らない珍しいケースですね。

コレクションのところは、リストかRangeというものと覚えておくと最初はいいと思う。Rangeについては後で説明します。

こういうのは何度も書いて覚えるのが一番！という事で自分で書いてみましょう。

**itemsを一つずつ取り出してprintlnするコードを書け**

{% capture for_basic2 %}
fun main() {
  val items = listOf("ひとつ!", "ふたーつ!", "みっつ！")

}
{% endcapture %}
{% include kotlin_quote.html body=for_basic2 %}

**itemsを一つずつ取り出してprintlnするコードを書け**
{% capture for_basic3 %}
fun main() {
  val items = listOf("なにぃっ？", "リュウがいない", "いったいどこへ…")

}
{% endcapture %}
{% include kotlin_quote.html body=for_basic3 %}

**itemsの中身を全部合計した値を求めよ**

sumに要素を足していきます。

{% capture for_basic4 %}
fun main() {
  val items = listOf(1, 2, 3, 4, 5)
  var sum = 0

  // TODO: ここにfor文を書いて合計をsumに入れよ


  // TODO: 以下はいじらない
  println(sum)

}
{% endcapture %}
{% include kotlin_quote.html body=for_basic4 %}


**itemsの中身を全部合計した値を出力せよ**

今度は変数も自分で作って、それをprintlnするのも自分で書いてください。

{% capture for_basic5 %}
fun main() {
  val items = listOf(5, 6, 7, 8)

  // TODO:ここでfor文を書いて合計を変数に入れ

  // TODO:ここでprintlnして
}
{% endcapture %}
{% include kotlin_quote.html body=for_basic5 %}


**itemsの中身を全部つなげた文字列を作れ**

今度は文字列です。全部をつなげた文字列を作ってください。
なお、この文字列のつなげかたはあまり良くないのですが、それは終盤でたぶん解説します。

{% capture for_basic6 %}
fun main() {
  val items = listOf("これを", "全部", "つなげる")

  var kotae = ""

  // TODO:ここでfor文を書いてkotaeにitemsの中身を全部つなげる

  // 以下はいじらない
  println(kotae)

}
{% endcapture %}
{% include kotlin_quote.html body=for_basic6 %}

**itemsの中身を全部つなげた文字列を作って出力せよ**

今度は変数も自分で作って、それをprintlnするのも自分で書いてください。

{% capture for_basic7 %}
fun main() {
  val items = listOf("今度も", "全部", "つなげる")

  // TODO:ここでfor文を書いてitemsの中身を全部つなげる

  // TODO:ここでつなげた文字の入った変数をprintlnする
}
{% endcapture %}
{% include kotlin_quote.html body=for_basic7 %}

### 添字はindicesかwithIndex()

以下のようなものがあったとします。

{% capture for_index1 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in youbi) {
    println(a)
  }
}
{% endcapture %}
{% include kotlin_quote.html body=for_index1 %}

ここで、この出力を「月曜、水曜、金曜、日曜」と一日おきにするにはどうしたらいいかを考えます。

もちろんここまでの知識を使って、Booleanのフラグを使って以下のように書けば書けそうですが

{% capture for_index2 %}
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
{% include kotlin_quote.html body=for_index2 %}

これはちょっとややこしい。
あれ？「!toggle」は初めてかな？ビックリマークは反転させる、という機能で、trueがfalse、falseがtrueになります。
まぁいい。

それよりも、こういうのは添字の一覧を回せると良いです。それはindicesというのを使います。
以下のコードを実行してみてください。

{% capture for_index3 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in youbi.indices) { // ここがNew!
    println(a)
  }
}
{% endcapture %}
{% include kotlin_quote.html body=for_index3 %}
