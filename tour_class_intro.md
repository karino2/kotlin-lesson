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

## コンストラクタの引数とプロパティ

<iframe width="560" height="315" src="https://www.youtube.com/embed/SSeJAblq4qM?si=PMDrfOVQA1_62Opr" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

この動画では以下の話をしています。

- プロパティの初期化はコンストラクタが呼ばれる時である
- コンストラクタに引数を追加出来る
- コンストラクタの引数にvalを追加すると、結構いろいろな事が起こる

### プロパティの初期化のタイミング

以下のようなコードがあった時に、

```kotlin
class Hoge {
  val name = "ハロー"
}
```

このnameを初期化する行は、このコードが評価される時には実行されないで、コンストラクタが呼ばれる問に実行される。

```kotlin
class Hoge {
  val name = "ハロー"
}
// ここまではまだ実行されない

// ここで実行される
val a = Hoge()

// ここでも実行される（別のオブジェクトに対して）
val b = Hoge()
```

この事を関数を使って試してみよう。

{% capture prop-init-timing %}
fun test() : String {
  println("呼ばれた！")
  return "はろー"
}


class Hoge {
  val name = test()
}
// この時点ではまだnameの初期化は呼ばれない

fun main() {
  println("mainの先頭、まだ呼ばれてない")

  val a = Hoge()
  println("aが出来た")
  val b = Hoge()
  println("bが出来た")
}
{% endcapture %}
{% include kotlin_quote.html body=prop-init-timing %}


### コンストラクタへの引数の追加

コンストラクタに引数を追加する事が出来る。
これは関数に引数を追加するのに似ている。

```kotlin
// 関数
fun Ika(name: String) {
  println(name)
}

// コンストラクタ
class Hoge(name: String) {
  val title = name
}
```

コンストラクタに引数を追加するには、クラスの名前の後にカッコで引数の名前をカンマ区切りで書く。また、型をコロンの後につける。

この追加した引数は、プロパティの初期化に使える。

{% capture construct-args %}

class Hoge(a: String, b: Int) {
  val name = a
  val id = b
}

fun main() {

  val h1 = Hoge("はろー", 123)
  val h2 = Hoge("わーるど", 456)

  println("h1は、「name: ${h1.name}」 と 「id: ${h1.id}」")
  println("h2は、「name: ${h2.name}」 と 「id: ${h2.id}」")
}
{% endcapture %}
{% include kotlin_quote.html body=construct-args %}

### コンストラクタの引数にvalを追加

一見一つ前と同じように見えるが全然違うのがこのval。
コンストラクタの引数にvalというのをつけると、それがプロパティになる。
valをつけるといろいろ省略出来る便利機能です。

以下のようにnameの方にだけvalをつけると、

```kotlin
class Hoge(val name: String, b: Int) {
  val id = b
}
```

valの無い以下のコードと一緒だ。

```kotlin
class Hoge(tmp0: String, b: Int) {
  val name = tmp0
  val id = b
}
```

{% capture construct-val-1 %}

class Hoge1(val name: String, b: Int) {
  val id = b
}

class Hoge2(tmp0: String, b: Int) {
  val name = tmp0
  val id = b
}

fun main() {

  val h1 = Hoge1("はろー", 123)
  val h2 = Hoge2("わーるど", 456)

  println("h1は、「name: ${h1.name}」 と 「id: ${h1.id}」")
  println("h2は、「name: ${h2.name}」 と 「id: ${h2.id}」")
}
{% endcapture %}
{% include kotlin_quote.html body=construct-val-1 %}

ここはややこしいのでもう少し詳しく見てみる。

以下のようなコンストラクタにvalの無いコードの場合、aとbにはさわれません。

```kotlin
class Hoge(a: String, b: Int) {
  val name = a
  val id = b
}
```

実際に試してみましょう。

{% capture construct-val-2 %}

class Hoge(a: String, b: Int) {
  val name = a
  val id = b
}

fun main() {

  val h1 = Hoge("はろー", 123)

  println(h1.a) // エラー！
}
{% endcapture %}
{% include kotlin_quote.html body=construct-val-2 %}

でもvalをつけると、これがプロパティになって触る事が出来るようになります。

{% capture construct-val-3 %}

class Hoge(val a: String, b: Int) {
  val name = a
  val id = b
}

fun main() {

  val h1 = Hoge("はろー", 123)

  println(h1.a) // valをつけたからOK。
}
{% endcapture %}
{% include kotlin_quote.html body=construct-val-3 %}

触る事が出来るかどうかってだけだと思っても良いのだけれど、
最初のうちはちゃんと中括弧の中のプロパティに変換される、と思う方が良いです。

### dataクラスの解説を読み直してみよう

この辺まで理解した後に、
以前やった[data class入門](dataclass_intro.md)を読むと、この機能を使っている事がわかります。
dataクラスとclassは似ている、というかほとんど同じもので、もっと言うとclassに追加で幾つか自動でメンバ関数が生成されるのがdataクラスです（dataクラスはclassに機能を追加したもの）。

以下のような例がありました。

{% capture dataclass_intro1 %}
data class TwoInt(val v1: Int, val v2: Int)

fun main() {
  val ti1 = TwoInt(1, 2)

  println(ti1.v1)
  println(ti1.v2)
}
{% endcapture %}
{% include kotlin_quote.html body=dataclass_intro1 %}

このTwoIntは、省略したvalの記法を使っています。

```kotlin
data class TwoInt(val v1: Int, val v2: Int)
```

省略記法を使わないと以下のようになります。

```kotlin
data class TwoInt(tmp1: Int, tmp2: Int) {
  val v1 = tmp1
  val v2 = tmp2
}
```

data classなのかただのclassなのかは、実はこのくらいの例では違いは無いので、
dataはなくしてしまっても構いません。

```kotlin
class TwoInt(tmp1: Int, tmp2: Int) {
  val v1 = tmp1
  val v2 = tmp2
}
```

先程のコードで試してみましょう。

{% capture class-verbose %}
class TwoInt(tmp1: Int, tmp2: Int) {
  val v1 = tmp1
  val v2 = tmp2
}

fun main() {
  val ti1 = TwoInt(1, 2)

  println(ti1.v1)
  println(ti1.v2)
}
{% endcapture %}
{% include kotlin_quote.html body=class-verbose %}

v1だけ省略記法にしてみる。

{% capture class-semi-verbose %}
class TwoInt(val v1: Int, tmp2: Int) {
  val v2 = tmp2
}

fun main() {
  val ti1 = TwoInt(1, 2)

  println(ti1.v1)
  println(ti1.v2)
}
{% endcapture %}
{% include kotlin_quote.html body=class-semi-verbose %}

両方省略記法にしてみる。

{% capture class-almost %}
class TwoInt(val v1: Int, val v2: Int) {
}

fun main() {
  val ti1 = TwoInt(1, 2)

  println(ti1.v1)
  println(ti1.v2)
}
{% endcapture %}
{% include kotlin_quote.html body=class-almost %}

最後の空の中括弧を省略してみる

{% capture class-simplify %}
class TwoInt(val v1: Int, val v2: Int)

fun main() {
  val ti1 = TwoInt(1, 2)

  println(ti1.v1)
  println(ti1.v2)
}
{% endcapture %}
{% include kotlin_quote.html body=class-simplify %}

これで[data class入門](dataclass_intro.md)で書いたのとほぼ同じ記法になりました。