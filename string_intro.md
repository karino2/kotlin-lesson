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

## 改行は＼n（Windowsだと￥n）

文字列の中に改行を入れる事が出来ます。改行はバックスラッシュとnで書きます。Windowsだと半角の￥記号とnです。
（バックスラッシュはホームページ上でトラブルを生みがちなので説明では全角で書いています）

改行ってなんやねん？と思う人もいると思うので、見てみるのがいいです。

{% capture kaigyou_code %}
fun main() {
  val mojiretu = "これ\nは\n改行\nの\n例です"

  println(mojiretu)
}
{% endcapture %}
{% include kotlin_quote.html body=kaigyou_code %}

このバックスラッシュとnというのを入れると、次の行に行く感じです。

## 文字列の連結は「+」

文字列をつなぎ合わせるのはプラス記号（`+`）です。

{% capture renketu_code1 %}
fun main() {
  val s1 = "ついに念願の"
  val s2 = "アイスソードを"
  val s3 = "てにいれたぞ"

  val s = s1+s2+s3

  println(s)
}
{% endcapture %}
{% include kotlin_quote.html body=renketu_code1 %}

`+=`の話もこの辺に書く。

## Raw string

ダブルクオート三つでraw stringです。

{% capture rawstr_code1 %}
fun main() {
  val s = """
複数行がこうやって書ける。
ダブルクオートも使える。"文字列"みたいに。
いろいろテキストをそのまま書けて便利。
"""

  println(s)
}
{% endcapture %}
{% include kotlin_quote.html body=rawstr_code1 %}


## String template

文字列の中にドルと変数名でString templateです。

{% capture strtemplate_code1 %}
fun main() {
  val counter = 1234
  val s = "あなたは$counter 人目の来場者です"

  println(s)
}
{% endcapture %}
{% include kotlin_quote.html body=strtemplate_code1 %}

空白を開けたくないときやもっと複雑な式を入れたいときは中括弧でくくります。

{% capture strtemplate_code2 %}
fun main() {
  val counter = 1234
  val s = "あなたは${counter}人目の来場者です"

  println(s)
}
{% endcapture %}
{% include kotlin_quote.html body=strtemplate_code2 %}
