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

```
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

以下のように書いても全く同じ意味です。

```kotlin
val content = "これは一行目です\nこれは二行目です\nこれは三行目です\n"
```

こういう風に書くのは面倒なので、ファイルの中身と同じようなテストデータを用意する時には今後はこのraw stringを使っていきます。
前回のTextFileLib.readTextでファイルを読みだした結果はだいたいこのような文字列となるので、
この文字列を自由に扱えたらファイルの中身も自由に扱えます。

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

```
1691126681002,これは一行目のアイテムです
1691137849935,これは二行目のアイテムです
1691189379291,これは三行目です。別にどんな文字列でもいいですが、このフォーマットだと改行は入れられない。
```

１つ目のカンマはいいとして、後ろの文字にカンマが使われるとどうなるでしょう？例えば１行目の「これは」の後にユーザーがカンマを入力した場合を考えると以下のようになります。

```
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
  // 1行ずつに分割する
  val lines = content.split("\n")

  // 各行に対して
  for(line in lines) {
    // カンマで二つに分割する
    val items = line.split(",", limit=2)

    println("１つ目が「${items[0]}」、二つ目が「${items[1]}」")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=code_split4 %}

これで文字列からPostのオブジェクトのリストに戻せそうです。（後でそういう課題をやります）

## splitの逆はjoinToString

ファイルから読む場合はsplitして処理します。逆にファイルに保存する場合はリスト最後に一つの文字列につなげる、という事を良くやります。
これまでやったようにvarと`+=`でつなげてもいいのですが、この処理は良くあるので専用のメソッドも覚えるに値します。
それがjoinToStringです。

以下のように使います。

{% capture code_join1 %}

fun main() {
  val list = listOf("これは一行目です", "これは二行目です", "これは三行目です")
  val content = list.joinToString("\n")
  println("「$content」")
}
{% endcapture %}
{% include kotlin_quote.html body=code_join1 %}

要素と要素の間に指定した文字を挟んでつなげてくれます。例えば改行では無くカンマを挟むと以下のようになります。

{% capture code_join2 %}

fun main() {
  val list = listOf("これは一行目です", "これは二行目です", "これは三行目です")
  val content = list.joinToString(",")
  println("「$content」")
}
{% endcapture %}
{% include kotlin_quote.html body=code_join2 %}

## startsWith, endsWith

文字列が何で始まっているか、何で終わっているか、というので分岐したい事も良くあります。
そういう時につかうのがstartsWithとendsWithです。

まずstartsWithの例を見てみましょう。

{% capture code_startswith1 %}
fun main() {
  val content = "hogeika"

  println(content.startsWith("hoge"))
  println(content.startsWith("hoga"))
  println(content.startsWith("ika"))
}
{% endcapture %}
{% include kotlin_quote.html body=code_startswith1 %}

指定した文字列で始まっているならtrueを、始まっていないならfalseを返します。

次はendsWithの例。

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

指定した文字で終わっていたらtrueを、終わっていなければfalseを返します。

例えばファイルの一覧から`.txt`で終わるファイルだけを集めたりする時に使います。

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
