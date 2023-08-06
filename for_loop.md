---
title: "forループ入門"
layout: page
---
ループと条件分岐（ifの事）がプログラムの基本要素です。
という事でここでforループをマスターすると、プログラム完全に理解した、と言えます（言えない）。

という事でここではforループを見ていきましょう。

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

**課題: itemsを一つずつ取り出してprintlnするコードを書け**

{% capture for_basic2 %}
fun main() {
  val items = listOf("ひとつ!", "ふたーつ!", "みっつ！")

}
{% endcapture %}
{% include kotlin_quote.html body=for_basic2 %}

**課題: itemsを一つずつ取り出してprintlnするコードを書け**
{% capture for_basic3 %}
fun main() {
  val items = listOf("なにぃっ？", "リュウがいない", "いったいどこへ…")

}
{% endcapture %}
{% include kotlin_quote.html body=for_basic3 %}

**課題: itemsの中身を全部合計した値を求めよ**

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


**課題: itemsの中身を全部合計した値を出力せよ**

今度は変数も自分で作って、それをprintlnするのも自分で書いてください。

{% capture for_basic5 %}
fun main() {
  val items = listOf(5, 6, 7, 8)

  // TODO:ここでfor文を書いて合計を変数に入れ

  // TODO:ここでprintlnして
}
{% endcapture %}
{% include kotlin_quote.html body=for_basic5 %}


**課題: itemsの中身を全部つなげた文字列を作れ**

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

**課題: itemsの中身を全部つなげた文字列を作って出力せよ**

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

**課題: 1から10までprintlnせよ**

1から10までのrangeをfor文で回してprintlnします。

{% capture for_range7 %}
fun main() {
}
{% endcapture %}
{% include kotlin_quote.html body=for_range7 %}


**課題: 1から10まで足した結果を求めよ**

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

**課題: 1から100まで足した結果を求めよ**

一緒やん、と思うかもしれんけど、100まで足した結果も求めておこう。
変数sumも自分で作って。

{% capture for_range8 %}
fun main() {
}
{% endcapture %}
{% include kotlin_quote.html body=for_range8 %}

**課題: itemsを逆順に取り出してprintlnせよ**

{% capture for_range9 %}
fun main() {
  val items = listOf("ころしてでもうばいとる", "そうかんけいないね", "ついにねんがんのアイスソードをてにいれたぞ")
}
{% endcapture %}
{% include kotlin_quote.html body=for_range9 %}

**課題: itemsを逆順につなげてprintlnせよ**

{% capture for_range10 %}
fun main() {
  val items = listOf("ころしてでもうばいとる", "そうかんけいないね", "ついにねんがんのアイスソードをてにいれたぞ")
  var concatted = ""
  // TODO: itemsの中を逆順にとりだしてつなげて、concattedに入れる

  println(concatted)

}
{% endcapture %}
{% include kotlin_quote.html body=for_range10 %}

### 一つ飛ばしで辿るのはパーセント（%）を使う

唐突ですがkotlinには、通常の足し算、引き算、掛け算、割り算 の他に、剰余と言われるものがあります。
これは割り算の「あまり」を求めるもので、`%`を使います。

以下例を見てみましょう。

{% capture for_modulo1 %}
fun main() {
  // これは割り算
  println(10/3)

  // これは剰余
  println(10%3)

  // いろいろな余り
  println(4%2)
  println(3%2)
  println(10%7)
  println(1234%7)
}
{% endcapture %}
{% include kotlin_quote.html body=for_modulo1 %}

これだけだと何に使うんだ？って感じですが、一つ飛ばしとか3回に一回とかをやりたい時に便利です。

{% capture for_modulo2 %}
fun main() {
  for(i in 0..10) {
    if (i%2 == 0)
      println("$i は偶数です")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=for_modulo2 %}

`$i`に関しては[文字列入門](string_intro.md)の「String template」を参照。
2で割った余りが0というのは偶数という意味ですが、ようするに0, 2, 4, 6, 8と進むという事です。

もうひとつ例を見てみましょう。
月曜から日曜までのリストがあったとして、一日飛ばしに、「月曜、水曜、金曜、日曜」をプリントしたいとします。
すると、以下のようになります。

{% capture for_modulo3 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in 0..6) {
    if (a % 2 == 0)
      println(youbi[a])
  }
}
{% endcapture %}
{% include kotlin_quote.html body=for_modulo3 %}

火木土にしたいなら、いくつかやり方はありますが、以下とかどうでしょう？

{% capture for_modulo4 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in 0..6) {
    if ((a+1) % 2 == 0)
      println(youbi[a])
  }
}
{% endcapture %}
{% include kotlin_quote.html body=for_modulo4 %}

さらに2つ飛ばしなら以下。

{% capture for_modulo5 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in 0..6) {
    if (a % 3 == 0)
      println(youbi[a])
  }
}
{% endcapture %}
{% include kotlin_quote.html body=for_modulo5 %}

こういう風に、一つずつ交互に何かしたい、みたいな時には剰余、つまり`%`を良く使います。
これは使う時が来たら思い出してここ見直すのがいいでしょう。
