---
title: "コレクション入門、前篇"
layout: page
---
ListとMapについて書く。Setもちょっと書くかも。

## List入門

ListはlistOfで作り`[]`でアクセスします。

{% capture list_basic %}
fun main() {
  val items = listOf("ひとつ!", "ふたーつ!", "みっつ！")

  println(items[1])
}
{% endcapture %}
{% include kotlin_quote.html body=list_basic %}

### 百本ノックを10個くらいやってみよう

この手のものはたくさん書いて慣れるに尽きます。

[第6.1回: 配列100本ノック - 算数で挫折した人向けの、Javascript入門](https://karino2.github.io/js-introduction/ch06_1.html)と同じようなことをやってみましょう。
問題作るのかったるいので、向こう見ながら自分でやってみてください。もういいかな、と思うくらいまでやって先に進みましょう。

**以下のリストを生成せよ**

1. "むぇ～～～"
2. "コケー"
3. "ダネ～～"

{% capture list_create %}
fun main() {
  val kotae = 0

  println(kotae)
}
{% endcapture %}
{% include kotlin_quote.html body=list_create %}


**「"コケー"」 を取り出せ**

kotaeに入れてね。

{% capture list_access %}
fun main() {
  val list = listOf("むぇ～～～","コケー","ダネ～～")
  val kotae = 0

  println(kotae)
}
{% endcapture %}
{% include kotlin_quote.html body=list_access %}

こっち系の問題は100本ノックの違う問題をやる時はlistOfから書かないといけないけれど、練習になって良いでしょう。

## Map入門

MapはmapOfとtoで作り`[]`でアクセスします。

{% capture map_basic %}
fun main() {
  val charaNum = mapOf("ストII" to 8,
                       "ストII'" to 12,
                       "スパII" to 16,
                      "餓狼伝説" to 3)

  println(charaNum["スパII"])
  println(charaNum["餓狼伝説"])
  println(charaNum["ストII"])
}
{% endcapture %}
{% include kotlin_quote.html body=map_basic %}

### 百本ノックを10個くらいやってみよう

ということでMapも[第6.2回: 辞書100本ノック - 算数で挫折した人向けの、Javascript入門](https://karino2.github.io/js-introduction/ch06_2.html)のうち、
パターン1とパターン2だけ飽きるまでやってみてください。
パターン3はmutableが出てくるので後回しで。

**以下の辞書を生成せよ**

|キー	| 要素 |
| ----| ---- |
| るーしー | 15014 |
| ダニエル | 12518 |

{% capture map_create %}
fun main() {
  val kotae = 0

  println(kotae)
}
{% endcapture %}
{% include kotlin_quote.html body=map_create %}


**mapから「15014」を取り出せ**

{% capture map_create %}
fun main() {
  val map = mapOf("るーしー" to 15014,
                  "ダニエル" to 12518)
  val kotae = 0

  println(kotae)
}
{% endcapture %}
{% include kotlin_quote.html body=map_create %}

----

## 以下はあとで後編に移動します


### 添字はindicesかwithIndex()

以下のようなものがあったとします。

{% capture for_index1 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in youbi) {
    println(a)
  }
}
{% endcapture %}
{% include kotlin_quote.html body=for_index1 %}

ここで、この出力を「月曜、水曜、金曜、日曜」と一日おきにするにはどうしたらいいかを考えます。

もちろんここまでの知識を使って、Booleanのフラグを使って以下のように書けば書けそうですが

{% capture for_index2 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")
  var toggle = true

  for(a in youbi) {
    if(toggle)
      println(a)
    toggle = !toggle
  }
}
{% endcapture %}
{% include kotlin_quote.html body=for_index2 %}

これはちょっとややこしい。
あれ？「!toggle」は初めてかな？ビックリマークは反転させる、という機能で、trueがfalse、falseがtrueになります。
まぁいい。

それよりも、こういうのは添字の一覧を回せると良いです。それはindicesというのを使います。
以下のコードを実行してみてください。

{% capture for_index3 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in youbi.indices) { // ここがNew!
    println(a)
  }
}
{% endcapture %}
{% include kotlin_quote.html body=for_index3 %}

これは何か、というと、itemの添字が順番に出るのです。
添字というのは大かっこ（つまり`[]`）で指定する奴です。

つまり、以下のようにすれば全要素が出力出来る。

{% capture for_index4 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in youbi.indices) {
    println(youbi[a])
  }
}
{% endcapture %}
{% include kotlin_quote.html body=for_index4 %}

このように添え字を`youbi[a]`とすれば要素になる訳ですね。

これだけだと前と一緒ですが、例えば逆順に出力するなら以下になります。

{% capture for_index5 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in youbi.indices) {
    println(youbi[7-a])
  }
}
{% endcapture %}
{% include kotlin_quote.html body=for_index5 %}

7から引けばいい。7というのはyoubiの個数です。リストの個数はcountで取れるので、以下のようにも書ける。

{% capture for_index6 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in youbi.indices) {
    println(youbi[youbi.count()-a])
  }
}
{% endcapture %}
{% include kotlin_quote.html body=for_index6 %}
