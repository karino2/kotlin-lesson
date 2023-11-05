---
title: "関数入門"
layout: page
---

kotlinの関数について、とりあえずActivityの中でコードをメンバ関数に抜き出していくのに必要な程度の入門をする。

## 関数とは

同じようなコードを複数の場所で使いたい、という事があります。
そういう時にコードをまとめて関数という物にして、それを複数の場所で使う、という事が出来ます。

例えば以下みたなコードがあったとしましょう。

{% capture func_ex1 %}

fun main() {
  println("ほげ")
  println("いか")
  println("ふが")

  println("ほげ")
  println("いか")
  println("ふが")
}
{% endcapture %}
{% include kotlin_quote.html body=func_ex1 %}

大した意味は無いですが、このコードには同じ三行のprintlnが2回繰り返されています。
こういう時に、コードをまとめて関数というものにして、それを使う、という事が出来ます。

{% capture func_ex2 %}
fun printHogeIkaFuga() {
  println("ほげ")
  println("いか")
  println("ふが")
}

fun main() {
  printHogeIkaFuga()

  printHogeIkaFuga()  
}
{% endcapture %}
{% include kotlin_quote.html body=func_ex2 %}

このprintHogeIkaFugaというのが「関数」と呼ばれるものです。
以下関数について、簡単に見ていきます。

関数には大きく２つの側面があります。

- 関数を作る
- 関数を呼ぶ（使う）

上のコードでは、以下の部分が「関数を作る」部分で、

```kotlin
fun printHogeIkaFuga() {
  println("ほげ")
  println("いか")
  println("ふが")
}
```

以下のコードが「関数を呼ぶ」部分です。

```kotlin
  printHogeIkaFuga()
```

この２つについて、以下ではより詳しく見ていく事にします。

## 関数を作る

関数を作る事について、以下では詳細に見ていきます。
なお、関数を作る事を専門用語では「関数を定義する」といいます。

関数の定義は、以下の三段階で考えると良いでしょう。

- 一番カンタンな関数
- 関数のパラメータ
- 関数から値を返す

以下、順番に見ていきます。

### 一番カンタンな関数の作り方

一番カンタンな関数は、先程のprintHogeIkaFugaのような物になります。

```kotlin
fun printHogeIkaFuga() {
  println("ほげ")
  println("いか")
  println("ふが")
}
```

まず最初の行に注目しましょう。以下の部分です： `fun printHogeIkaFuga() {`

一番簡単な関数は以下のように書きます。

1. 最初は`fun`で始めます
2. 次に「関数の名前」を置きます
3. その後にかっこ、つまり`()`を置きます
4. その後ろに中括弧、`{}`を置きます
5. 中括弧の中に関数で実行したいコードを書きます

2の「関数の名前」は、あなたが勝手に決める名前です。変数名とかと同じようなものですね。
この場合はprintHogeIkaFugaという名前にしましたが、好きな名前をつける事が出来ます。

とりあえず自分で何回か書いてみてください。

**課題: 「むぇ〜〜」と「ぼえぇ〜〜」を出力する関数、printMueBoeを書け**

とりあえず、以下のコードを実行する関数、`printMueBoe`を作ってください。

```kotlin
println("むえぇ〜〜")
println("ぼえぇ〜〜")
```

この問題は、慣れるまで3〜4回くらい繰り返し解いてください。

{% capture func-decl-q1 %}
// TODO: 以下に関数を書く

fun main() {
  printMueBoe()
  // むえぇ〜~
  // ぼえぇ〜〜
  // と出力されたら正解
}
{% endcapture %}
{% include kotlin_quote.html body=func-decl-q1 %}

{% capture func_decl-q1-a %}
```kotlin
// TODO: 以下に関数を書く
fun printMueBoe() {
  println("むえぇ〜〜")
  println("ぼえぇ〜〜")
}

fun main() {
  printMueBoe()
  // むえぇ〜~
  // ぼえぇ〜〜
  // と出力されたら正解
}
```
{% endcapture %}
{% include collapse_quote.html body=func_decl-q1-a title="解答例" %}

