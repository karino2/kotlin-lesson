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
