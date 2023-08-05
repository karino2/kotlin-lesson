---
title: "コレクション入門、後編"
layout: page
---
for文が使えるといろいろと出来る事が増えるので、ここでコレクション周りの使える奴らを見ていきます。

## 個数は.count()で取る

リストの個数は.count()で取れます。

{% capture count_code %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  println(youbi.count())
}
{% endcapture %}
{% include kotlin_quote.html body=count_code %}

個数は7だけど、`youbi[7]`は最後「の次」の要素にアクセスしようとしてエラーになるので中止。`youbi[6]`までしかアクセス出来ない。
つまりcountの結果をアクセスするのはエラー。

{% capture count_code2 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  // エラー
  val hoge = youbi[youbi.count()]
  println(hoge)
}
{% endcapture %}
{% include kotlin_quote.html body=count_code2 %}

`youbi.count()`は7なので、`youbi[youbi.count()]`は`youbi[7]`になってしまって、範囲外アクセスとなってしまう。
最後の要素は`yougi[6]`なので、1を引く以下のコードは動く。

{% capture count_code3 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  // これはオーケー。最後の要素。
  val hoge = youbi[youbi.count()-1]
  println(hoge)
}
{% endcapture %}
{% include kotlin_quote.html body=count_code3 %}

ややこしいですね。
なお、最後の要素を取り出すのに1を引くのを忘れるのが良くあるので、最後の要素を取る、`.last()`というのがあります。

{% capture count_code4 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  val hoge = youbi.last()
  println(hoge)
}
{% endcapture %}
{% include kotlin_quote.html body=count_code4 %}

こちらの方がミスは少ないね。

## 添字はindicesかwithIndex()

以下のようなものがあったとします。

{% capture index_code1 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in youbi) {
    println(a)
  }
}
{% endcapture %}
{% include kotlin_quote.html body=index_code1 %}

ここで、この出力を「月曜、水曜、金曜、日曜」と一日おきにするにはどうしたらいいかを考えます。

もちろんここまでの知識を使って、Booleanのフラグを使って以下のように書けば書けそうですが

{% capture index_code2 %}
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
{% include kotlin_quote.html body=index_code2 %}

これはちょっとややこしい。
あれ？「!toggle」は初めてかな？ビックリマークは反転させる、という機能で、trueがfalse、falseがtrueになります。
まぁいい。

それよりも、こういうのは添字の一覧を回せると良いです。それはindicesというのを使います。
以下のコードを実行してみてください。

{% capture index_code3 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in youbi.indices) { // ここがNew!
    println(a)
  }
}
{% endcapture %}
{% include kotlin_quote.html body=index_code3 %}

これは何か、というと、itemの添字が順番に出るのです。
添字というのは大かっこ（つまり`[]`）で指定する奴です。

つまり、以下のようにすれば全要素が出力出来る。

{% capture index_code4 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in youbi.indices) {
    println(youbi[a])
  }
}
{% endcapture %}
{% include kotlin_quote.html body=index_code4 %}

このように添え字を`youbi[a]`とすれば要素になる訳ですね。

これだけだと前と一緒ですが、例えば逆順に出力するなら以下になります。

{% capture index_code5 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in youbi.indices) {
    println(youbi[6-a])
  }
}
{% endcapture %}
{% include kotlin_quote.html body=index_code5 %}

6から引けばいい。6というのはyoubiの個数-1です。なんで1引くかというと0から始まるから。

リストの個数はcountで取れるので、以下のようにも書ける。

{% capture index_code6 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in youbi.indices) {
    println(youbi[youbi.count()-1-a])
  }
}
{% endcapture %}
{% include kotlin_quote.html body=index_code6 %}

少し読みにくいので変数を作ってもいいかもしれない。

{% capture index_code7 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in youbi.indices) {
    val revIndex = youbi.count()-1-a 
    println(youbi[revIndex])
  }
}
{% endcapture %}
{% include kotlin_quote.html body=index_code7 %}