### 関数のパラメータ

コードをまとめて再利用する時に、
完全に同じコードを実行するだけだとあまり便利ではありません。
コードをちょっとだけ変えたい、という事がある。

例えば以下のコードを考えましょう。

{% capture func_param1 %}

fun main() {
  val suto2 = listOf("ストII", "ストIIダッシュ", "ストIIターボ", "スーパーストII", "スパIIX")
  val garou = listOf("餓狼伝説", "餓狼伝説2", "餓狼伝説スペシャル")

  for(name in suto2) {
    println(name)
  }

  println("-----")

  for(name in garou) {
    println(name)
  }
}
{% endcapture %}
{% include kotlin_quote.html body=func_param1 %}

２つのfor文は、ほとんど同じに見えます。

```kotlin
  for(name in suto2) {
    println(name)
  }
```

```kotlin
  for(name in garou) {
    println(name)
  }
```

違うのはsuto2とgarouの所だけです。

このように、コードのほとんどが同じだけど一箇所だけ違う、みたいなのは良くあります。
このように、ちょっとだけ違う時に、
違う所を変数にしておく機能を「パラメータ」と言います。

実際にこのパラメータというのを使って関数を作るとどうなるかを見てみましょう。
printListという関数を作る事にします。

{% capture func_param2 %}

fun printList(list : List<String>) {
  for(name in list) {
    println(name)
  }
}

fun main() {
  val suto2 = listOf("ストII", "ストIIダッシュ", "ストIIターボ", "スーパーストII", "スパIIX")
  val garou = listOf("餓狼伝説", "餓狼伝説2", "餓狼伝説スペシャル")

  printList(suto2)

  println("-----")

  printList(garou)
}
{% endcapture %}
{% include kotlin_quote.html body=func_param2 %}

まず関数を作る所に注目してください。
以下のようになっています。

```kotlin
fun printList(list : List<String>) {
  for(name in list) {
    println(name)
  }
}
```

このlistというのがパラメータです。これは変数の一種です。
パラメータは関数を作る時の`()`の間に入れます。
以下の部分の事を言っています。`fun printList(list : List<String>)`

パラメータの名前の後に、型を`:`で指定します。この辺はvarの型の指定の仕方と同じです。

listという変数に対して、この関数は、以下を実行しています。

```kotlin
  for(name in list) {
    println(name)
  }
```

このlistという変数が何なのかは、この時点では良くわかりません。型が`List<String>`だという事しかわからない。

呼び出す側でこのlistの値を入れます。

```kotlin
  printList(suto2)
```

このように書くと、listという変数にsuto2を入れて関数を実行します。
以下のようにすれば、

```kotlin
  printList(garou)
```

listという変数にgarouを入れて関数を実行します。

パラメータは、関数を呼び出す時に値が決まる変数だ、という事です。

### ２つ以上のパラメータ

パラメータが２つ以上ある場合は、カンマで区切ります。
例えば以下のように書く。

```kotlin
fun printSum(a: Int, b:Int) {
  println(a+b)
}
```

これは、`a: Int`というパラメータと、`b: Int`というパラメータの２つがあるケースです。
このように、カンマで区切って並べます。

### パラメータは引数とも呼ぶ

これは用語の問題ですが、パラメータは「引数」とも呼びます。
このページではなるべくパラメータで統一したいと思いますが、
たまに意識せずに使ってしまうかもしれないのと、
他のドキュメントでは「引数」と呼んでいるものもあるので説明しておきます。
「ひきすう」と読みます。

では少し自分でも書いてみましょう。

**課題:２つの数字を足してprintlnする、printSumという関数を作れ**

これは既に上の例で出てきたものですが、自分でも書いてみましょう。
もしこの問題がすんなり解けなければ、先に行く前にこの問題を3回くらい繰り返し解いてください。

{% capture func-param-q1 %}
// TODO: 以下に関数を書く

