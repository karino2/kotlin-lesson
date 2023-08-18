---
title: "テキスト処理入門"
layout: page
---
前の課で文字列をファイルに書いたり読んだりが、とりあえずは出来るようになりました。

あとは保存する文字列を作ったり、保存されている文字列を読んでどうにかすれば、
実はほとんど全てのデータを保存する事が出来ます。

ここでは目的の文字列を作ったり、文字列から目的のデータに変換したりする処理、「テキスト処理」を学びます。

テキスト処理は一大分野で、世の中には仕事のほとんどがテキスト処理だ、というプログラマもそれなりにいるくらいです。
ここではとりあえず当面の用が足せるくらいの理解を目指します。

## 文字で分割するsplit

{% capture code_split0 %}
fun main() {
  val content = "1, 2, 3"

  val list = content.split(",")
  println(list.size)
  for((index, item) in list.withIndex()) {
    println("${index}番目の文字列は「${item}」です")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=code_split0 %}



{% capture code_split1 %}
val content = """これは一行目です
これは二行目です
これは三行目です
"""

fun main() {
  val list = content.split("\n")
  println(list.size)
  for((index, line) in list.withIndex()) {
    println("${index}番目の文字列は「${line}」です")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=code_split1 %}

{% capture code_split2 %}
val content = """これは一行目です
これは二行目です
これは三行目です"""

fun main() {
  val list = content.split("\n")
  println(list.size)
  for((index, line) in list.withIndex()) {
    println("${index}番目の文字列は「${line}」です")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=code_split2 %}


## splitの逆はjoinToString

{% capture code_join1 %}

fun main() {
  val list = listOf("これは一行目です", "これは二行目です", "これは三行目です")
  val content = list.joinToString("\n")
  println("「$content」")
}
{% endcapture %}
{% include kotlin_quote.html body=code_join1 %}

## startsWith, endsWith


{% capture code_startswith1 %}
fun main() {
  val content = "hogeika"

  println(content.startsWith("hoge"))
  println(content.startsWith("hoga"))
  println(content.startsWith("ika"))
}
{% endcapture %}
{% include kotlin_quote.html body=code_startswith1 %}


{% capture code_startswith2 %}
fun main() {
  val content = "hogeika"

  println(content.endsWith("ka"))
  println(content.endsWith("ika"))
  println(content.endsWith("a"))
  println(content.endsWith("hka"))
  println(content.endsWith("hoge"))
}
{% endcapture %}
{% include kotlin_quote.html body=code_startswith2 %}

## substring

{% capture code_substr1 %}
fun main() {
  val content = "hogeika"

  println(content.substring(1))
  println(content.substring(1, 3))
  println(content.substring(1..3))
}
{% endcapture %}
{% include kotlin_quote.html body=code_substr1 %}

## 長さはlength

{% capture code_length1 %}
fun main() {
  val content = "hogeika"

  println(content.length)
  println(content.substring(content.length-3))
  println(content.substring(content.length-3, content.length-1))
}
{% endcapture %}
{% include kotlin_quote.html body=code_length1 %}

## trim, trimStart, trimEnd

前後の空白とかをカットする。

{% capture code_trim1 %}
fun main() {
  val content = "  前後 と中 に空白  "

  val trim = content.trim()
  val trimStart = content.trimStart()
  val trimEnd = content.trimEnd()
  println("「${trim}」")
  println("「${trimStart}」")
  println("「${trimEnd}」")
}
{% endcapture %}
{% include kotlin_quote.html body=code_trim1 %}


