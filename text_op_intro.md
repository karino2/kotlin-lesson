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

- [String - Kotlin Programming Language](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-string/)
- [String (Java Platform SE 8 )](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)

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

ちなみにこういう風に、最初に数字を置いてカンマで区切って次に文字を置く、みたいなルールを「フォーマット」と呼びます。日本語に訳すと書式なんでしょうけれど、みんなカタカナでフォーマットと呼んでますね。

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

**課題: .txtで終わるファイルだけを含んだリストを返す、textFileOnly関数を作れ**

ヒント: `List<String>`を引数にして`List<String>`を返します。mutableListOfのローカル変数を作って、endsWithでチェックしてadd。

{% capture code_startswith3 %}
// TODO: textFileOnly関数を作れ


// 以下はいじらない
fun main() {
  val list = listOf("2023_surf.png", "hello.txt", "メモ.txt", "2023_08_20_風景.jpg", "ネタ帳.txt")

  val actual = textFileOnly(list)

  println(actual == listOf("hello.txt", "メモ.txt", "ネタ帳.txt"))
}
{% endcapture %}
{% include kotlin_quote.html body=code_startswith3 %}

## 文字の位置を探すindexOf

文字の中で、指定された文字がどこにあるかを探すのがindexOfメソッドです。
先頭から何文字目かのインデックスを返します。

見つからないと-1を返します。

{% capture code_indexof1 %}
fun main() {
  val content = "これはテストです"

  println(content.indexOf("は"))
  println(content.indexOf("テス"))
  println(content.indexOf("で"))
  println(content.indexOf("ほげ"))
}
{% endcapture %}
{% include kotlin_quote.html body=code_indexof1 %}

次のsubstringと組み合わせるとだいたいなんでも出来ますが、めちゃバグりやすい。

## 一部を取り出すsubstring

文字列の中の一部を文字列として取り出すのはsubstringメソッドです。
数字一つを指定するとそこより後ろの文字全部を、数字二つを指定すると始まりのインデックスと終わりのインデックスの一つ手前までを（なんで！？って感じだが）返します。

また、rangeを指定する事もできます。以下例を見てみましょう。

{% capture code_substr1 %}
fun main() {
  val content = "hogeika"

  println(content.substring(1))
  println(content.substring(1, 3))
  println(content.substring(1..3))
}
{% endcapture %}
{% include kotlin_quote.html body=code_substr1 %}

indexOfとsubstringを使えばたいていの事は出来るけれど、添字のバグが生まれやすいので、10倍界王拳くらいのつもりで使いましょう。

## 長さはlength

文字の長さはlengthです。これはメソッドでは無くプロパティ（つまり`length()`では無く`length`）です。
また、substringと組み合わせると後ろの方だけ取る、みたいな事も出来ます。

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

中にある空白はカットされず、前後のだけがカットされている事に注目。
trim系はあくまで両端をカットするだけで、何かtrim対象じゃない文字が間にあったらその中はもうカットしません。

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

これは割と基本的なパターンで、

1. ファイルから読む
2. trimEndで最後の改行を（あれば）取り除く
3. splitで行ごとにバラす

というのは今後何度もやる事になるでしょう。

## 課題: テキストからPostのリストを作ろう

ファイルから読み込むケースをまずはやってみます。
以下のような形式のテキストから、

```
1691126681002,これは一行目のアイテムです
1691137849935,これは二行目のアイテムです
1691189379291,これは三行目です。別にどんな文字列でもいいですが、このフォーマットだと改行は入れられない。
```

Postのリストを作ります。

```kotlin
data class Post(val content: String, val created: Date)
```

- ヒント1: "1691126681002"は大きい数字なので数字にするのにtoInt()では無くtoLong()を使う。
- ヒント2: 1691126681002という数字から対応するDateを作るのはDate(1691126681002)

という事でやってみましょう。

1. 最後の改行を（あれば）trim
2. 行にsplitでバラす
3. 各行をさらにカンマでバラす、limitも忘れずに
4. Post型のオブジェクトを作りリストに追加していく

これを行う関数は、parseTextという名前にしましょう。このように文字列からオブジェクトにする事をパースといいます。

{% capture code_fromfile0 %}
import java.util.Date

data class Post(val content: String, val created: Date)

// TODO: ここにparseTextを作れ


// 以下はいじらない

