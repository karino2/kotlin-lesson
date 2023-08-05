---
title: "forループ入門"
layout: page
---
コレクションの説明を書こうと思ったら、for文が無いときつい事に気づいたのでfor文を解説する事に。

Listの要素を一つずつ取り出したりするのがfor文です。

{% capture for_basic1 %}
fun main() {
  val items = listOf("ひとつ!", "ふたーつ!", "みっつ！")

  for(a in items) {
    println(a)
  }
}
{% endcapture %}
{% include kotlin_quote.html body=for_basic1 %}

文法としては以下のようになります。

```
for(変数名 in コレクション) {

}
```

変数名というところでつけた名前が、このforの中括弧で囲まれた範囲（ブロックといいます）でだけ使えるようになります。
新しい変数を作るのにvalやvarを作らない珍しいケースですね。

コレクションのところは、リストかRangeというものと覚えておくと最初はいいと思う。Rangeについては後で説明します。

こういうのは何度も書いて覚えるのが一番！という事で自分で書いてみましょう。

**itemsを一つずつ取り出してprintlnするコードを書け**

{% capture for_basic2 %}
fun main() {
  val items = listOf("ひとつ!", "ふたーつ!", "みっつ！")

}
{% endcapture %}
{% include kotlin_quote.html body=for_basic2 %}

**itemsを一つずつ取り出してprintlnするコードを書け**
{% capture for_basic3 %}
fun main() {
  val items = listOf("なにぃっ？", "リュウがいない", "いったいどこへ…")

}
{% endcapture %}
{% include kotlin_quote.html body=for_basic3 %}

**itemsの中身を全部合計した値を求めよ**

sumに要素を足していきます。

{% capture for_basic4 %}
fun main() {
  val items = listOf(1, 2, 3, 4, 5)
  var sum = 0

  // TODO: ここにfor文を書いて合計をsumに入れよ


  // TODO: 以下はいじらない
  println(sum)

}
{% endcapture %}
{% include kotlin_quote.html body=for_basic4 %}


**itemsの中身を全部合計した値を出力せよ**

今度は変数も自分で作って、それをprintlnするのも自分で書いてください。

{% capture for_basic5 %}
fun main() {
  val items = listOf(5, 6, 7, 8)

  // TODO:ここでfor文を書いて合計を変数に入れ

  // TODO:ここでprintlnして
}
{% endcapture %}
{% include kotlin_quote.html body=for_basic5 %}


**itemsの中身を全部つなげた文字列を作れ**

今度は文字列です。全部をつなげた文字列を作ってください。
なお、この文字列のつなげかたはあまり良くないのですが、それは終盤でたぶん解説します。

{% capture for_basic6 %}
fun main() {
  val items = listOf("これを", "全部", "つなげる")

  var kotae = ""

  // TODO:ここでfor文を書いてkotaeにitemsの中身を全部つなげる

  // 以下はいじらない
  println(kotae)

}
{% endcapture %}
{% include kotlin_quote.html body=for_basic6 %}

**itemsの中身を全部つなげた文字列を作って出力せよ**

今度は変数も自分で作って、それをprintlnするのも自分で書いてください。

{% capture for_basic7 %}
fun main() {
  val items = listOf("今度も", "全部", "つなげる")

  // TODO:ここでfor文を書いてitemsの中身を全部つなげる

  // TODO:ここでつなげた文字の入った変数をprintlnする
}
{% endcapture %}
{% include kotlin_quote.html body=for_basic7 %}

## Range入門

for文のコレクションのところ、

```
for(変数名 in コレクション) {

}
```

には、Rangeというものも指定出来ます。
Rangeは0から5まで、とかそういうの。

Rangeは`0..5`のように書きます。例えば以下。

{% capture for_range1 %}
fun main() {
  for(num in 0..5) {
    println(num)
  }
}
{% endcapture %}
{% include kotlin_quote.html body=for_range1 %}

この`0..5`というのがRangeです。なお、5を含めたくない場合は`..<`というのが使えます。

{% capture for_range2 %}
fun main() {
  for(num in 0..<5) {
    println(num)
  }
}
{% endcapture %}
{% include kotlin_quote.html body=for_range2 %}

そんなの`0..4`と書けば一緒じゃない？というのは一緒なんですが、コレクションのcountとかと組み合わせる時に便利なので、
そんなのあるんだ、とおぼえておくと良いです。

別にこのnumは使わなくてもいい。同じ事を三回出力する、みたいな時は以下のように書きます。

{% capture for_range3 %}
fun main() {
  for(num in 1..3) {
    println("大事な事なので3回言いました")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=for_range3 %}

カウントダウンのような事をしたければ引き算で良いでしょう。

{% capture for_range4 %}
fun main() {
  for(num in 0..3) {
    println(num-3)
  }
}
{% endcapture %}
{% include kotlin_quote.html body=for_range4 %}

一旦変数にいれてもいいです。

{% capture for_range5 %}
fun main() {
  for(num in 0..3) {
    val num2 = num-3
    println(num2)
  }
}
{% endcapture %}
{% include kotlin_quote.html body=for_range5 %}

リストを逆順に出力したい時にも使えない事も無い（後編でindicesなども説明するけれど）

{% capture for_range6 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in 0..6) {
    println(youbi[6-a])
  }
}
{% endcapture %}
{% include kotlin_quote.html body=for_range6 %}

こういう時に6なのか7なのかとかはややこしいね。

いくつか自分で書いてみましょう。

**1から10までprintlnせよ**

1から10までのrangeをfor文で回してprintlnします。

{% capture for_range7 %}
fun main() {
}
{% endcapture %}
{% include kotlin_quote.html body=for_range7 %}


**1から10まで足した結果を求めよ**

1から10まで足すといくつになるか、高校生で等差数列の和の公式というのを習いますが、
プログラマは何も考えずにコンピュータに足させます。

{% capture for_range7 %}
fun main() {
  var sum = 0

  // TODO: 以下で1から10まで足す

  println(sum)
}
{% endcapture %}
{% include kotlin_quote.html body=for_range7 %}

**1から100まで足した結果を求めよ**

一緒やん、と思うかもしれんけど、100まで足した結果も求めておこう。
変数sumも自分で作って。

{% capture for_range8 %}
fun main() {
}
{% endcapture %}
{% include kotlin_quote.html body=for_range8 %}

**itemsを逆順に取り出してprintlnせよ**

{% capture for_range9 %}
fun main() {
  val items = listOf("ころしてでもうばいとる", "そうかんけいないね", "ついにねんがんのアイスソードをてにいれたぞ")
}
{% endcapture %}
{% include kotlin_quote.html body=for_range9 %}

**itemsを逆順につなげてprintlnせよ**

{% capture for_range10 %}
fun main() {
  val items = listOf("ころしてでもうばいとる", "そうかんけいないね", "ついにねんがんのアイスソードをてにいれたぞ")
  var concated = ""
  // TODO: itemsの中を逆順にとりだしてつなげて、conctedに入れる

  println(concated)

}
{% endcapture %}
{% include kotlin_quote.html body=for_range10 %}