// 以下はいじらない
fun main() {
  printSum(7, 2)
  // 「9」と出力されたら正解
}
{% endcapture %}
{% include kotlin_quote.html body=func-param-q1 %}

{% capture func_param-q1-a %}
```kotlin
// TODO: 以下に関数を書く
fun printSum(a: Int, b: Int) {
  println(a+b)
}

// 以下はいじらない
fun main() {
  printSum(7, 2)
  // 「9」と出力されたら正解
}
```
{% endcapture %}
{% include collapse_quote.html body=func_param-q1-a title="解答例" %}

**課題:２つの文字列を連結してprintlnする、renketuという関数を作れ**

今度はパラメータが文字列の例です。

{% capture func-param-q2 %}
// TODO: 以下に関数を書く

// 以下はいじらない
fun main() {
  renketu("ほげ", "いか")
  // 「ほげいか」と出力されたら正解
}
{% endcapture %}
{% include kotlin_quote.html body=func-param-q2 %}

{% capture func_param-q2-a %}
```kotlin
// TODO: 以下に関数を書く
fun renketu(s1: String, s2: String) {
  println(s1+s2)
}

// 以下はいじらない
fun main() {
  renketu("ほげ", "いか")
  // 「ほげいか」と出力されたら正解
}
```
{% endcapture %}
{% include collapse_quote.html body=func_param-q2-a title="解答例" %}

### 関数から値を返す

関数は、「結果を返す」事も出来ます。
「結果を返す」というのがどういう事かを知るべく、まず例を見ましょう。

{% capture func-return1 %}
fun sum(a: Int, b: Int) : Int {
  return a+b
}

fun main() {
  val c = sum(7, 2)
  println(c)
}
{% endcapture %}
{% include kotlin_quote.html body=func-return1 %}

関数を作る所に関しては、新しい事は以下の２つです。

1. 閉じカッコの後ろに `: Int` がついている
2. 関数の中に`return`というのがある

順番に説明していきましょう。

１つ目の、「カッコの後ろの型」は、この関数が「返す値の型」になります。

２つ目の、`return`は、「値を返す命令」です。
この`return`の「次に来る式」を、この関数は「返す」事になります。

返した結果は、呼び出す側で受け取る事が出来ます。
それはイコールの右で使ったり出来ます。

```kotlin
  val c = sum(7, 2)
```

こうすると、cにはsumから返された値が入ります。

なお、returnが実行されるとこの関数の実行は終了します。その次の行には処理が行きません。

例えば以下のような関数があったとしましょう。

{% capture func-return2 %}
fun printHoge(s: String) : String {
  println(s)
  if (s == "ほげ")
    return "いか"

  // sが"ほげ"の場合はこの行は実行されない  
  println("ほげじゃない！")
  return s
}

fun main() {
  printHoge("ほげ")
}
{% endcapture %}
{% include kotlin_quote.html body=func-return2 %}

この場合、「"ほげじゃない！"」とは出力されません。
なお、関数の返す値の型が「String」となっているので、この関数はいつでも文字列をreturnしないといけません（だから最後にreturn sをしている）。

いくつか練習問題をやってみましょう。

{% capture comment1 %}
**なんか値を返すとか唐突な感じがするんだけど？**  
コードを再利用する時に、一部を変数にしてそれを使う時に設定出来る、というのは、理解すればそんなに不自然では無いと思いますが、
「値を返す」というのはなんか唐突に出てきた感じがします。
いまいち必要性も良くわからない。

実際、アセンブリ言語やBASICといった言語では、関数はありません。
昔の言語には関数はなかった。

けれど、これらの言語ではコードの一部を再利用するのがうまく行かなくて、
コードを再利用するにはどういった機能が必要か？というのをいろいろ試行錯誤しました。
その結果、とりあえず以下があれば再利用出来そうだ、と分かりました。

1. 呼び出し元に実行が「戻る」機能
2. ローカル変数
3. パラメータ
4. 呼び出し元に値を返す機能