val content = """1691126681002,これは一行目のアイテムです
1691137849935,これは,二行目のアイテムです
1691189379291,これは三行目です。別にどんな文字列でもいいですが、このフォーマットだと改行は入れられない。
"""

fun main() {
  val actual = parseText(content)

  println(actual.size == 3)
  println(actual[1].created == Date(1691137849935))
  println(actual[1].content == "これは,二行目のアイテムです")
}
{% endcapture %}
{% include kotlin_quote.html body=code_fromfile0 %}

## 課題: Postのリストからテキストを作ろう

今度は逆にファイルに保存する時です。Postのリストからひとつながりの文字列を作ります。フォーマットも先ほどと同じフォーマットにしましょう。

名前はconvertToTextにしますか。convertは変換するとかそういう意味です。

ヒント: まずは１行を1要素とするListを作って、joinToStringしよう。

{% capture code_tofile0 %}
import java.util.Date

data class Post(val content: String, val created: Date)

// TODO: ここにconvertToTextを作れ


// 以下はいじらない

fun main() {

  val target = listOf(Post("これは一行目のアイテムです", Date(1691126681002)),
                      Post("これは,二行目のアイテムです", Date(1691137849935)),
                      Post("これは三行目です。別にどんな文字列でもいいですが、このフォーマットだと改行は入れられない。", Date(1691189379291))
                      )
  val actual = convertToText(target)


val expect = """1691126681002,これは一行目のアイテムです
1691137849935,これは,二行目のアイテムです
1691189379291,これは三行目です。別にどんな文字列でもいいですが、このフォーマットだと改行は入れられない。"""

  println(actual == expect)
}
{% endcapture %}
{% include kotlin_quote.html body=code_tofile0 %}

## 課題: ListViewの編集編に投稿時間をつけたものを保存するようにしよう

1. [ListViewに挑戦、編集編](listview_edit.md)
2. [日付を扱う、Date入門](date_intro.md)の「課題: 「ListViewに挑む！編集編」に、投稿時間をつける」
3. [data class入門](dataclass_intro.md)の「課題： 前回の日付の入門でやった、「ListViewに挑む！編集編」に、投稿時間をつけたものをdata class化しよう」

をさらに保存するようにしよう。
毎回ちょっとずつ更新しているので全体像が散らばってきたので、ここでやりたい事を記述しなおします。
練習のために1から作ってもいいでしょう。

### レイアウト

レイアウトは以下のようにする

1. 入力用EditText
2. LoadボタンとSubmitボタン
3. ListView

2はLinearLayoutのhorizontalで。

入力用EditTextにテキストを入力してSubmitとすると、入力したテキストとその時点の時刻がListViewに表示され、さらにファイルに保存されるとする。
ファイル名は"listview_save_data.txt"にしましょう。

ListViewのアイテムのレイアウトは

1. TextView
2. 削除ボタンと日付のTextView

としましょう。日付のTextViewはテキストサイズを小さめで。

### ファイル保存のためにTextFileLibを追加

[ファイルをとりあえず使うだけの入門](textfilelib_intro.md)でやったのと同様にTextFileLib.ktを追加して、その他必要な作業をやります。

### data classと保存のフォーマット

data classは以下のPostで。

```kotlin
data class Post(val content: String, val created: Date)
```

保存のフォーマットはここ最近やっていた、以下の形式とします。

```
1691126681002,これは一行目のアイテムです
1691137849935,これは,二行目のアイテムです
1691189379291,これは三行目です。別にどんな文字列でもいいですが、このフォーマットだと改行は入れられない。
```

### ソースコードの概要

`MutableList<Post>` をメンバ変数に持ち、これのArrayAdapterを作る。

onCreateの方にLoadとSubmitのsetOnClickListenerの処理を書く。

Submitの都度"listview_save_data.txt"にメンバ変数のMutableListにPostを追加してnotifyDatasetChangedをしつつ、
このリストを文字列にしてTextFileLib.writeTextする。

Loadボタンは"listview_save_data.txt"からテキストを読んでMutableListに加え直す。
ロードの時には一旦リストをクリアするようにしましょう。

getViewの方ではTextViewにPostのcontentを表示して日付のTextViewのcreatedをtoStringしたものをセットする。
さらに削除ボタンのonClickListenerではリストからアイテムを削除してnotifyDatasetChangedする。