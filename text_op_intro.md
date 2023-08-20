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

なお、公式ドキュメントにここに載せてないのもあるので、必要になったらそちらも参照したい。
英語だけど自動翻訳とか使って頑張れ。
[String - Kotlin Programming Language](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-string/)

以下ではとりあえず使いそうなString型のメソッドを紹介していきます。

## 最終的にやりたい事

個々のメソッドを見ていく前に、そもそもどういう事をやりたくなるのか、という事から説明してみます。

とりあえずやりたい事としては、ListViewに表示するアイテムを、1行1アイテムで保存していくような事がやりたい。
例えば、以下のようなdata classとリストがあったとする。

```kotlin
data class Post(val content: String, val created: Date)

val mlist = mutableListOf<Post>()
```

このmlistをファイルに保存して、次回起動時にこのファイルから読み出してリストをもう一回作りたい、とする。

こういう時に、各要素を1行で保存する、というのが一番カンタンな保存方法となる。
DateはgetTimeで数字に出来る事を思うと、カンマで区切った以下のような文字列なら、このデータを保存出来そう。

```kotlin
1691126681002,これは一行目のアイテムです
1691137849935,これは二行目のアイテムです
1691189379291,これは三行目です。別にどんな文字列でもいいですが、このフォーマットだと改行は入れられない。
```

こんな風に、1行につき一つの要素が対応するような文字列を作ってファイルに保存したり、ファイルから取り出して１行ずつ見て言ってPostオブジェクトに変えていったりしたい。

こういう感じの事が出来るようになるのを目標に、いろいろ機能を見ていきます。

## 文字で分割するsplit

まずは文字列を適当な文字で分割するsplitというメソッドを見ていきます。
splitは分割する文字列を渡すと、それで分割した結果のListが返ってきます。

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

`content.split(",")`で、カンマで分割した結果のリストを返します。
この区切る文字を「セパレータ」と呼んだりもします。

また、良くある事として、1行ずつに分ける、というのもこれを使います。行で分割するには、行の終わりに必ず`\n`があるのを利用して、
`\n`で分割すれば良い。

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

ダブルクオート三つはraw stringという機能で、改行などを含める文字列を作るのに使えるものでした。（[文字列入門](string_intro.md)参照）

これはファイルの中を前回のTextFileLib.readTextで読みだした結果とだいたい同じようなものなので、以下ファイルの中身をそのまま書く時には使っていきたいと思います。

さて、0番目から始まるのはいいとして、最後に１行、空の行が出来てしまいます。
これは最後が改行で終わっているからです。

以下のように最後の改行を取ると、ちゃんと3行だけになります。

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

なお、この最後に改行だけがある場合に実際にどう対処すれば良いか、というのは、後の`trimEnd`を使えば良いです。

### limitでsplitした結果の数を指定

さて、先ほど見たファイルに保存する例を考えます。

```kotlin
1691126681002,これは一行目のアイテムです
1691137849935,これは二行目のアイテムです
1691189379291,これは三行目です。別にどんな文字列でもいいですが、このフォーマットだと改行は入れられない。
```

１つ目のカンマはいいとして、後ろの文字にカンマが使われるとどうなるでしょう？例えば１行目の「これは」の後にユーザーがカンマを入力した場合を考えると以下のようになります。

```kotlin
1691126681002,これは,一行目のアイテムです
```

これをsplitすると3つになってしまう。でも本当は、最初の数字とそれ以外の文字の二つにわけたい。

こういう用途では、splitした結果の要素数の最大個数を指定する、というのが出来ます。
`limit=`というもので指定します。

以下例を見てみましょう。

{% capture code_split3 %}

fun main() {
  val content = "a/ここにも/が"
  println(content.split("/"))
  println(content.split("/", limit=2))
}
{% endcapture %}
{% include kotlin_quote.html body=code_split3 %}

`limit=2`とあると、その個数までsplit出来たら以後の文字の中にセパレータがあっても無視する、という挙動になります。

関数の引数に`limit=2`などと指定するのは今回が初めての新しい指定方法ですね。
これはしばらくはこのsplitでしか使わないので細かい解説はせずに、
splitだけこういうのがあると覚えておいて先に進んでください。

これを使えば、以下のように各行を処理する事が出来ます。

{% capture code_split4 %}
val content = """1691126681002,これは一行目のアイテムです
1691137849935,これは二行目のアイテムです
1691189379291,これは三行目です。別にどんな文字列でもいいですが、このフォーマットだと改行は入れられない。"""

fun main() {
  val lines = content.split("\n")
  for(line in lines) {
    val items = line.split(",", limit=2)
    println("１つ目が「${items[0]}」、二つ目が「${items[1]}」")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=code_split4 %}

これで文字列からPostのオブジェクトのリストに戻せそうです。（後でそういう課題をやります）

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

なお、トリムする文字を指定する事も出来る。
これはダブルクオートじゃなくてシングルクオート（`"a"`では無く`'a'`とする）なのに注意。
理由は文字列じゃなくて文字だからなのだけれど、とりあえずtrimの時はシングルクオートと丸暗記しておけばいいです。

splitの例であった、最後が空行が入っているケースも、これを使うと以下のように出来る。

{% capture code_trim2 %}
val content = """これは一行目です
これは二行目です
これは三行目です
"""

fun main() {
  val trim = content.trimEnd('\n')
  val list = trim.split("\n")
  println(list.size)
  for((index, line) in list.withIndex()) {
    println("${index}番目の文字列は「${line}」です")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=code_trim2 %}