これらを合わせたものを、現代のプログラム言語では「関数」と呼ぶことにしています。
これらの機能が当然必要である、というよりは、
いろいろ試行錯誤したらこのくらいあればどうにかなった、というものなので、
あまり必然性は無いかもしれません。

実際昔の言語ではこれらが全て揃っていない、サブルーチンとかサブプロシージャといった機能もありました。
ただこれらの機能が揃っていないと、グローバル変数というものがたくさん出てきて再利用がうまく行かない、ということが分かっています。
{% endcapture %}
{% include myquote.html body=comment1 %}



**課題:２つの数字を足して返す、sumという関数を作れ**

これは既に上の例で出てきたものですが、自分でも書いてみましょう。
もしこの問題がすんなり解けなければ、先に行く前にこの問題を3回くらい繰り返し解いてください。

{% capture func-return-q1 %}
// TODO: 以下にsum関数を書く

// 以下はいじらない
fun main() {
  val c = sum(7, 2)
  println(c == 9) // trueと出たら正解
}
{% endcapture %}
{% include kotlin_quote.html body=func-return-q1 %}

{% capture func_return-q1-a %}
```kotlin
// TODO: 以下に関数を書く
fun sum(a: Int, b: Int) : Int {
  return a+b
}

// 以下はいじらない
fun main() {
  val c = sum(7, 2)
  println(c == 9) // trueと出たら正解
}
```
{% endcapture %}
{% include collapse_quote.html body=func_return-q1-a title="解答例" %}

**課題:２つの文字列を連結して返す、renketuという関数を作れ**

{% capture func-return-q2 %}
// TODO: 以下にrenketu関数を書く

// 以下はいじらない
fun main() {
  val s = renketu("ほげ", "いか")
  println(s == "ほげいか") // trueと出力されたら正解
}
{% endcapture %}
{% include kotlin_quote.html body=func-return-q2 %}

{% capture func-return-q2-a %}
```kotlin
// TODO: 以下に関数を書く
fun renketu(s1: String, s2: String) : String{
  return s1+s2
}

// 以下はいじらない
fun main() {
  val s = renketu("ほげ", "いか")
  println(s == "ほげいか") // trueと出力されたら正解
}
```
{% endcapture %}
{% include collapse_quote.html body=func-return-q2-a title="解答例" %}

**課題:文字列のリストを連結して返す、renketuListという関数を作れ**

{% capture func-return-q3 %}
// TODO: 以下にrenketuList関数を書く

// 以下はいじらない
fun main() {
  val list = listOf("ほげ", "いか", "ふが")
  val s = renketuList(list)
  println(s == "ほげいかふが") // trueと出力されたら正解
}
{% endcapture %}
{% include kotlin_quote.html body=func-return-q3 %}

{% capture func-return-q3-a %}
```kotlin
// TODO: 以下に関数を書く
fun renketuList(l: List<String>) : String{
  var s = ""
  for(s1 in l) {
    s += s1
  }
  return s
}

// 以下はいじらない
fun main() {
  val list = listOf("ほげ", "いか", "ふが")
  val s = renketuList(list)
  println(s == "ほげいかふが") // trueと出力されたら正解
}
```
{% endcapture %}
{% include collapse_quote.html body=func-return-q3-a title="解答例" %}

### 関数の作り方まとめ

以上を簡単にまとめておきます。

- 関数は`fun hoge(){}`みたいな感じで作る
- パラメータというのがあって、`()`の中に入れる
- 関数は値を返す事が出来て、 `fun hoge() : Int { return XXX }`みたいな感じで書く

## 関数の呼び方

関数は大きく、以下の２つにわけられるという話をしました。

- 関数を作る
- 関数を呼ぶ（使う）

けれど作る事を理解してしまえば呼ぶのは簡単です。
基本的に作ったものに対応するので、呼ぶ側でも「パラメータ」と「値を返す」ケースを見ていけば良い、
となります。

まずいちばん基本的な所から始めるために、以下の例を再掲します。

