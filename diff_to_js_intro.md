---
title: "JS入門との差分"
layout: page
---
この辺からプログラム言語の勉強に入ります。

## JS入門を6.3までやる

プログラムについて完全にゼロから解説するのは結構たいへんなので、
以前JavaScriptという別の言語用に作った、以下の入門をやってもらいたい。

[算数で挫折した人向けのJavaScript入門](https://karino2.github.io/js-introduction/)

これを6.3まで進めて欲しい。
別の言語といっても6.3までの範囲だとkotlinともあまり違いは無いので。ほとんど無駄にはならないはず。

以下はこの入門を6.3までやった後に読んでください。

## JS入門とkotlinで違う所

JS入門は6.3までやりましたか？
やったと信じて続きを書きます。

6.3までの範囲ならほとんど同じとはいえ、ちょっとは違う所もあるので、
以下では、kotlinでは違う所を中心に補足説明します。

### 細かい話

- セミコロンはいらない
- MessageBox.show()はprintln()

{% capture hyousyousiki_code %}
fun main() {
  println("さぁ、表彰式だ")
  println("なにぃっ！")
}
{% endcapture %}
{% include kotlin_quote.html body=hyousyousiki_code %}


### mainとかいう奴

ここで実験するコードでは、必ず「`fun main() {`」と、その閉じ中カッコで囲む必要がある

```kotlin
fun main() {

  // いつもここに何かコードを書く

}
```

このfunとかが何かは後で関数のところまで進んだら説明するので、それまでは必ずテストコードはこれで囲まないといけない、と覚えておいてくれ。

{% capture hello_world_code %}
fun main() {
  println("はろーわーるど")
}
{% endcapture %}
{% include kotlin_quote.html body=hello_world_code %}

### 変数がvalとvar

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

### ifの後などの中括弧が一文なら要らない

これはJSでもそうなのだけれど、JSはセミコロンオートインサーションという特殊事情があっていろいろ説明が面倒なのでなるべく中括弧をつけるのを推奨していた。
けれどkotlinはそういう事は無いので、ifの後の文が1文だけなら中括弧無しでも構わない。

{% capture if_else_code %}
fun main() {
  val isYes = true

  println("ついにねんがんのアイスソードを手に入れたぞ")
  if(isYes)
    println("そうかんけいないね")
  else
    println("ころしてでもうばいとる")
}
{% endcapture %}
{% include kotlin_quote.html body=if_else_code %}

ちなみに、もちろん中括弧つけてもいい。

```kotlin
  if(isYes) {
    println("そうかんけいないね")
  } else {
    println("ころしてでもうばいとる")
  }
```

つまり、以下の三つは全部同じ意味になる。

```kotlin
  if(isYes) {
    println("そうかんけいないね")
  } else {
    println("ころしてでもうばいとる")
  }
```

```kotlin
  if(isYes)
    println("そうかんけいないね")
  else
    println("ころしてでもうばいとる")
```

```kotlin
  if(isYes) println("そうかんけいないね") else println("ころしてでもうばいとる")
```

3番目は変数の右辺の時にたまに使う。（[ListViewに挑む！表示編](listview_disp.md)で出てくる）

なお、当然だが実行する文が2行以上の時は中括弧つけないといけないので、以下のケースでは中括弧無しに書く方法は無い（どこが終わりかわからないので）

```kotlin
  if(isYes) {
    println("そう")
    println("かんけいないね")
  } else {
    println("ころしてで")
    println("もうばいとる")
  }
```

### 配列がList

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

### 辞書がMap

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

Mapについては使う必要が出てきたら真面目に見ていくが、
JSほどは使う機会は多く無い。