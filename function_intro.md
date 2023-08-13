---
title: "関数入門"
layout: page
---

kotlinの関数について、とりあえずActivityの中でコードをメンバ関数に抜き出していくのに必要な程度の入門をする。

## 関数という概念自体はJS入門でやってください

関数というのを他の言語でやった事が無い人、具体的には仮引数やreturnというのが何なのかわからない人は、
以下のJS入門の 第七回〜第九回 をやってください。 

- [第七回: 関数その1 関数を作る事、呼び出す事 - 算数で挫折した人向けの、Javascript入門](https://karino2.github.io/js-introduction/ch07.html)
- [第八回: 関数その2 関数からもらうもの - 算数で挫折した人向けの、Javascript入門](https://karino2.github.io/js-introduction/ch08.html)
- [第九回: 関数その3 関数に渡すもの - 算数で挫折した人向けの、Javascript入門](https://karino2.github.io/js-introduction/ch09.html)

## kotlinの関数の定義の仕方

関数は以下のように定義します。

{% capture func_intro1 %}

fun sum(a: Int, b:Int) : Int {
  return a+b
}

fun main() {
  println(sum(1, 2)+3)
}
{% endcapture %}
{% include kotlin_quote.html body=func_intro1 %}

さて、関数を定義しているのは以下の部分になります。

```kotlin
fun sum(a: Int, b:Int) : Int {
  return a+b
}
```

### kotlinの関数は、仮引数とreturnの型を指定する必要がある

JS入門との一番の違いとしては、仮引数とreturnの型を、最初に指定する必要があります。

仮引数の方は変数名（上の例だとaとb）の後にコロンを置いて、その後に型を置きます。

中括弧の前がreturnの型です。
return文を見なくても、funの行だけで引数とreturnの型が分かるようになっているのが特徴です。

例えばreturnの所でtoStringして戻りの型をStringに変えてみましょう。
そうするとこうなります。

{% capture func_intro2 %}

fun sum2(a: Int, b:Int) : String {
  return (a+b).toString()
}

fun main() {
  // 6じゃなくて"33"になる事に注目
  println(sum2(1, 2)+3)
}
{% endcapture %}
{% include kotlin_quote.html body=func_intro2 %}