{% capture func-call1 %}
fun printHogeIkaFuga() {
  println("ほげ")
  println("いか")
  println("ふが")
}

fun main() {
  printHogeIkaFuga()

  printHogeIkaFuga()  
}
{% endcapture %}
{% include kotlin_quote.html body=func-call1 %}

ここで、関数を「呼んでいる」のは以下です。

```kotlin
  printHogeIkaFuga()

  printHogeIkaFuga()  
```

関数の呼び方は、関数名の後にカッコをつける事で呼ぶことが出来ます。
この場合はprintHogeIkaFugaが関数名で、その後に`()`をつける、
つまり`printHogeIkaFuga()`とすると「関数を呼ぶ」ことが出来ます。

### パラメータのある関数を呼び出す

パラメータのある関数は、カッコの中にパラメータに「渡したい」値をカンマ区切りで入れることで呼び出すことが出来ます。

例えば以下のような関数があったとします。

```kotlin
fun printSum(a: Int, b:Int) {
  println(a+b)
}
```

すると、この関数は、以下のように呼び出すことができます。

```kotlin
printSum(1, 2)
```

このように、パラメータに「渡したい」値をカッコの中にカンマ区切りで書きます。

### 関数からの値の受け取り方

関数が値を返した時、呼び出し側からはこれを「戻り」といいます。
戻りを受け取る方法を見てみましょう。

例えば以下のような関数があったとします。

```kotlin
fun sum(a: Int, b: Int) : Int {
  return a+b
}
```

この関数の返す値は、以下のように受け取ることが出来ます。

```kotlin
val c = sum(1, 2)
```

他にも、直接printlnで使ったりも出来ます。

```kotlin
println(sum(1, 2))
```

返す、というのは最初のうちは分かりにくいかもしれませんが、使ってると慣れるのでバンバン使ってみてください。

### 返す型が無い場合の補足

以下のような一番カンタンな例の関数について補足しておきます。

{% capture func_noreturn %}

fun printHello() {
  println("Hello World")
}

fun main() {
  printHello()
}
{% endcapture %}
{% include kotlin_quote.html body=func_noreturn %}

この時、printHelloはreturnをしていないので、「返す値が無い」ということになります。
何も返さない場合は、`Unit`という特殊な型の値を返しているとみなされます。
数字で何も無いをゼロで表現するようなものです。

つまり、printHelloは以下のように書いても全く同じ意味になります。

```kotlin
fun printHello() : Unit{
  println("Hello World")
}
```

また、戻りの型がUnitの関数では、何も式を指定しないreturnを使えます。

{% capture func_noreturn2 %}

fun printHoge(s: String) {
  println(s)
  if (s == "ほげ")
  {
    // 何も無いreturn。ここでこの関数を抜ける
    return
  }
  println("ほげじゃない！")
}

fun main() {
  printHoge("ほげ")
  printHoge("いか")
}
{% endcapture %}
{% include kotlin_quote.html body=func_noreturn2 %}

### 関数の名前の補足

関数の名前はなんでも良い、と言ったけれど、厳密にはちょっとした決まりはあります。
まず、数字で始まる関数名は禁止されています。
また、`if`、`fun`、`return`といった他の所で使われる名前は関数名には出来ません。

また、以下は禁止されている訳では無いけれど、
お行儀の良い人は守るルールとして、
関数の名前は小文字始まりで区切りは大文字にする、というのがあります。
つまりprintHelloとかsetupDataとか。

最後のは一応そういう風にしましょうね、という事になっているけれど、
別段そうしないと動かないという訳では無い。
ご飯を食べる前にいただきますと言いましょう、くらいのルールです。

### 関数は呼ばないと実行されない

ここらへんで、ちょっと以下のコードを見てください。

{% capture func-no-call1 %}
fun printHogeIka() {
  println("ほげ")
  println("いか")
}

fun main() {
}
{% endcapture %}
{% include kotlin_quote.html body=func-no-call1 %}

このコードは、「ほげ」や「いか」が表示されません。

