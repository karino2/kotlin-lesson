---
title: "List入門"
layout: page
---
for文が使えるといろいろと出来る事が増えるので、ここでListをもうちょっと詳しく見ていきます。

ここでは以下のメソッドなどを見ていきます。

- list.count()
- list.last()
- list.indices
- list.withIndex()

{% capture comment1 %}
**メソッドって何よ？**  
メソッドという言葉が唐突に出てきたかもしれないけれど、これはクラスというものを説明しないと何なのか説明出来ないため、現時点ではメソッドとは何かを説明する事は出来ません。
ですが、呼び名が無いと説明が出来ないので「メソッド」という名前は出させてもらってます。

何か、というと、`.`をつけた後に呼ぶ奴、くらいに覚えてもらっておくと良いです。
厳密にはindicesなどはメソッドでは無くてプロパティなのですが、そもそもメソッドとは何かを説明してないので、
ここでは`.`の後に続く呼びだす系のものをふわっと「メソッド」と呼ぶ事にします。
{% endcapture %}
{% include myquote.html body=comment1 %}

## 個数は.count()で取る

リストの個数は.count()で取れます。

{% capture count_code %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  println(youbi.count())
}
{% endcapture %}
{% include kotlin_quote.html body=count_code %}

個数は7だけど、`youbi[7]`は最後「の次」の要素にアクセスしようとしてエラーになるので注意。`youbi[6]`までしかアクセス出来ない。
つまり以下のようなコードはエラー。

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
youbiの最後の要素は`yougi[6]`なので、1を引く以下のコードは動く。

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
なお、最後の要素を取り出すのに1を引くのを忘れるバグが良くあるので、最後の要素を取る専用の手段、`.last()`というのがあります。

{% capture count_code4 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  val hoge = youbi.last()
  println(hoge)
}
{% endcapture %}
{% include kotlin_quote.html body=count_code4 %}

こちらの方がミスは少ないね。

## 添字はindices

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
あれ？`!toggle`は初めてかな？ビックリマークは反転させる、という機能で、trueがfalse、falseがtrueになります。
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

そしてこれを一つ飛ばしにするなら、[forループ入門](for_loop.md)でやったように`%`を使うのが良さそう。

{% capture index_code5 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in youbi.indices) {
    if(a%2 == 0)
      println(youbi[a])
  }
}
{% endcapture %}
{% include kotlin_quote.html body=index_code5 %}

これで一つ飛ばしに出力出来ました。

**以下のコードを変更し、曜日を2個飛ばしに出力せよ**

曜日を二つ飛ばし、つまり月曜、木曜、日曜と出力されるようにしてください。

{% capture index_code6 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in youbi.indices) {
    // TODO: 以下を変更
    println(youbi[a])
  }
}
{% endcapture %}
{% include kotlin_quote.html body=index_code6 %}


### 逆順にしてみる

インデックスの一覧を回すのを応用すると、逆順に出力したりも出来ます。

{% capture revindex_code1 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in youbi.indices) {
    println(youbi[6-a])
  }
}
{% endcapture %}
{% include kotlin_quote.html body=revindex_code1 %}

6から引けばいい。6というのはyoubiの個数-1です。なんで1引くかというと0から始まるから。

リストの個数はcountで取れるので、以下のようにも書ける。

{% capture revindex_code2 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in youbi.indices) {
    println(youbi[youbi.count()-1-a])
  }
}
{% endcapture %}
{% include kotlin_quote.html body=revindex_code2 %}

少し読みにくいので変数を作ってもいいかもしれない。

{% capture revindex_code3 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in youbi.indices) {
    val revIndex = youbi.count()-1-a 
    println(youbi[revIndex])
  }
}
{% endcapture %}
{% include kotlin_quote.html body=revindex_code3 %}

少し練習問題をやってみましょう。

**日曜、金曜、水曜...と逆順の一つ飛ばしでprintlnせよ**

以下のコードを変更して、逆順でさらに一つ飛ばしになるようにしてみましょう。
一つ飛ばしは[forループ入門](for_loop.md)を参考に。

{% capture revindex_code4 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in youbi.indices) {
    // TODO: 以下を修正せよ
    println(a)
  }
}
{% endcapture %}
{% include kotlin_quote.html body=revindex_code4 %}

## 添字と要素を同時に取る、withIndex()

