---
title: "コレクション入門、後編"
layout: page
---
for文が使えるといろいろと出来る事が増えるので、ここでコレクション周りの使える奴らを見ていきます。

## 個数は.count()で取る

リストの個数は.count()で取れます。

{% capture for_count1 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  println(youbi.count())
}
{% endcapture %}
{% include kotlin_quote.html body=for_count1 %}



## 添字はindicesかwithIndex()

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
