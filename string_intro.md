---
title: "文字列入門"
layout: page
---
文字列についてはJS入門でも少しやりましたが、ゲームよりAndroidアプリの方が文字列を良く使うので、ここであらためて見ておきましょう。

といっても文字列はいろんな機能があるので、最初に全部やるのは大変です。
ここでは、ドラクエIIIに例えるととりあえず船取るくらいまで通じる程度の入門をしたいと思います。

まず文字列はダブルクオートで囲んだものです。`"ほげほげ"`などが文字列です。
型としてはString型です。

{% capture str_code1 %}
fun main() {
  val mojiretu = "これは文字列です"

  println(mojiretu)
}
{% endcapture %}
{% include kotlin_quote.html body=str_code1 %}

また、文字列の中に改行を入れる事が出来ます。改行はバックスラッシュとnで書きます。Windowsだと半角の￥記号とnです。

改行ってなんやねん？と思う人もいると思うので、見てみるのがいいです。

{% capture kaigyou_code %}
fun main() {
  val mojiretu = "これ\nは\n改行\nの\n例です"

  println(mojiretu)
}
{% endcapture %}
{% include kotlin_quote.html body=kaigyou_code %}
