---
title: "簡単な電卓を作ってみよう"
layout: page
---
Androidアプリの入門として、足し算とクリアしか無い電卓を作ってみよう。
簡単な方針を書いておきます。

## レイアウト

一番外側のLinearLayoutに、以下のような子供を持たせる。

1. 現在入力中の数字を表示するTextView
2. 「9, 8, 7, C」 のボタンを持ったLinearLayout
3. 「6, 5, 4, +」 のボタンを持ったLinearLayout
4. 「3, 2, 1, =」 のボタンを持ったLinearLayout
5. 「0」のボタン

という感じでどうでしょう？

Cはクリアです。

数字が押されるとTextViewの文字の右隣りに今押された数字が入るのだが、今のテキストが0の時だけ特別扱いする。

## 文字に押された数字を足していく

文字列に数字を足していくのは、イメージとしては以下のような感じで出来ます。

{% capture str_add %}
fun main() {
  var s = "0"

  s += "1"
  s += "2"
  s += "3"

  println(s)
}
{% endcapture %}
{% include kotlin_quote.html body=str_add %}


全てのボタンにsetOnClickListenerを適当にぶら下げる。例えば9のボタンなら以下みたいな感じ。

```kotlin
findViewById<Button>(R.id.buttonNine).setOnClickListener { 
  findViewById<TextView>(R.id.result).text += "9"
}
```

これだと数字の一番左に余分な"0"が入っちゃうので、if文で"0"の時はsは無視して直接入れる、みたいな処理を書く必要がある。
10回も同じ処理書かなくてはいけなくてダルいが頑張る。

## 計算結果を覚えておく方法

「+」の処理は難しいので、いくつか必要なヒントを書いておきます。

まず、「+」を押した時点での数字を覚えておいて、そこから先に数字を入れられたらリセットする必要がある。（この辺は実際にその辺の電卓とか電卓アプリ触って見ると分かります）

リセットは手抜きでこの時点で0を入れてしまってもいいけれど、値を覚えておく必要はあります。

最初が以下のようになっているのが、

```kotlin
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
```

このonCreateの「外に」以下のように変数を用意しておくと値を覚えておけます（この説明はそのうちしますので今はそういうものと思って使ってください）。

```kotlin
    var genzainoAtai = 0

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
```

このgenzainoAtaiという変数に数字を保存しておく事が出来る。

## 文字列と数字の変換

`findViewById<TextView>(R.id.result).text` で取れるのは文字列なのだけど、保存したり足し算したりする時は数字にしたい。

文字列から数字に変換するのはtoIntというのを使います。

{% capture str_to_int %}
fun main() {
  val s = "123"
  val num = s.toInt()

  // 文字列と数字を足すと連結になっちゃう。(123+4で127にならない)
  println(s+4)

  // toInt()で数字にしたら足し算になる。
  println(num+4)
}
{% endcapture %}
{% include kotlin_quote.html body=str_to_int %}

つまり保存したくなったら、以下のようにすれば良い。

```kotlin
genzainoAtai = findViewById<TextView>(R.id.result).text.toInt()
```