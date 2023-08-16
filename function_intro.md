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

funというキーワードで始まり、関数名が続きます。
この場合はsum、という名前の関数が定義出来ます。

以下、上のJS入門との違いを中心に見ていきましょう。

### 引数、仮引数、returnについて（用語の解説）

JS入門の方は算数で挫折した人向けだったので専門用語を極力使わない解説になっていますが、こちらは算数で挫折してない人向けなので普通に用語を幾つか使います。

### 引数

以下のコードで、

```kotlin
sum(2, 3)
```

この2とか3とかを「引数」と言います「ひきすう」と呼びます。[第九回: 関数その3 関数に渡すもの - 算数で挫折した人向けの、Javascript入門](https://karino2.github.io/js-introduction/ch09.html)で「渡すもの」と呼んでいたものです。
これを「引数」と呼びます。

### 仮引数

JS入門で渡された文字を受け取る、という所で出てきたargumentsというのが、kotlinでは変数になっています（後述）。これを仮引数といいます。「かりひきすう」と読みます。
これは後の具体例を見れば分かると思う。

### 関数のreturn

[第八回: 関数その2 関数からもらうもの - 算数で挫折した人向けの、Javascript入門](https://karino2.github.io/js-introduction/ch08.html)で、関数から「もらうもの」と言っていたものを…
ここの呼び名はたぶんバラツキがあって定番が無い気がする。
返値とか戻り値とか言うけれど、あんまり統一されていない。

私は良く「関数のreturn」と言っています。このドキュメントでも「関数のreturn」と呼ぶことにします。

この辺は一見難しそうだけれど、実は単なる言葉なので使っていくとすでに知っている事だと気づくはずです。
という事で以下を見ていけばたぶん分かるから以下を見ていこう。

### kotlinでは、argumentsの代わりに変数（仮引数）を使い、しかも型を指定する必要がある

[第九回: 関数その3 関数に渡すもの - 算数で挫折した人向けの、Javascript入門](https://karino2.github.io/js-introduction/ch09.html)では、
関数にものを渡すとargumentsという変数で受け取れる、という話をしました。

ですがkotlinでは渡されるものの個数が決まっていて、それぞれに変数名と型を指定しないといけません。

以下の例で言うところのa, bがその変数名に相当します。

```kotlin
fun sum(a: Int, b:Int) : Int {
  // 省略
}
```

このa, bなどを、仮引数といいます。
仮引数は、変数名（上の例だとaとb）の後にコロンを置いて、その後に型を置きます。

仮引数を2つで定義すると、呼び出す時にも引数を二つ渡してしか呼べません。
また、引数の型があっていないといけません。

```kotlin
sum(1, 2) // OK
sum(1) // エラー
sum(1, 2, 3) // エラー
sum("1", "2") // エラー、型が違う
```

ちなみに、`sum(1, 2)`と呼ぶ時の1と2を引数といいます。

### kotlinの関数は、returnの型を指定する必要がある

kotlinの関数では、returnの型も最初に指定する必要があります。
以下の関数定義のうち、

```kotlin
fun sum(a: Int, b:Int) : Int {
  // 省略
}
```

一番最後のInt（中括弧の直前のInt）がreturnの型です。
return文を見なくても、funの行だけで引数とreturnの型が分かるようになっているのが特徴です。

全部Intだとわかりにくいので、returnの型を変えてみましょう。
例えばsumのreturnの所でtoStringして戻りの型をStringに変えてみましょう。
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

sum2の閉じカッコの後のコロンの後がIntからStringに変わっているのに注意してください。

こういうのは自分で書いてみるのが一番なので、いくつか簡単な関数を書いてみよう。

**課題: aとbを掛けるmulという関数を書け**

引数はInt、戻りもIntで。

{% capture func_intro_q1 %}

// TODO: ここにmul関数を書け


fun main() {
  // TOOD: 以下でtrueが出力されるのを確認せよ
  println(mul(2, 3) == 6)
}
{% endcapture %}
{% include kotlin_quote.html body=func_intro_q1 %}

**課題: aとbを掛けたあとにcを足す、mulAddという関数を書け**

{% capture fun_intro_q2 %}

// TODO: ここにmulAdd関数を書け


fun main() {
  // 以下がtrueが出力されるのを確認せよ
  println(mulAdd(2, 3, 4) == 10)
}
{% endcapture %}
{% include kotlin_quote.html body=fun_intro_q2 %}

**課題: 「ダネ〜」とprintlnした後に3を返す、fusigidaneという関数を書け**

{% capture fun_intro_q3 %}

// TODO: ここにfusigidane関数を書け


fun main() {
  // 以下で「ダネ〜」と出力された後にtrueが出力されるのを確認せよ
  println(fusigidane() == 3)
}
{% endcapture %}
{% include kotlin_quote.html body=fun_intro_q3 %}

**課題: 1+2+...+99+100した結果を返す、sumTillHundred関数を作れ**

till hundredは百まで、って意味です。

{% capture fun_intro_q4 %}

// TODO: ここにsumTllHundred関数を書け


fun main() {
  // 以下がtrueが出力されるのを確認せよ
  println(sumTillHundred() == 5050)
}
{% endcapture %}
{% include kotlin_quote.html body=fun_intro_q4 %}

**課題: 1+3+5+...+97+99した結果を返す、sumOdd関数を作れ**

奇数は英語でoddと言います。

{% capture fun_intro_q5 %}

// TODO: ここにsumOdd関数を書け


fun main() {
  // 以下がtrueが出力されるのを確認せよ
  println(sumOdd() == 2500)
}
{% endcapture %}
{% include kotlin_quote.html body=fun_intro_q5 %}

**課題: 1, 3, 5, ..., 97, 99の要素を持つListを返す関数、oddList関数を作れ**

- ヒント1: 関数の定義で戻りをListとしておくと、MutableListをreturnしてもListになります。
- ヒント2: Listなどの型は要素の型を`<>`で指定する必要があります

{% capture fun_intro_q6 %}

// TODO: ここにoddList関数を書け

fun main() {
  // 以下が全てtrueが出力されるのを確認せよ
  val olist = oddList()
  println(olist.size == 50)
  println(olist[0] == 1)
  println(olist[15] == 31)
  println(olist.sum() == 2500)
}
{% endcapture %}
{% include kotlin_quote.html body=fun_intro_q6 %}

**課題: Stringのリストの奇数番目だけを含んだ新しいリストを返す、oddOnly関数を書け**

渡されたStringのリストから、1番目、3番目、5番目、7番目…と奇数番目だけの要素を持った新しいリストを作って返す、oddOnlyを作れ。
0番目から数える事にする（以下のprintlnが全部trueになるように考えてくれ）

{% capture fun_intro_q7 %}

// TODO: ここにoddOnly関数を書け

fun main() {

  val list = listOf("0番目",  "1番目", "2番目", "3番目", "4番目", "5番目")

  val olist = oddOnly(list)
  // 以下が全てtrueなのを確認せよ。 
  println(olist.size == 3)
  println(olist[1] == "3番目")
}
{% endcapture %}
{% include kotlin_quote.html body=fun_intro_q7 %}


### returnしない場合は戻りの型は無しで良い

printlnだけする関数など、returnするものが無い関数では、戻りの型は書かなくても良いです。

{% capture func_noreturn %}

fun printHello() {
  println("Hello World")
}

fun main() {
  printHello()
}
{% endcapture %}
{% include kotlin_quote.html body=func_noreturn %}

コロンも無くなっている事に注目してください。

なお、戻りの型を書かないと、何も無いを意味するUnit型というものになります。

つまり、printHelloは以下のように書いても全く同じ意味になります。

```kotlin
fun printHello() : Unit{
  println("Hello World")
}
```

**課題: 「ほげ」と「いか」の2行をprintlnする、printHogeIkaという関数を書け**

{% capture fun_noreturn_q1 %}

// TODO: ここにprintHogeIka関数を書け


fun main() {
  // TOOD: 以下のコメントを外して、「ほげ」と「いか」が出力されるのを確認せよ
  // printHogeIka()
}
{% endcapture %}
{% include kotlin_quote.html body=fun_noreturn_q1 %}


### 関数の名前は小文字始まりで単語境界は大文字にする決まり

最後になったけれど、関数の名前は小文字始まりで区切りは大文字にする。
つまりprintHelloとかsetupDataとか。

一応そういう風にしましょうね、という事になっているけれど、
別段そうしないと動かないという訳では無い。
ご飯を食べる前にいただきますと言いましょう、くらいのルールです。

## これまでのコードを関数を使ってみよう

関数を学ぶには使ってみるのが一番。けれどなんか人工的な例を出すのは盛り上がりに欠けるので、これまで書いたコードを関数を使って直してみよう。

なお、以下では関数にしてくくりだせ、と言ったら、基本的には「メンバ関数」というものにします。
置き場所としては[コードの置き場所入門](code_location_intro.md)で言う所の「2番目の区画」になります。

### ListViewの表示編のfor文を関数にする

[ListView表示編の二回目以降](listview_disp_second.md)では、以下のようなコードがあったと思います。

```kotlin
    val listData = mutableListOf()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        for(i in 1..100) {
          listData.add("${i}番目の項目です")
        }

        findViewById<ListView>(R.id.listView1).adapter = adapter
    }
```

このうち、以下の部分をsetupDataという関数にして、それを呼び出すように変えてください。

```kotlin
  for(i in 1..100) {
    listData.add("${i}番目の項目です")
  }
```

### 簡易電卓のonClickListenerの処理を関数にする

[簡単な電卓を作ってみよう](simple_calc.md)で、9個のボタンの処理がほとんど同じになっていたと思う。
これを関数にして共通化してみよう。

1. とりあえず「1」のボタンが押された時に、入ってる値が0だった時の処理をif文で入れる
2. その処理を関数にして、それを呼ぶようにする
3. 「1」に特有の部分を仮引数にして外から1を渡すようにする（"1"を渡してもいいです、お好きな方で）
3. [List入門](list_intro.md)の「電卓の課題をfor文化出来ないか考えてみる」あたりを参考に全部のボタンで同じ処理をする