これまでは、書いたコードが基本的には上から順番に一行ずつ実行される感じでしたが、
関数は作った時には実行されません。

関数は「呼んだ時」に実行されます。

上の例ではprintHogeIka関数を誰も呼んでないので、中の

```kotlin
  println("ほげ")
  println("いか")
```

は実行されない訳です。

以下のようにprintHogeIkaを「呼べば」実行されます。

{% capture func-no-call2 %}
fun printHogeIka() {
  println("ほげ")
  println("いか")
}

fun main() {
  // ここで呼んでる！
  printHogeIka()
}
{% endcapture %}
{% include kotlin_quote.html body=func-no-call2 %}

### mainという関数について簡単に説明

これまで、mainというのはあまり説明してきませんでした。
このページ上でkotlinのコードを書く時には、`fun main(){}`の中に書かないといけない、
と言ってきただけです。

ですが関数について理解した今、少しだけmainについて説明しておきましょう。

このmainも関数です。引数は無く、戻りの型はUnitです。

まず、kotlinでは基本的には全てのコードは何かの関数の中に書かないといけません。
関数の外にはコードは書けない。

けれど関数の中に書くだけだとコードは実行されません。
ということでkotlinのコードは実行されない…ということは無く、
「最初に呼ばれると決まっている関数」というのがあります。

Androidだとここはちょっと複雑ですが（ZygoteがforkしてActivityStackに応じてActivityThreadの関数が呼ばれる）、
Android以外のkotlinでは、「最初にmainという関数が呼ばれる」と決まっています。

このweb上でのkotlinのコードを実行するシステム（playgroundと言います）でも、
中のコードを全部実行したあとに、最後にmainという関数を呼び出す、と決まっています。

だから実行したいコードはmainの関数の中に書いておく必要があります。
自動で呼ばれるのはmain関数だけなので、この中から呼ばれない関数は実行されません。

また、main関数が無いとエラーになります。

以下のコードはエラーです。

{% capture func_main1 %}
// main関数が無い！
printHoge("ほげいか")
{% endcapture %}
{% include kotlin_quote.html body=func_main1 %}

以下のように、main関数の中に書く必要があります。

{% capture func_main2 %}
fun main() {
  // main関数の中で実行しているので、palygroundにより実行される
  printHoge("ほげいか")
}
{% endcapture %}
{% include kotlin_quote.html body=func_main2 %}

ただし、サンプルコードで一部を強調したい時に、
周りのコードを書くことがあります。

