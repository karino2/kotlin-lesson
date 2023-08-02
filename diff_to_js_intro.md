---
title: "「算数で挫折した人向けの、Javascript入門」との差分"
layout: page
---
[算数で挫折した人向けのJavaScript入門 - 算数で挫折した人向けの、Javascript入門](https://karino2.github.io/js-introduction/)を6.3までやってもらったと思うので、
この範囲でkotlinは違う、というところを簡単にまとめておく。

## 細かい話

- セミコロンはいらない
- MessageBox.show()はprintln()

{% capture hyousyousiki_code %}
fun main() {
  println("さぁ、表彰式だ")
  println("なにぃっ！")
}
{% endcapture %}
{% include kotlin_quote.html body=hyousyousiki_code %}


## mainとかいう奴

ここで実験するコードでは、必ず「`fun main() {`」と、その閉じ中カッコで囲む必要がある

```kotlin
fun main() {

}
```

これが何かは後で関数のところまで進んだら説明するので、それまでは必ずテストコードはこれで囲まないといけない、と覚えておいてくれ。

{% capture hello_world_code %}
fun main() {
  println("はろーわーるど")
}
{% endcapture %}
{% include kotlin_quote.html body=hello_world_code %}


## 変数がvalとvar

JS入門ではvarだったが、kotlinにはvalとvarの２つがある。
valは最初に値を設定すると以後変更出来ない変数。varは一度値を設定した後も、違う値を再セット出来る変数。

{% capture val_error %}
fun main() {
  val a = 3
  a = 4 // コンパイルエラー

  println(a)
}
{% endcapture %}
{% include kotlin_quote.html body=val_error %}

{% capture var_ok %}
fun main() {
  var a = 3
  a = 4 // varなのでOK！
  
  println(a)
}
{% endcapture %}
{% include kotlin_quote.html body=var_ok %}

意外と同じ変数に値をもう一度設定したいことは無いので、だいたいvalだけ使ってたまに必要なところだけvarを使う感じになる。
足し算して和を求める時とかはvarが必要になりがち。

{% capture range_sum %}
fun main() {
  val items = listOf(1, 2, 3, 4, 5)

  var sum = 0
  for(item in items) {
    sum += item
  }

  println(sum)
}
{% endcapture %}
{% include kotlin_quote.html body=range_sum %}

## 配列がList

kotlinにも配列があるけれど、あまり使わない。JSの配列に相当するものはListと思っておく方がいい。
ListについてはあとのCollectionsのところで詳しく扱うが、とりあえず配列の代わりはList。

ListはlistOfで作り、JSと同じ感じにアクセスする。

{% capture list_basic %}
fun main() {
  val items = listOf("ひとつ!", "ふたーつ!", "みっつ！")

  println(items[1])
}
{% endcapture %}
{% include kotlin_quote.html body=list_basic %}

これでitemsの型がListになります。（厳密には`List<String>`になる。これもあとでまじめに見ていく）

kotlinも0から始まる言語ですね。

要素の追加方法とかは違うが、その辺はあとで真面目に見ていきます。

## 辞書がMap

kotlinは、辞書をMapと呼びます。同じものです。
kotlinは辞書を定義するのはJSに比べると少しかったるくて、toのペアを並べてmapOfする。

{% capture map_basic %}
fun main() {
  val charaNum = mapOf("ストII" to 8, "ストII'" to 12, "スパII" to 16, "餓狼伝説" to 3)

  println(charaNum["スパII"])
  println(charaNum["餓狼伝説"])
  println(charaNum["ストII"])
}
{% endcapture %}
{% include kotlin_quote.html body=map_basic %}

JSに比べるとかったるいね。

これでcharaNumはMap型になる（厳密には`Map<String, Int>`になる、これも後でまじめに見ていく）

こちらも要素の追加方法などはあとで見ていく。
