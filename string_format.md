---
title: "文字列のフォーマット"
layout: page
---
String型にはformatという関数があります。

formatはめちゃくちゃ高機能な関数で、しかもほとんどはAndroid開発では使いません。
そこで良く使う機能だけを簡単に見てみます。

## formatの概要とフォーマット記述子

formatというのは、文字列の中に値を埋め込むのに使うもので、
[文字列入門のString template](string_intro.md#string-template)と似たような機能になります。
けれどString templateよりもっと複雑で細かい制御が出来る。

まずは例を見てみましょう。以下が簡単なformatの例になります。

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

`"hoge %s ika".format("うが〜")` みたいなのがformatの例。

基本的には

- 文字列の中にパーセントで始まる記号を入れる
- その文字列にドットをつけてfomatを呼び出して、パーセントの記号をその値で置き換える

という事をします。
この置き換えの所で細かい指定を出来る、というのがformatの売りです。

パーセントの所をフォーマット記述子と言います。

### フォーマット記述子の種類

Android開発で使うのはsとdとfの三つです。しかもsはString templateで代用出来るのでdとfだけ知っていれば十分でしょう。

- `%s` ... 文字列
- `%d` ... 整数
- `%f` ... 小数(1.23とか)

とりあえずこの三つがあるという事を覚えたら、あとはdとfしか使いません。

{% capture string_format_2 %}
fun main() {
  val s = "文字列： %s".format("うが〜")
  println(s)

  val s2 = "整数： %d".format(123)
  println(s2)

  val s3 = "小数： %f".format(3.14)
  println(s3)
}
{% endcapture %}
{% include kotlin_quote.html body=string_format_2 %}

### 整数の桁指定とパディング

dの前にその数字を何桁として出力するかを数字で指定出来ます。
4桁として指定したいなら`%4d`とします。

桁が足りない所をどう埋めるかを「パディング」と呼びます。

0でパディングするかスペースでパディングするかのどちらかしか使わないでしょう。
先に例を見てみます。

{% capture string_format_int_padding %}
fun main() {
  val s1 = "整数： %d".format(123)
  println(s1)

  val s2 = "整数4桁、スペースパディング： %4d".format(123)
  println(s2)

  val s3 = "整数4桁、0パディング： %04d".format(123)
  println(s3)
}
{% endcapture %}
{% include kotlin_quote.html body=string_format_int_padding %}

数字が4桁以上だとどうなるかなども試してみてください。

何も指定しないとスペースでパディングされます。
パディングを0にしたいならパーセントのあとに0と書いてから桁数とdなどを書きます。
つまり`%04d`と書くと、4桁で0のパディング、という意味になります。

### 小数の桁指定とパディング

小数は小数点の上と下の指定があるので少しややこしいです。
基本的には`%11.3f`の形で指定します。11の所が桁数指定、3の所が小数点以下を幾つにするかです。

桁数の指定は小数点の為のドットやその下の小数も全部含めた桁数になります。

{% capture string_format_float_padding %}
fun main() {
  val s1 = "小数点以下だけ指定： %.3f".format(123.1)
  println(s1)

  val s2 = "全部で6桁： %6.1f".format(123.1)
  println(s2)

  val s3 = "全部で6桁、0パディング： %06.1f".format(123.1)
  println(s3)
}
{% endcapture %}
{% include kotlin_quote.html body=string_format_float_padding %}

## Javaのドキュメント（英語）

ほぼ自分のためのメモ。

[Formatter (Java Platform SE 8 )](https://docs.oracle.com/javase/8/docs/api/java/util/Formatter.html)