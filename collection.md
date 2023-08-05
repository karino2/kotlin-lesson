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

## 続きはforループをやってから

forループをやってからでないと具体例を示しづらいので、続きはforループをやったあとに詳しく見ていきます。

[forループ入門](for_loop.md)