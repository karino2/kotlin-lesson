---
title: "コレクション入門"
layout: page
---
ListとMapについて書く。Setもちょっと書くかも。

## List入門

ListはlistOfで作り`[]`でアクセスします。

{% capture list_basic %}
fun main() {
  val items = listOf("ひとつ!", "ふたーつ!", "みっつ！")

  println(items[1])
}
{% endcapture %}
{% include kotlin_quote.html body=list_basic %}

### 百本ノックを10個くらいやってみよう

この手のものはたくさん書いて慣れるに尽きます。

[第6.1回: 配列100本ノック - 算数で挫折した人向けの、Javascript入門](https://karino2.github.io/js-introduction/ch06_1.html)と同じようなことをやってみましょう。
問題作るのかったるいので、向こう見ながら自分でやってみてください。もういいかな、と思うくらいまでやって先に進みましょう。

**以下のリストを生成せよ**

1. "むぇ～～～"
2. "コケー"
3. "ダネ～～"

{% capture list_create %}
fun main() {
  val kotae = 0

  println(kotae)
}
{% endcapture %}
{% include kotlin_quote.html body=list_create %}


**「"コケー"」 を取り出せ**

kotaeに入れてね。

{% capture list_access %}
fun main() {
  val list = listOf("むぇ～～～","コケー","ダネ～～")
  val kotae = 0

  println(kotae)
}
{% endcapture %}
{% include kotlin_quote.html body=list_access %}