{% capture func_main3 %}
fun main() {
//sampleStart
  printHoge("ほげいか")
//sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=func_main3 %}

このコードは一見main関数が無いように見えますが、隠してあるだけです（上の+の所を押すと隠されたコードが見えます）。

- kotlinのコードは全部関数（またはクラス）の中に書かないといけない
- 関数は定義するだけでは実行されない
- main関数だけは自動で実行される

となっています。

### 練習問題

関数は説明するとややこしいけれど使うとそうでも無いので、バンバン練習問題をやって書いていきましょう。

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

{% capture func_intro_q1-a %}
```kotlin
// TODO: ここにmul関数を書け
fun mul(a: Int, b:Int) : Int {
  return a*b
}

fun main() {
  // TOOD: 以下でtrueが出力されるのを確認せよ
  println(mul(2, 3) == 6)
}
```
{% endcapture %}
{% include collapse_quote.html body=func_intro_q1-a title="解答例" %}


**課題: aとbを掛けたあとにcを足す、mulAddという関数を書け**

{% capture fun_intro_q2 %}

// TODO: ここにmulAdd関数を書け


fun main() {
  // 以下がtrueが出力されるのを確認せよ
  println(mulAdd(2, 3, 4) == 10)
}
{% endcapture %}
{% include kotlin_quote.html body=fun_intro_q2 %}

{% capture func_intro_q2-a %}
```kotlin
// TODO: ここにmulAdd関数を書け
fun mulAdd(a: Int, b:Int, c:Int) : Int {
  return a*b+c
}

fun main() {
  // 以下がtrueが出力されるのを確認せよ
  println(mulAdd(2, 3, 4) == 10)
}
```
{% endcapture %}
{% include collapse_quote.html body=func_intro_q2-a title="解答例" %}


**課題: 「ダネ〜」とprintlnした後に3を返す、fusigidaneという関数を書け**

{% capture fun_intro_q3 %}

// TODO: ここにfusigidane関数を書け


fun main() {
  // 以下で「ダネ〜」と出力された後にtrueが出力されるのを確認せよ
  println(fusigidane() == 3)
}
{% endcapture %}
{% include kotlin_quote.html body=fun_intro_q3 %}

{% capture func_intro_q3-a %}
```kotlin
// TODO: ここにfusigidane関数を書け
fun fusigidane() : Int {
  println("ダネ〜")
  return 3
}

fun main() {
  // 以下で「ダネ〜」と出力された後にtrueが出力されるのを確認せよ
  println(fusigidane() == 3)
}
```
{% endcapture %}
{% include collapse_quote.html body=func_intro_q3-a title="解答例" %}


**課題: 1+2+...+99+100した結果を返す、sumTillHundred関数を作れ**

till hundredは百まで、って意味です。

{% capture fun_intro_q4 %}

// TODO: ここにsumTillHundred関数を書け


fun main() {
  // 以下がtrueが出力されるのを確認せよ
  println(sumTillHundred() == 5050)
}
{% endcapture %}
{% include kotlin_quote.html body=fun_intro_q4 %}

{% capture func_intro_q4-a %}
```kotlin
// TODO: ここにsumTillHundred関数を書け
fun sumTllHundred() : Int {
  var sum = 0
  for(a in 1..100) {
    sum += a
  }
  return sum
}

fun main() {
  // 以下がtrueが出力されるのを確認せよ
  println(sumTillHundred() == 5050)
}
```
{% endcapture %}
{% include collapse_quote.html body=func_intro_q4-a title="解答例" %}


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

{% capture fun_intro_q5-hint %}
奇数は2で割ったあまりが1。2で割った余りは`%`を使う。例： `a % 2`
{% endcapture %}
{% include collapse_quote.html body=fun_intro_q5-hint title="ヒント" %}


{% capture func_intro_q5-a %}
```kotlin
// TODO: ここにsumOdd関数を書け
fun sumOdd() : Int {
  var sum = 0
  for(a in 1..99) {
    if((a%2) == 1) {
      sum += a
    }
  }
  return sum
}

fun main() {
  // 以下がtrueが出力されるのを確認せよ
  println(sumOdd() == 2500)
}
```
{% endcapture %}
{% include collapse_quote.html body=func_intro_q5-a title="解答例" %}



**課題: 1, 3, 5, ..., 97, 99の要素を持つListを返す関数、oddList関数を作れ**

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

{% capture fun_intro_q6-hint %}
関数の定義で戻りをListとしておくと、MutableListをreturnしてもListになります。
{% endcapture %}
{% include collapse_quote.html body=fun_intro_q6-hint title="ヒント1" %}

{% capture fun_intro_q6-hint2 %}
Listなどの型は要素の型を`<>`で指定する必要があります
{% endcapture %}
{% include collapse_quote.html body=fun_intro_q6-hint2 title="ヒント2" %}

{% capture func_intro_q6-a %}
```kotlin
// TODO: ここにoddList関数を書け
fun oddList() : List<Int> {
  val mlist = mutableListOf<Int>()
  for(a in 1..99) {
    if((a%2) == 1) {
      mlist.add(a)
    }
  }
  return mlist
}

fun main() {
  // 以下が全てtrueが出力されるのを確認せよ
  val olist = oddList()
  println(olist.size == 50)
  println(olist[0] == 1)
  println(olist[15] == 31)
  println(olist.sum() == 2500)
}
```
{% endcapture %}
{% include collapse_quote.html body=func_intro_q6-a title="解答例" %}

**課題: 指定された範囲内の奇数を全部足した結果を返す、sumOdd関数を作れ**

`sumOdd(1, 4)`とすると、1から4までの間の奇数を足した結果、つまり`1+3`を返せ。
`sumOdd(1, 10)`とすると、`1+3+5+7+9`を返せ。

{% capture fun_intro_q7 %}
// TODO: ここにsumOdd関数を書け


fun main() {
  // 以下がtrueが出力されるのを確認せよ
  println(sumOdd(1, 3) == 4)
  println(sumOdd(1, 4) == 4)
  println(sumOdd(3, 5) == 8)
  println(sumOdd(1, 9) == 25)
  println(sumOdd(1, 99) == 2500)
}
{% endcapture %}
{% include kotlin_quote.html body=fun_intro_q7 %}


{% capture func_intro_q7-a %}
```kotlin
// TODO: ここにsumOdd関数を書け
fun sumOdd(a: Int, b:Int) : Int {
  var sum = 0
  for(n in a..b) {
    if((n%2) == 1) {
      sum += n
    }
  }
  return sum
}

fun main() {
  // 以下がtrueが出力されるのを確認せよ
  println(sumOdd() == 2500)
}
```
{% endcapture %}
{% include collapse_quote.html body=func_intro_q7-a title="解答例" %}



**課題: Stringのリストの奇数番目だけを含んだ新しいリストを返す、oddOnly関数を書け**

渡されたStringのリストから、1番目、3番目、5番目、7番目…と奇数番目だけの要素を持った新しいリストを作って返す、oddOnlyを作れ。
0番目から数える事にする（以下のprintlnが全部trueになるように考えてくれ）

{% capture fun_intro_q8 %}

// TODO: ここにoddOnly関数を書け

fun main() {

  val list = listOf("0番目",  "1番目", "2番目", "3番目", "4番目", "5番目")

  val olist = oddOnly(list)
  // 以下が全てtrueなのを確認せよ。 
  println(olist.size == 3)
  println(olist[1] == "3番目")
}
{% endcapture %}
{% include kotlin_quote.html body=fun_intro_q8 %}

{% capture fun_intro_q8-hint %}
withIndexを使うといいでしょう。[リスト入門](list_intro.md)参照。
{% endcapture %}
{% include collapse_quote.html body=fun_intro_q8-hint title="ヒント1" %}


{% capture func_intro_q8-a %}
```kotlin
// TODO: ここにoddList関数を書け
fun oddOnly() : List<String> {
  val mlist = mutableListOf<String>()

  for((index, elem) in mlist.withIndex()) {
    if((index%2) == 1) {
      mlist.add(elem)
    }
  }
  return mlist
}

fun main() {

  val list = listOf("0番目",  "1番目", "2番目", "3番目", "4番目", "5番目")

  val olist = oddOnly(list)
  // 以下が全てtrueなのを確認せよ。 
  println(olist.size == 3)
  println(olist[1] == "3番目")
}
```
{% endcapture %}
{% include collapse_quote.html body=func_intro_q8-a title="解答例" %}


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
4. 「2」でもその関数を呼び出して、最初が「"0"」だった時の処理がうまく動いているか確認
5. [List入門](list_intro.md)の「電卓の課題をfor文化出来ないか考えてみる」あたりを参考に全部のボタンで同じ処理をする


## より概念的な事を知りたい人はJS入門をやってください

関数とはどういうものか、についてより詳しい説明が欲しい場合は、
以下のJS入門の 第七回〜第九回 をやってください。 

- [第七回: 関数その1 関数を作る事、呼び出す事 - 算数で挫折した人向けの、Javascript入門](https://karino2.github.io/js-introduction/ch07.html)
- [第八回: 関数その2 関数からもらうもの - 算数で挫折した人向けの、Javascript入門](https://karino2.github.io/js-introduction/ch08.html)
- [第九回: 関数その3 関数に渡すもの - 算数で挫折した人向けの、Javascript入門](https://karino2.github.io/js-introduction/ch09.html)


