---
title: "ツアー副読本：クラス入門"
layout: page
---

[クラス（ツアー） - Kotlin 日本語リファレンス](https://karino2.github.io/kotlin-web-site-ja/docs/kotlin-tour-classes.html)を読んでいく時の補助教材です。

## クラス、最初の一歩

<iframe width="560" height="315" src="https://www.youtube.com/embed/ExxYpVTITAQ?si=duc06-Jn-JCStaFo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

クラスとオブジェクトというのがある。

**オブジェクト**

- 変数と関数を持つ
- クラスから作られる

**クラス**

- オブジェクトの設計図である
- コードで書くのはこちら
- 変数と関数を書く（けれど実際はオブジェクトの変数と関数となる）

例：

```kotlin
class Hoge {

  var name = "はろー"

  fun fuga() {
    println("うがうが")
  }
}
```

変数や関数は複数持てる。

### コンストラクタ

クラスからオブジェクトを作るのは、**コンストラクタ**と呼ばれる特別な関数を使う。

コンストラクタは、クラスを書くと自動で作られる。

以下のように書くと、

```kotlin
class Hoge {
  // ...
}
```

Hogeという名前の関数が作られる。
これを呼び出すとオブジェクトが作られる。

```kotlin
val a = Hoge()
val b = Hoge()
```

### オブジェクトの関数は同じオブジェクトの変数を触れる

オブジェクトの関数は、同じオブジェクトに属している変数を触ることが出来ます。
コード上はクラスの変数を触っているように見えるのだけれど、
クラスでは無くオブジェクトの変数を触っている、というのが重要です。

{% capture method-example %}
class Hoge {

  var name = "はろー"

  fun fuga() {
    println("うがうが: ${name}")
  }
}

fun main() {
  // Hogeという関数を呼ぶ。これは自動で作られる関数で、コンストラクタと呼ばれる
  val a = Hoge()
  val b = Hoge()

  // こうやってオブジェクトの変数に代入出来る
  b.name = "むぇ〜〜〜"

  // こうやってオブジェクトの関数を呼ぶ
  a.fuga()
  b.fuga()
}
{% endcapture %}
{% include kotlin_quote.html body=method-example %}

オブジェクトの関数や変数の触り方とか呼び方とかは後で説明するけれど、とりあえずaとbという別のオブジェクトの関数を呼んでいて、
`a.fuga()`と`b.fuga()`の結果が違うことに注目してください。

### 用語いろいろ

クラスには大量の名前が出てきて初心者を威圧してきます。
全部一気に覚えるのは大変なので出てくる都度徐々に覚えていきましょう。

**オブジェクト**

幾つか名前があるが同じ意味です。

- オブジェクト
- インスタンス

**オブジェクトの関数**

幾つかの名前がある。

- メンバ関数
- メソッド

どちらも同じ意味です。

**オブジェクトの変数**

- プロパティ
- フィールド
- メンバ変数

プロパティは微妙に意味が違う（メンバ変数と後に出てくるcomputedプロパティというものの両方を含んだ言葉）のですが、
それはおいおい説明していくのでしばらくはオブジェクトの変数のことと思っておいてください。
しばらくはこの３つは同じ意味と思ってもらって良いです。
