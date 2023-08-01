---
title: "「算数で挫折した人向けの、Javascript入門」との差分"
layout: page
---
[算数で挫折した人向けのJavaScript入門 - 算数で挫折した人向けの、Javascript入門](https://karino2.github.io/js-introduction/)を6.3までやってもらったと思うので、
この範囲でkotlinは違う、というところを簡単にまとめておく。

## 細かい話

- セミコロンはいらない
- MessageBox.show()はprintln()

## mainとかいう奴

ここで実験するコードでは、必ず「`fun main() {`」と、その閉じ中カッコで囲む必要がある

```kotlin
fun main() {

}
```

これが何かは後で関数のところまで進んだら説明するので、それまでは必ずテストコードはこれで囲まないといけない、と覚えておいてくれ。

{% capture hello_world %}
fun main() {
  println("はろーわーるど")
}
{% endcapture %}
{% include kotlin_quote.html body=hello_world %}


## 変数がvalとvar

JS入門ではvarだったが、kotlinにはvalとvarの２つがある。



