---
title: "ツアー副読本：基本型"
layout: page
---

[基本型（ツアー） - Kotlin 日本語リファレンス](https://karino2.github.io/kotlin-web-site-ja/docs/kotlin-tour-basic-types.html)を読んでいく時の補助教材です。

## 知っておきたいこと

ツアーの基本型を見ると、型のテーブルがあって、たくさんの型が出てきます。
Byte型、UByte型、Char型など、聞いたこと無いのがぶわっと出てきて、
こんなの全然分からんがな、となるかもしれませんが、これを全部ちゃんとわかっている必要はありません。

一方でわかっておいて欲しいこともあります。
わかって欲しいことは以下です。

- Int, String, Long, Boolean, Doubleを分かっている
- これらが型であることが分かっている
- 型を指定した変数の作り方が分かる（`var a: Int = 0`などのようにコロンで後ろに型を付ける）
- IntとLongのようなサイズ違いが他にByte, Shortというのがあるらしいと分かる
- UByteとかCharとか知らない型もあるらしいが、これらが型だということが分かる

まずはこの位が分かっていれば良いでしょう。
以下補足していきます。

## 様々なサイズの整数

整数というと普通`Int`です。そして[日付とDate](date_intro.md)の所で`Long`も少し出てきました。

整数にはそれをコンピュータ上で扱う時に、どれだけの容量を使うか、に応じてKotlinには4種類の整数があります。

- `Byte`: 8ビットの整数
- `Short`: 16ビットの整数
- `Int`: 32ビットの整数
- `Long`: 64ビットの整数

コンピュータでは数字を2進数で扱い、その桁数をビットと呼びますが、
このビットをどれだけ使うか、で4種類の整数の型があります。

ここで重要なのはこれらの数字を全部覚えることでは無くて、

- 整数を格納するのに使うビット数が違うこと
- 大きさの順番は `Long` ＞　`Int` ＞ `Short` ＞ `Byte` の順番であること

の２つを覚えることです。

しかも`Short`と`Byte`はほとんど使う機会は無いので、
出てきた時にこのページを思い出してその時見直せば十分です。

`Byte`は-128〜127までの数字を格納出来ます。
昔のFFなどで255が最大値なのもこの-128〜127で表される範囲が256なことから来ています。
FF4などでは255は「た5」になる、というのは有名なバグですね。アラームを「た5」にしてプリンプリンセスを倒し続けた幼少期を過ごした人も居るでしょう。

そういう訳で`Byte`は127が最大値なのでかなり小さい数字しか格納出来ません。

`Short`が-3万から3万くらいの数字が入り、`Int`以上はすごく大きい数字となります。
これらの範囲を覚える必要はありませんが、収められる数字の範囲が違うんだな、くらいには理解しておいてください。

{% capture comment1 %}
**何故「た5」なのか？**  
KotlinもAndroidも関係無い話ではありますが、そんなに難しい話でも無いので。
アイテムの個数は0〜255の数字(Kotlinでは'UByte'に相当するも)の変数に入れていたのだろうけれど、
アイテムが99個になったらそれ以上には増えないように処理が入っていたのだと思います。

例えば個数が53個の時、10の位の5と1の位の3に分けて、それぞれ`"0"〜"9"`の文字を表示するプログラムになっていたのだと思われますが、
バグで個数が100個を超える、例えば123個になると10の位が12になって`"0"〜"9"`の範囲に収まらなくなります。
するとその隣にあった"あ"から順番に表示されてしまったのだと思います。

つまり0だと"0"が、1だと"1"が…と9だと"9"が表示されるのですが、10だと"あ"が間違えて表示されていまう。
そもそも10なんて来ないはずなので気にして無かったのでしょう。
10が"あ"、11が"い"、と数えていくと、14で"お"、15で"か"、20で"さ"、25で"た"になります。

”た"と"5”は、25と5になり、255個を表すのでしょうね。
{% endcapture %}
{% include myquote.html body=comment1 %}


## 浮動小数点のDouble

Kotlinでは整数といえばだいたい`Int`です。
そして小数といえばだいたい`Double`です。
そこで`Double`は知っておきたい。

これまであまり出てきませんでしたが、小数を扱うのがDouble型です。

{% capture double-ex1 %}
fun main() {
  // こう書くとaはDouble型
  val a = 1.2

  println(a / 2.0)
  println(1/2) // こちらは整数
}
{% endcapture %}
{% include kotlin_quote.html body=double-ex1 %}

`1`と書くと`Int`型になり、`1.0`と書くと`Double`型になります。
`1.1`とか`2.3`とかも`Double`型です。

そして小数のことを、「浮動小数点」と呼びます。
名前がなんか難しそうですが、これは小数のこと、とおぼえてください。

平均を求める時なんかは`Double`を良く使います。
例えば体重を毎日測って

- 月曜: 62.7kg
- 火曜: 63.4kg
- 水曜: 63.1kg
- 木曜: 62.9kg

だった場合、平均は以下のように求めることが出来ます。

{% capture double-ex2 %}
fun main() {

  val taijuu = listOf(62.7, 63.4, 63.1, 62.9)

  var sum = 0.0

  for(one in taijuu) {
    sum += one
  }

  val heikin = sum/taijuu.size

  println("平均の体重は${heikin}です")
}
{% endcapture %}
{% include kotlin_quote.html body=double-ex2 %}

少し練習問題をやっておきましょう。

**課題1: 上と同じことを自分で1から書け**

とりあえず自分で書いてみないと分からないと思うので、上と全く同じことを自分で書いてみてください。

{% capture double-q1 %}
fun main() {

  val taijuu = listOf(62.7, 63.4, 63.1, 62.9)

  // TODO: 以下にfor文などを書いて、heikinをちゃんと平均にせよ
  val heikin = 0.0

  println("平均の体重は${heikin}です")
}
{% endcapture %}
{% include kotlin_quote.html body=double-q1 %}

**課題2: 以下の買い物をした時の消費税を求めよ**

消費税の計算などにはDoubleが便利です。最後は本当はIntにした方が正しいですが、とりあえずDoubleのままでいいでしょう。（`ToInt()`でIntに出来ます）。

- りんご: 340円
- みかん: 390円
- ジャイアントコーン: 140円

{% capture double-q2 %}
fun main() {

  val kingaku = listOf(340.0, 390.0, 140.0)

  // TODO: 以下にfor文などを書いて、syouhizeiを消費税の額にせよ（合計じゃなくて消費税だけの額で）
  val syouhizei = 0.0

  println("消費税は${syouhizei}円です")
}
{% endcapture %}
{% include kotlin_quote.html body=double-q2 %}

{% capture double-q2-hint %}
まず合計を求めます。それから消費税は10％なので0.1を掛けます。
{% endcapture %}
{% include collapse_quote.html body=double-q2-hint title="ヒント" %}


{% capture double-q2-a %}
```kotlin
fun main() {

  val kingaku = listOf(340.0, 390.0, 140.0)

  // TODO: 以下にfor文などを書いて、syouhizeiを消費税の額にせよ（合計じゃなくて消費税だけの額で）
  var goukei = 0.0
  for(one in kingaku) {
    goukei += one
  }
  val syouhizei = goukei * 0.1

  println("消費税は${syouhizei}円です")
}
```
{% endcapture %}
{% include collapse_quote.html body=double-q2-a title="解答例" %}

**課題3: 以下の買い物をした時の消費税を求めよ**

今度はもっと難しくして、個数と代金を別にします。

- りんご: 120円を4個
- みかん: 47円を10個
- ジャイアントコーン: 137円を3個

買うことにします。
以下の消費税を求めよ。

{% capture double-q3 %}
fun main() {

  val nedan = listOf(120.0, 47.0, 137.0)
  val kosuu = listOf(4, 10, 3)

  // TODO: 以下にfor文などを書いて、syouhizeiを消費税の額にせよ（合計じゃなくて消費税だけの額で）
  val syouhizei = 0.0

  println("消費税は${syouhizei}円です")
}
{% endcapture %}
{% include kotlin_quote.html body=double-q3 %}


{% capture double-q3-hint %}
0, 1, 2のインデックスでループを回します。0..2を使うといいでしょう。
{% endcapture %}
{% include collapse_quote.html body=double-q3-hint title="ヒント" %}


{% capture double-q3-a %}
```kotlin
fun main() {

  val nedan = listOf(120.0, 47.0, 137.0)
  val kosuu = listOf(4, 10, 3)

  // TODO: 以下にfor文などを書いて、syouhizeiを消費税の額にせよ（合計じゃなくて消費税だけの額で）
  var sum = 0.0
  for(i in 0..2) {
    sum += nedan[i]*kosuu[i]
  }
  val syouhizei = sum*0.1

  println("消費税は${syouhizei}円です")
}
```
{% endcapture %}
{% include collapse_quote.html body=double-q3-a title="解答例" %}

**課題4: BMIを計算しよう**

BMIというのは、Body Mass Indexの略で、どのくらいデブかを示す指標で、身長と体重から計算します。
（なお成人向けなので小中学生は別の指数があるらしいです。詳しくないのでとりあえず大人向けで計算します）

[ＢＭＩと適正体重 - 高精度計算サイト](https://keisan.casio.jp/exec/system/1161228732#!)

計算式は以下です。

```
BMI = 体重kg/ (身長m * 身長m)
```

ここで`*`はプログラムでは掛け算を表し、`/`は割り算を表します。

注意するのは、身長がメートルなことです。
171cmなら1.71mとする必要があります。

身長170cmで体重63kgなら、21.8が答えになります。

プログラム上ではあえて身長はcmにしてあるので、ちゃんとmに換算して計算してください。

{% capture double-q4 %}
fun main() {

  val taijuu = 63.0
  val sinchou = 170.0

  // TODO: 以下にBMIを計算せよ
  val BMI = 0.0

  println("身長: ${sinchou}、体重： ${taijuu} のBMIは ${BMI} です。")
}
{% endcapture %}
{% include kotlin_quote.html body=double-q4 %}

{% capture double-q4-hint %}
身長をメートルにしたsinchouM変数を作るといいでしょう。
{% endcapture %}
{% include collapse_quote.html body=double-q4-hint title="ヒント" %}


{% capture double-q4-a %}
```kotlin
fun main() {

  val taijuu = 63.0
  val sinchou = 170.0

  // TODO: 以下にBMIを計算せよ
  val sinchouM = sinchou/100.0
  val BMI = taijuu/(sinchouM*sinchouM)

  println("身長: ${sinchou}、体重： ${taijuu} のBMIは ${BMI} です。")
}
```
{% endcapture %}
{% include collapse_quote.html body=double-q4-a title="解答例" %}

**課題5: BMIから、痩せすぎ、太り過ぎ、普通を判定しよう**

BMIは大雑把には

- 18.5未満 ... 痩せすぎ
- 18.5以上〜25未満 ... 普通
- 25以上 ... 太り過ぎ

となります。
そこでBMIを計算し、上のそれぞれの範囲で

- `痩せすぎ`
- `普通`
- `太り過ぎ`

と表示するようにしましょう。

{% capture double-q5 %}
fun main() {

  val taijuu = 63.0
  val sinchou = 170.0

  // TODO: 以下にBMIを計算せよ（前問と同じ）
  val BMI = 0.0

  // TODO: 以下でBMIに応じて、printlnせよ
}
{% endcapture %}
{% include kotlin_quote.html body=double-q5 %}

{% capture double-q5-hint %}
ifとelse ifを使います。`BMI < 18.5`, `BMI < 25`と順番にチェックしていく感じです。
{% endcapture %}
{% include collapse_quote.html body=double-q5-hint title="ヒント" %}

{% capture double-q5-a %}
```kotlin
fun main() {

  val taijuu = 63.0
  val sinchou = 170.0

  // TODO: 以下にBMIを計算せよ（前問と同じ）
  val sinchouM = sinchou/100.0
  val BMI = taijuu/(sinchouM*sinchouM)

  // TODO: 以下でBMIに応じて、printlnせよ
  if (BMI < 18.5) {
    println("痩せすぎ")
  } else if( BMI < 25.0) {
    println("普通")
  } else {
    println("太り過ぎ")
  }
}
```
{% endcapture %}
{% include collapse_quote.html body=double-q5-a title="解答例" %}

### IntとDoubleを行ったり来たりする

たまに両者を行ったり来たりする必要があります。
これは`String`と`Int`の間を行ったり来たりする必要があるのに似ています。

`Double`にするには`toDouble()`を使います。これはStringにもIntにも使えます。

また、`Double`から`Int`にするには`toInt()`を使います。小数点以下は切り捨てです。

{% capture todouble-ex1 %}
fun main() {
  val a = 123

  val b = a.toDouble()
  println(b/2)

  val c = "1.5"
  val d = c.toDouble()
  println(d*2)

  val e = (d*3).toInt()
  println(e)
}
{% endcapture %}
{% include kotlin_quote.html body=todouble-ex1 %}

また、`Int`と`Double`を掛けると`Double`になり、`Int`と`Double`を足しても`Double`になります。
どちらかが`Double`だったら結果も`Double`になります。

{% capture todouble-ex2 %}
fun main() {

  val a = 123
  val b = a / 2.0 // bはDouble
  println(b)

  val c = 123 * 1.5 // dはDouble
  println(c)
}
{% endcapture %}
{% include kotlin_quote.html body=todouble-ex2 %}

少しなれるために、先程の課題をIntと行ったり来たりするバージョンでやってみましょう。

**課題6: 以下の買い物をした時の消費税を求めよ**

課題2と同じ以下の計算を行いますが、最初のkingakuが`Int`になっているので計算する時に`Double`にして計算してください。
また、最後の結果は`Int`として小数点以下を切り捨てて表示してください。

- りんご: 340円
- みかん: 390円
- ジャイアントコーン: 140円

{% capture double-q6 %}
fun main() {

  val kingaku = listOf(340, 390, 140)

  // TODO: 以下にfor文などを書いて、syouhizeiを消費税の額にせよ（合計じゃなくて消費税だけの額で）
  // syouhizei変数の型はIntになるようにせよ
  val syouhizei = 0

  println("消費税は${syouhizei}円です")
}
{% endcapture %}
{% include kotlin_quote.html body=double-q6 %}

{% capture double-q6-hint %}
この問題は合計まではIntのままでOKです（Doubleにしても良い）。
そこに0.1を掛けると結果が自動でDoubleになるので実は`toDouble()`が要らない。
最後の結果は`toInt()`したいので、一旦syouhizeiDという`Double`の変数で求めたあとに、
`syouhizei`という変数を`toInt()`してつくる。
{% endcapture %}
{% include collapse_quote.html body=double-q6-hint title="ヒント" %}


{% capture double-q6-a %}
```kotlin
fun main() {

  val kingaku = listOf(340, 390, 140)

  // TODO: 以下にfor文などを書いて、syouhizeiを消費税の額にせよ（合計じゃなくて消費税だけの額で）
  var goukei = 0 // Intで合計を求める
  for(one in kingaku) {
    goukei += one
  }
  val syouhizeiD = goukei * 0.1 // syouhizeiはこれでDoubleになる
  val syouhizei = syouhizeiD.toInt()

  println("消費税は${syouhizei}円です")
}
```
{% endcapture %}
{% include collapse_quote.html body=double-q6-a title="解答例" %}

**課題7: 以下の買い物をした時の消費税を求めよ**

個数と代金を別にした版で同様の問題を解きます。結果は`Int`で出力。

- りんご: 120円を4個
- みかん: 47円を10個
- ジャイアントコーン: 137円を3個

買うことにします。
以下の消費税を求めよ。

{% capture double-q7 %}
fun main() {

  val nedan = listOf(120, 47, 137)
  val kosuu = listOf(4, 10, 3)

  // TODO: 以下にfor文などを書いて、syouhizeiを消費税の額にせよ（合計じゃなくて消費税だけの額で）
  val syouhizei = 0

  println("消費税は${syouhizei}円です")
}
{% endcapture %}
{% include kotlin_quote.html body=double-q7 %}

{% capture double-q7-a %}
```kotlin
fun main() {

  val nedan = listOf(120, 47, 137)
  val kosuu = listOf(4, 10, 3)

  // TODO: 以下にfor文などを書いて、syouhizeiを消費税の額にせよ（合計じゃなくて消費税だけの額で）
  var sum = 0
  for(i in 0..2) {
    sum += nedan[i]*kosuu[i]
  }
  val syouhizeiD = sum*0.1
  val syouhizei = syouhizeiD.toInt()

  println("消費税は${syouhizei}円です")
}
```
{% endcapture %}
{% include collapse_quote.html body=double-q7-a title="解答例" %}

### `Double`と`Float`

整数にも`Int`の大きいバージョンの`Long`とか小さいバージョンの`Byte`などがあったように、
`Double`にも小さいバージョンの`Float`というのがあります。

小さいバージョンだがだいたい同じもの、とだけなんとなく覚えておけば良いでしょう。
Androidのグラフィックス周りではたまに`Float`を要求するものがあるので、
それが出てきた時に調べれば十分です。