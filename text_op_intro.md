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

## 行ごとに分割するsplit

{% capture code_split1 %}
val content = """これは一行目です
これは二行目です
これは三行目です
"""

fun main() {
  val list = content.split("\n")
  println(list.size)
  for((index, line) in list.withIndex()) {
    println("${index}行目の文字列は「${line}」です")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=code_split1 %}

## splitの逆はjoinToString

{% capture code_join1 %}

fun main() {
  val list = listOf("これは一行目です", "これは二行目です", "これは三行目です")
  val content = list.joinToString("\n")
  println(content)
}
{% endcapture %}
{% include kotlin_quote.html body=code_join1 %}
