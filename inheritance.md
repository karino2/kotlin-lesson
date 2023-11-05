---
title: "クラスの継承"
layout: page
---
ツアーを終えたあとに最低限読んで欲しい所を読んでいくと、継承の所で詰まるはずです。

ここでは[リファレンスの継承](https://karino2.github.io/kotlin-web-site-ja/docs/inheritance.html)を読んでくのに必要な解説を提供します。

## 継承とはどういうものか

まずは継承というのがどういうものか、という解説動画です。

<iframe width="560" height="315" src="https://www.youtube.com/embed/JWYWnsQ9Rko?si=0-3JVVZmmc640nen" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

動画では　`class B: A{}`と書いていましたが、Aのあとにはカッコが必要でした。
以下が正解です。

```kotlin
class B : A() {
  //...
}
```

### overrideの実例

{% capture override-ex1 %}

open class A {
  open fun hoge() {
    println("Aのhoge")
  }

  fun ika() {
    println("Aのikaだ。これからhogeを呼ぶぜ")
    hoge()
    println("Aのikaが終わるぜ")
  }
}

class B : A() {
  override fun hoge() {
    println("Bが横取りしたhogeだぜ")
  }
}

fun main() {
  println("aのika: ")
  val a : A = B()
  a.ika()

  println("---")
  println("")
  println("bのika: ")
  val b = B()
  b.ika()

  println("---")
  println("")
  println("cのika: ")
  val c = A()
  c.ika()
}
{% endcapture %}
{% include kotlin_quote.html body=override-ex1 %}
