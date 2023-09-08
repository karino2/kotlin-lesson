---
title: "forループ入門の課題の解答"
layout: page
---
[forループ入門](for_loop.md)の課題の解答です。

**課題: itemsを一つずつ取り出してprintlnするコードを書け**

{% capture for_basic2 %}
fun main() {
  val items = listOf("ひとつ!", "ふたーつ!", "みっつ！")

  // 答え
  for(a in items) {
    println(a)
  }
}
{% endcapture %}
{% include kotlin_quote.html body=for_basic2 %}

**課題: itemsを一つずつ取り出してprintlnするコードを書け**
{% capture for_basic3 %}
fun main() {
  val items = listOf("なにぃっ？", "リュウがいない", "いったいどこへ…")

  // 答え
  for(a in items) {
    println(a)
  }
}
{% endcapture %}
{% include kotlin_quote.html body=for_basic3 %}

**課題: itemsの中身を全部合計した値を求めよ**

sumに要素を足していきます。

変数名はnにしていますが、aでもいいです。数字はnにする事が多い。

{% capture for_basic4 %}
fun main() {
  val items = listOf(1, 2, 3, 4, 5)
  var sum = 0

  // TODO: ここにfor文を書いて合計をsumに入れよ
  // 答え
  for(n in items) {
    sum += n
  }


  // TODO: 以下はいじらない
  println(sum)

}
{% endcapture %}
{% include kotlin_quote.html body=for_basic4 %}


**課題: itemsの中身を全部合計した値を出力せよ**

今度は変数も自分で作って、それをprintlnするのも自分で書いてください。

{% capture for_basic5 %}
fun main() {
  val items = listOf(5, 6, 7, 8)

  // TODO:ここでfor文を書いて合計を変数に入れ
  // 答え
  var sum = 0
  for(n in items) {
    sum += n
  }

  // TODO:ここでprintlnして
  // 答え2
  println(sum)
}
{% endcapture %}
{% include kotlin_quote.html body=for_basic5 %}


**課題: itemsの中身を全部つなげた文字列を作れ**

今度は文字列です。全部をつなげた文字列を作ってください。

解答例では変数名をsにしていますがaでもいいです。文字列はsにする事が多い。（String型だから）

{% capture for_basic6 %}
fun main() {
  val items = listOf("これを", "全部", "つなげる")

  var kotae = ""

  // TODO:ここでfor文を書いてkotaeにitemsの中身を全部つなげる
  // 答え
  for(s in items) {
    kotae += s
  }

  // 以下はいじらない
  println(kotae)

}
{% endcapture %}
{% include kotlin_quote.html body=for_basic6 %}

**課題: itemsの中身を全部つなげた文字列を作って出力せよ**

今度は変数も自分で作って、それをprintlnするのも自分で書いてください。

{% capture for_basic7 %}
fun main() {
  val items = listOf("今度も", "全部", "つなげる")

  // TODO:ここでfor文を書いてitemsの中身を全部つなげる
  // 答え
  var all = ""
  for(s in items) {
    all += s
  }

  // TODO:ここでつなげた文字の入った変数をprintlnする
  // 答え2
  println(all)
}
{% endcapture %}
{% include kotlin_quote.html body=for_basic7 %}


**課題: 1から10までprintlnせよ**

1から10までのrangeをfor文で回してprintlnします。

{% capture for_range7 %}
fun main() {
  // 答え
  for(a in 1..10) {
    println(a)
  }
}
{% endcapture %}
{% include kotlin_quote.html body=for_range7 %}


**課題: 1から10まで足した結果を求めよ**

解答では数字はiにしていますがnでもいいです。数字はiもよく使います（特に1から順番に大きくなるような時はiという変数名を使う事が多い）。

{% capture for_range7 %}
fun main() {
  var sum = 0

  // TODO: 以下で1から10まで足す
  // 答え
  for(i in 1..10) {
    sum += i
  }

  println(sum)
}
{% endcapture %}
{% include kotlin_quote.html body=for_range7 %}

**課題: 1から100まで足した結果を求めよ**

{% capture for_range8 %}
fun main() {
  // 答え
  var sum = 0
  for(i in 1..100) {
    sum += i
  }
  println(sum)
}
{% endcapture %}
{% include kotlin_quote.html body=for_range8 %}

**課題: itemsを逆順に取り出してprintlnせよ**

ここでは変数sに入れましたが、直接printlnしてもOKです。

{% capture for_range9 %}
fun main() {
  val items = listOf("ころしてでもうばいとる", "そうかんけいないね", "ついにねんがんのアイスソードをてにいれたぞ")
  // 答え
  for(i in 0..2) {
    val s = items[2-i]
    println(s)
  }
}
{% endcapture %}
{% include kotlin_quote.html body=for_range9 %}

**課題: itemsを逆順につなげてprintlnせよ**

{% capture for_range10 %}
fun main() {
  val items = listOf("ころしてでもうばいとる", "そうかんけいないね", "ついにねんがんのアイスソードをてにいれたぞ")
  var concatted = ""
  // TODO: itemsの中を逆順にとりだしてつなげて、concattedに入れる
  // 答え
  for(i in 0..2) {
    concatted += items[2-i]
  }

  println(concatted)

}
{% endcapture %}
{% include kotlin_quote.html body=for_range10 %}

