---
title: "コレクション概要"
layout: page
---
次のforループを説明するのに、ちょっとだけListを使える方がいいので、
JS入門と違うところだけを簡単に説明しておきます。

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
もういいかな、と思うくらいまでやって先に進みましょう。
足りないと思ったら上のリンク先の問題を適当にこちらでもやってみてください。

**課題: 以下のリストを生成せよ**

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

以下、全部同じ問題なので上のところに書いてください。

**課題: 以下のリストを生成せよ**

1. "一つ！"
2. "二つ！"
3. "三つ！"

**課題: 以下のリストを生成せよ**

1. "さぁ"
2. "ひょうしょうしきだ"
3. "なにぃっ"
4. "りゅうがいない？"

**課題: 以下のリストを生成せよ**

1. "トカゲだぎゃーさよなら"
2. "トカゲのともだちもわるくないな"

**課題: 以下のリストを生成せよ**

1. "ストII"
2. "ストIIダッシュ"
3. "ストIIターボ"
4. "スパII"
5. "スパIIX"

次は取り出す系もいくつかやってきましょう。

**課題: 「"コケー"」 を取り出せ**

kotaeに入れてね。

{% capture list_access0 %}
fun main() {
  val list = listOf("むぇ～～～","コケー","ダネ～～")
  val kotae = 0

  println(kotae)
}
{% endcapture %}
{% include kotlin_quote.html body=list_access0 %}

**課題: 「"三つ！"」 を取り出せ**

{% capture list_access1 %}
fun main() {
  val list = listOf("一つ！", "二つ！" "三つ！")
  val kotae = 0

  println(kotae)
}
{% endcapture %}
{% include kotlin_quote.html body=list_access1 %}

**課題: 「"なにぃっ"」 を取り出せ**

{% capture list_access2 %}
fun main() {
  val list = listOf( "さぁ", "ひょうしょうしきだ", "なにぃっ", "りゅうがいない？")
  val kotae = 0

  println(kotae)
}
{% endcapture %}
{% include kotlin_quote.html body=list_access2 %}

## Map入門

MapはmapOfとtoで作り`[]`でアクセスします。

ストIIはプレイヤーキャラが8人、ダッシュは12、餓狼伝説は3人でした。そういう感じのマップを作ると以下になります。

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

とりあえず以下にいくつか作っておくので、飽きるくらいやったら次へ。足りないと思ったら上記のJS入門の問題を適当にこちらで何問かやってみてください。

**課題: 以下のマップを生成せよ**

|キー	| 要素 |
| ----| ---- |
| るーしー | 15014 |
| ダニエル | 12518 |

{% capture map_create1 %}
fun main() {
  val kotae = 0

  println(kotae)
}
{% endcapture %}
{% include kotlin_quote.html body=map_create1 %}

**課題: 以下のマップを生成せよ**

|キー	| 要素 |
| ----| ---- |
| ストII | 1991 |
| ストIIダッシュ | 1992 |
| ストIIダッシュターボ | 1992 |
| スパII | 1993 |
| スパIIX | 1994 |

{% capture map_create2 %}
fun main() {
  val kotae = 0

  println(kotae)
}
{% endcapture %}
{% include kotlin_quote.html body=map_create2 %}

ダッシュとターボって同じ年だったっけ？結構間あった気がするんだが。

**課題: mapから「15014」を取り出せ**

{% capture map_access1 %}
fun main() {
  val map = mapOf("るーしー" to 15014,
                  "ダニエル" to 12518)
  val kotae = 0

  println(kotae)
}
{% endcapture %}
{% include kotlin_quote.html body=map_access1 %}

## 続きはforループをやってから

forループをやってからでないと具体例を示しづらいので、続きはforループをやったあとに詳しく見ていきます。

[forループ入門](for_loop.md)