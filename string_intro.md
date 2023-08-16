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
  val s1 = "ついにねんがんの"
  val s2 = "アイスソードを"
  val s3 = "てにいれたぞ"

  val s = s1+s2+s3

  println(s)
}
{% endcapture %}
{% include kotlin_quote.html body=renketu_code1 %}

こんな感じでつなげる事が出来ます。`"1"+"2"`は3では無く`"12"`になる事に注意しましょう。


{% capture renketu_code2 %}
fun main() {
  println("1"+"2")
}
{% endcapture %}
{% include kotlin_quote.html body=renketu_code2 %}

### varと+=で連結

kotlinには`+=`というのもあります。
少しややこしいですが、

```kotlin
a += b
```

は、以下と同じ意味です。

```kotlin
a = a + b
```

慣れないうちはいつも頭の中で下になおして考えましょう。
そしてこの`+=`は変更した結果をまた同じ変数に代入するので、いつも`var`になります。
`+=`を使う時は`var`、と覚えておくといいかもしれない。

以下例を見てみましょう。`+=`は、複数ある文字列をつなげる時に良く使います。

{% capture plusassign_code1 %}
fun main() {
  val s1 = "ついにねんがんの"
  val s2 = "アイスソードを"
  val s3 = "てにいれたぞ"

  var s = ""
  s += s1
  s += s2
  s += s3

  println(s)
}
{% endcapture %}
{% include kotlin_quote.html body=plusassign_code1 %}

sが`var`なのに注目。こういう風に`+=`を使う変数は`var`にしないといけない（上のコードをvalに変更してとどうなるか試してみよう）。

これだけだとなんでこんな事するの？って感じですが、Listとかfor文が出てくるともうちょっと意味が分かります。
以下はコレクションとforループをやるところで説明しますが、こんな感じで使う、という雰囲気を感じ取るべく載せておきます。

{% capture plusassign_code2 %}
fun main() {
  val items = listOf("ついにねんがんの", "アイスソードを", "てにいれたぞ")
  var s = ""

  for(item in items) {
    s += item
  }

  println(s)
}
{% endcapture %}
{% include kotlin_quote.html body=plusassign_code2 %}

こんな感じで、リストの文字列を全部つなげたい、みたいな時に`+=`を使うと便利です。

なお、別に上のコードの`+=`のところを、以下のように書いても構いません。

```kotlin
s = s + item
```

つまりこんな感じ。

{% capture plusassign_code3 %}
fun main() {
  val items = listOf("ついにねんがんの", "アイスソードを", "てにいれたぞ")
  var s = ""

  for(item in items) {
    s = s + item
  }

  println(s)
}
{% endcapture %}
{% include kotlin_quote.html body=plusassign_code3 %}

いいんだけど、普通は`+=`で書くかな。

## Raw string

ダブルクオート三つでraw stringと呼ばれるもになります。

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

こういう風に複数行をバックスラッシュnとかいっぱい入れずに書けてちょっと便利という機能ですが、たまにしか使いません。


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

変数の切れ目がkotlin側に分かるように空白など（カンマとかでも平気）が必要になる場合があります。（上の例で空白を削除して実行してみよう）

空白を開けたくないときやもっと複雑な式を入れたいときは中括弧でくくります。

{% capture strtemplate_code2 %}
fun main() {
  val counter = 1234
  val s = "あなたは${counter}人目の来場者です"

  println(s)
}
{% endcapture %}
{% include kotlin_quote.html body=strtemplate_code2 %}

結構便利なので良く使います。