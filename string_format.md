---
title: "文字列のフォーマット"
layout: page
---
String型にはformatという関数があります。
ここではこのformat関数について簡単に見ていきます。

{% capture string_format %}
fun main() {
  val s = "hoge %s ika".format("うが〜")
  println(s)

  val s2 = "hoge %d".format(123)
  println(s2)

  val s3 = "hoge %04d".format(23)
  println(s3)
}
{% endcapture %}
{% include kotlin_quote.html body=string_format %}
