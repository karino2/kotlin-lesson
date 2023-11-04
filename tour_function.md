---
title: "ツアー副読本：関数"
layout: page
---

[関数（ツアー） - Kotlin 日本語リファレンス](https://karino2.github.io/kotlin-web-site-ja/docs/kotlin-tour-functions.html)を読んでいく時の補助教材です。

## 最初に読む時

初めてツアーをやる時は、ラムダ式の手前まで読んだら次のクラスに進む方がいいと思います。
クラスが終わってからラムダ式に戻ってきましょう。

単一式関数までは特に補足無しで読むだけで分かるでしょう。

以下は、クラスが終わってから戻ってきてください。

## ラムダ式

ラムダ式の所は他の言語でラムダ式や関数オブジェクトを使った事が無い人には難解な説明かもしれません。

ラムダ式とは何か、という事について以下に動画を作りました。

<iframe width="560" height="315" src="https://www.youtube.com/embed/dtOIfBsLbAM?si=rMDcX0gnMm0hoG2Q" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### filterがなんか中括弧なんだけど？

「他の関数に渡す」の所で、例が以下のようなコードになっています。

```kotlin
val numbers = listOf(1, -2, 3, -4, 5, -6)
val positives = numbers.filter { x -> x > 0 }
val negatives = numbers.filter { x -> x < 0 }
println(positives)
// [1, 3, 5]
println(negatives)
// [-2, -4, -6]
```

filterの所、なんか`filter()`といいつつ呼ぶ時は`filter {}`になっててどういう事？
という感じがしますが、
これは後に出てくる「トレーリングラムダ」という省略記法です。

現時点では、このコードは以下と同じなので、以下が理解出来れば良いです。

{% capture kotlin-tour-lambda-filter %}
fun main() {
    //sampleStart
    val numbers = listOf(1, -2, 3, -4, 5, -6)
    val positives = numbers.filter( { x -> x > 0 } )
    val negatives = numbers.filter( { x -> x < 0 } )
    println(positives)
    // [1, 3, 5]
    println(negatives)
    // [-2, -4, -6]
    //sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=kotlin-tour-lambda-filter %}

これはnumbersのfilterというメソッドにラムダ式を渡しています。
ラムダ式は `{ x -> x > 0 }` と `{ x -> x < 0 }` の部分です。

この後に囲み記事で書いてある「もしラムダ式が唯一の関数のパラメータの場合〜」というのは、
この事を言っています。

次のmapの例も同じです。

### 「関数の型」のあたりがなんだかわからん

「関数の型」のあたりは解説がやや難しいかもしれない。

変数には型をつける事も出来る、という話がありました。
つまり、以下の２つのコードは同じ意味です。

```kotlin
val s = "ほげ"
val s : String = "ほげ"
```

型の指定は多くの場合で必要無いけれど、以下の２つの場所では必要な事があった。

- varで変数を作って後で入れる場合
- 関数の引数

前者はあまり出番が無いので主に後者が問題となる。
例えば以下のような関数を考える時、

```
fun printS(s: String) {
  println(s)
}
```

このsには型の指定、つまり`: String`の部分が必要だった。
これは、通常は

```
val s = "ほげ"
```

のようにイコールの右側から型が推測出来るのでつける必要が無いけれど、
関数は作る時点ではどの値が入るか分からないので推測に使えるデータが無いからだ。

理屈はおいといて、関数の引数では型を指定する必要がある。

### 引数に関数を渡す場合の指定

さて、ここで少しややこしい、「関数に関数を渡す」という事を考える。どういう事か？

先程のラムダ式の動画を見れば分かるように、
関数というのは関数オブジェクトというのが作られて、
それを「呼ぶ」事で実行するという話だった。

だから「呼ぶ前」の関数オブジェクトを、他の関数に引数で渡す事が出来る。

例えば以下の「？？ここはどうする？？」の部分を除いたコードを考える。

```kotlin
fun callF(s: String, myFunc: ？？ここをどうする？？) {
  myFunc(s)
}
```

callFという関数に、myFuncという関数を渡すようなコードだ。
渡された関数を `myFunc(s)` という風に呼び出している。

このように、関数を他の関数の引数にわたす、という事が出来る。

使う側は例えば以下のようになる。

```
callF("いかいか", {str:String -> println("ほげほげ: ${str}")})
```

さて、先程の関数の定義に戻ろう。

```kotlin
fun callF(s: String, myFunc: ？？ここをどうする？？) {
  ... 省略 ...
}
```

「？？ここをどうする？？」の所には何を書いたらいいだろうか？
ここには`{str:String -> println("ほげほげ: ${str}")}`の「型」が入る。

ラムダ式の型だが、ラムダ式は関数の一種だったのでこれは関数の型になる。

より具体的には、以下のように書いた時、

```
val f = {str:String -> println("ほげほげ: ${str}")}
```

このfの型はなんだろうか？というのがこの「関数の型」という所で解説している所だ。

ちなみに答えは`(String)->Unit`になる。

試してみよう。

{% capture pass-func-as-arg %}
fun callF(s: String, myFunc: (String)->Unit) {
  myFunc(s)
}


fun main() {
  callF("いかいか", {str:String -> println("ほげほげ: ${str}")})
}
{% endcapture %}
{% include kotlin_quote.html body=pass-func-as-arg %}


なんだかややこしいが、`(String)->Unit`が、これまでの`String`とか`Int`とかと同じように「型」である、
という事に注意すると解読出来ると思う。

なおこのcallFはかなりややこしいので、頑張って解読して理解して欲しい。
なんでこんな事をするのかはもうちょっと後で解説するので、
この時点ではcallFのコードを解読出来ればOK。

ちなみに、以下のコードは

```
val f = {str:String -> println("ほげほげ: ${str}")}
```

以下と同じ意味となる。

```
val f :(String)->Unit = {str:String -> println("ほげほげ: ${str}")}
```

### 「単独で呼び出す」が意味わからんのだが？

「単独で呼び出す」に、以下のようなコードがある。

{% capture kotlin-tour-lambda-standalone %}
fun main() {
    //sampleStart
    println({ string: String -> string.uppercase() }("hello"))
    // HELLO
    //sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=kotlin-tour-lambda-standalone %}


これがすんなり分かるかは人によるだろう。

という事ですんなりわからん人向けに簡単に解説を書く。
以下のコードは、

```kotlin
println({ string: String -> string.uppercase() }("hello"))
```

printlnの中で一気に２つの事をやっている

- ラムダ式で関数を作る
- その関数を呼び出す

これはややこしいので、この２つを間に変数をつかって分けて書くと以下のようになる。

```kotlin
val f = { string: String -> string.uppercase() }
println(f("hello"))
```

ラムダ式を作って、それを`"hello"`を引数に呼び出している訳だ。

これでも良く分からないなら、ラムダ式じゃない普通のfunで書き直してみても良い。
書き直してみると以下のようになる。

{% capture use-old-style %}
fun f(string: String) : String {
  return string.uppercase()
}

fun main() {
  println(f("hello"))
}
{% endcapture %}
{% include kotlin_quote.html body=use-old-style %}

つまり、

```kotlin
println({ string: String -> string.uppercase() }("hello"))
```

も

```kotlin
val f = { string: String -> string.uppercase() }
println(f("hello"))
```

も、

```kotlin
fun f(string: String) : String {
  return string.uppercase()
}
println(f("hello"))
```

も、同じ意味となる。
ラムダを直接呼ぶのは慣れるまでは読みにくいので、
良く分からない時は一旦変数fに入れるコードに書き直してみると良い。

### トレーリングラムダのあたりが良くわからんのだが？

唐突にfoldとか出てきて、何言ってるのおまえ？という気分になる人もいるかもしれないので、
もうちょっと簡単な例でトレーリングラムダの解説を書いておく。

以下のコードがあった。

{% capture pass-func-as-arg2 %}
fun callF(s: String, myFunc: (String)->Unit) {
  myFunc(s)
}


fun main() {
  callF("いかいか", {str:String -> println("ほげほげ: ${str}")})
}
{% endcapture %}
{% include kotlin_quote.html body=pass-func-as-arg2 %}

この呼び出す側、つまり以下の行が、

```kotlin
callF("いかいか", {str:String -> println("ほげほげ: ${str}")})
```

最後の引数がラムダ式になっている。
この場合、kotlinにはトレーリングラムダというルールがあって、
以下のように書いても良い、という風になっている。

```kotlin
callF("いかいか") {str:String -> println("ほげほげ: ${str}")}
```

最後の引数を、カッコの外に出しても良い。
なんでこうなっているかは後回しにして、とりあえずトレーリングラムダとはこれの事だ、
という事をここでは理解して欲しい。

実際に試してみよう。

{% capture pass-func-as-arg3 %}
fun callF(s: String, myFunc: (String)->Unit) {
  myFunc(s)
}

fun main() {
  //sampleStart

  // こう書いてもいいが
  // callF("いかいか", {str:String -> println("ほげほげ: ${str}")})

  // 以下みたいに書いてもいい
  callF("いかいか") {str:String -> println("ほげほげ: ${str}")}
  //sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=pass-func-as-arg3 %}

さて、これがトレーリングラムダだが、
もう一つルールがある。

それは、「ラムダを外に出した時に、他に引数が残っていなければ`()`を省略出来る」というもの。

前の方でfilterの例があった。

```kotlin
val numbers = listOf(1, -2, 3, -4, 5, -6)
val positives = numbers.filter( { x -> x > 0 } )
```

この二行目、以下の行は、

```kotlin
val positives = numbers.filter( { x -> x > 0 } )
```

トレーリングラムダを使うと以下のように書ける。

```kotlin
val positives = numbers.filter() { x -> x > 0 }
```

すると、filterの引数が何も残らないので、この`filter()`の`()`は省略出来て、以下のように書ける。

```kotlin
val positives = numbers.filter { x -> x > 0 }
```

これがトレーリングラムダの２つ目のルール、「ラムダを外に出した時に、他に引数が残っていなければ`()`を省略出来る」だ。
逆に言えば、以下のコードは

```kotlin
val positives = numbers.filter { x -> x > 0 }
```

いつも、以下のコードと同じ意味になる。

```kotlin
val positives = numbers.filter( { x -> x > 0 } )
```

慣れるまではこれに戻しても良い。

## いろいろ書いてみよう

ここまでの説明だと、だいたいが「これ何に使うの？」という事ばかりだったと思う。
という事で具体的に使う例をいろいろ書いてみよう。

なお、この例はどれもかなり難しいので、出来なかったら出来なかったでもOK。

### リストの要素に対して順番に渡された関数を実行する、myForeachを書く

リストの要素に対して、順番に処理をする、myForeachを考えよう。
型はいろいろ決めてしまう。

例えば以下のようなリストがあった時に、

```kotlin
val list = listOf("ほげ", "いか", "ふが")
```

これを順番にprintlnするには以下のようにすれば良い。

```kotlin
for(a in list) {
  println(a)
}
```

では、前に「むえぇ〜：」をつけてprintlnしたい場合はどうだろう？

それは以下のように書けば良いだろう。

```kotlin
for(a in list) {
  println("むえぇ〜：" + a)
}
```

この両者は、prntlnの所以外は同じだ。
同じ所は関数にして再利用したい、というのは良くある。

だから、以下の部分を再利用したい

```kotin
for(a in list) {
  // 「この中は使う人が決める」
}
```

こういう時に、「この中は使う人が決める」の部分を関数として外から渡すようにするのが、
良くあるやり方である。

言葉でごちゃごちゃ言われてもわからん、という人も居ると思うので、
先に答えを見てみよう。

以下のようになる。

```kotlin
fun myForeach(list: List<String>, f: (String)->Unit) {
  for(a in list) {
    f(a)
  }
}
```

このうち、以下の部分を見ると、

```kotlin
  for(a in list) {
    f(a)
  }
```

「この中は使う人が決める」の所が、`f(a)`になっている。

以上を実際に使ってみよう。


{% capture myforeach-1 %}
fun myForeach(list: List<String>, f: (String)->Unit) {
  for(a in list) {
    f(a)
  }
}

fun main() {
  val list = listOf("ほげ", "いか", "ふが")
  myForeach(list) { s-> println(s) }
  myForeach(list) { s-> println("むえぇ〜：" + s) }
}
{% endcapture %}
{% include kotlin_quote.html body=myforeach-1 %}

なお、ラムダ式を外に出すトレーリングラムダを使っている。
以下と同じ意味です。

```kotlin
  myForeach(list, { s-> println(s) })
  myForeach(list, { s-> println("むえぇ〜：" + s) })
```

**課題1: myForeachを使って、文字列を全部つなげるコードを完成させよ**

「ほげいかふが」と出力するように、連結するコードを書け。

{% capture myforeach-q1 %}
fun myForeach(list: List<String>, f: (String)->Unit) {
  for(a in list) {
    f(a)
  }
}

fun main() {
  val list = listOf("ほげ", "いか", "ふが")

  var s = ""
  // TODO: 以下でmyForeachとラムダ式で、sにlistの中身を連結せよ

  // 以下はいじらない
  println(s == "ほげいかふが")
}
{% endcapture %}
{% include kotlin_quote.html body=myforeach-q1 %}


{% capture myforeach-q1-a %}
```kotlin
fun myForeach(list: List<String>, f: (String)->Unit) {
  for(a in list) {
    f(a)
  }
}

fun main() {
  val list = listOf("ほげ", "いか", "ふが")

  var s = ""
  // TODO: 以下でmyForeachとラムダ式で、sにlistの中身を連結せよ
  myForeach(list) { s1-> s+=s1 }

  // 以下はいじらない
  println(s == "ほげいかふが")
}
```
{% endcapture %}
{% include collapse_quote.html body=myforeach-q1-a title="解答例" %}

**課題2: myForeachを自分で書け**

こういうのは、自分で書いてみないと分からないものなので、
先程のmyForeachを自分で書いてみよう。

先程の例に答えがあるので見ながら書いてもいいけれど、最終的には見ないで書けるように何度か書いてみよう。

{% capture myforeach-q2 %}
// TODO: ここにmyForeachを自分で書け

// 以下はいじらない
fun main() {
  val list = listOf("ほげ", "いか", "ふが")

  myForeach(list, { s-> println("むえぇ〜：" + s) })
}
{% endcapture %}
{% include kotlin_quote.html body=myforeach-q2 %}

**課題3: 全部の要素を二倍して渡すmyTwiceForeachを作ろう**

myForeachは文字列のリストを渡しましたが、今度はIntのリストを渡して、それを二倍して関数を呼び出すmyTwiceForeachを作りましょう。

良く意味が分からない人はmainの中を見て使い道を予想してください。

{% capture myforeach-q3 %}
// TODO: ここにmyTwiceForeachを書く

// 以下はいじらない
fun main() {
  val list = listOf(5, 4, 3)

  myTwiceForeach(list) { i: Int -> println("要素は2倍すると「${i}」です")}
}
{% endcapture %}
{% include kotlin_quote.html body=myforeach-q3 %}

{% capture myforeach-q3-hint %}
リストがIntなので、関数の型は`(Int)->Unit`となる。
{% endcapture %}
{% include collapse_quote.html body=myforeach-q3-hint title="ヒント" %}

{% capture myforeach-q3-hint2 %}
やりたい事をまずfor文で考えてみると、
```kotlin
for(a in list) {
  val a2 = 2*a
  // 「ここにa2を使った何かを実行する」
}
```
のようになる。「ここにa2を使った何かを実行する」の所が関数になる。
回答ではわざわざa2を作らなくてもいいが、作っても良い。
{% endcapture %}
{% include collapse_quote.html body=myforeach-q3-hint2 title="ヒント2" %}

{% capture myforeach-q3-a %}
```kotlin
// TODO: ここにmyTwiceForeachを書く
fun myTwiceForeach(list: List<Int>, f: (Int)->Unit) {
  for(a in list) {
    f(2*a)
  }
}

// 以下はいじらない
fun main() {
  val list = listOf(5, 4, 3)

  myTwiceForeach(list) { i: Int -> println("要素は2倍すると「${i}」です")}
}
```
{% endcapture %}
{% include collapse_quote.html body=myforeach-q3-a title="解答例" %}

**課題4: 条件に合う要素だけを含んだリストを返す、myFilterを作ろう**

条件にあった要素だけを含んだリストを返す、myFilterを作ろう。
mainの関数を見て、何を作らなきゃいけないかを理解してください。

なお、この課題はかなり難しいので一旦答えを見て理解したあとにもう一度やり直してもいいかもしれない。
次のmyMapもほとんど同じような問題なので、ある程度理解できたら次のmyMapを解いてみるといいかもしれません。

また、この課題を元に、myFilterを使って「偶数のリスト」とか「奇数のリスト」などを取り出す方法も考えてみてください。

{% capture myfilter-q1 %}
// TODO: ここにmyFilterを書く

// 以下はいじらない
fun main() {
  val numbers = listOf(1, -2, 3, -4, 5, -6)
  val positives = myFilter(numbers, { x -> x > 0 })
  val negatives = myFilter(numbers) { x -> x < 0 } // トレーリングラムダ

  println(positives)
  // [1, 3, 5]
  println(negatives)
  // [-2, -4, -6]

}
{% endcapture %}
{% include kotlin_quote.html body=myfilter-q1 %}

{% capture myfilter-q1-hint %}
引数の関数はIntを引数にとりBooleanを返す関数。

また、このmyFilter自身はリストを返すので戻りの型は`List<Int>`。
でもこの問題としては`MutableList<Int>`でもいいです。
{% endcapture %}
{% include collapse_quote.html body=myfilter-q1-hint title="ヒント" %}

{% capture myfilter-q1-hint2 %}
やりたい事をまずfor文で考えてみると、
```kotlin
val ret = mutableListOf<Int>()
for(a in list) {
  if (/* ここに何かを考える */ ) {
    ret.add(a)
  }
}
```
みたいな事をやりたい。このretを返したい。
{% endcapture %}
{% include collapse_quote.html body=myfilter-q1-hint2 title="ヒント2" %}

{% capture myfilter-q1-a %}
```kotlin
// TODO: ここにmyFilterを書く
fun myFilter(list: List<Int>, f: (Int)->Boolean) : MutableList<Int> {
  val ret = mutableListOf<Int>()
  for(a in list) {
    if(f(a)) {
      ret.add(a)
    }
  }
  return ret
}

// 以下はいじらない
fun main() {
  val numbers = listOf(1, -2, 3, -4, 5, -6)
  val positives = myFilter(numbers, { x -> x > 0 })
  val negatives = myFilter(numbers) { x -> x < 0 } // トレーリングラムダ

  println(positives)
  // [1, 3, 5]
  println(negatives)
  // [-2, -4, -6]

}
```
{% endcapture %}
{% include collapse_quote.html body=myfilter-q1-a title="解答例" %}

**課題5: myFilter2を使って、先頭がストIIの要素だけ取り出せ**

今度は使う側のコードです。
ストIIXは本来はスパIIXですが、そこは気にしないということで。

{% capture myfilter-q2 %}
fun myFilter2(list: List<String>, pred: (String)->Boolean) : List<String> {
  val ret = mutableListOf<String>()
  for(s in list) {
    if(pred(s))
      ret.add(s)
  }
  return ret
}

fun main() {
  val kakugee = listOf("餓狼伝説スペシャル", "ストII", "ストIIダッシュ", "ヴァンパイアセイバー", "ヴァンパイア", "サムライスピリッツ", "ストIIX")

  // 以下をmyFilter2を使って書き直せ
  val suto2 = listOf<String>()

  println(suto2)
}
{% endcapture %}
{% include kotlin_quote.html body=myfilter-q2 %}

{% capture myfilter-q2-hint %}
文字列の先頭が一致しているかは`s.startsWith("ほげ")`で判定するのだった。

[テキスト処理入門](text_op_intro.md)参照。
{% endcapture %}
{% include collapse_quote.html body=myfilter-q2-hint title="ヒント" %}

{% capture myfilter-q2-a %}
```kotlin
fun myFilter2(list: List<String>, pred: (String)->Boolean) : List<String> {
  val ret = mutableListOf<String>()
  for(s in list) {
    if(pred(s))
      ret.add(s)
  }
  return ret
}

fun main() {
  val kakugee = listOf("餓狼伝説スペシャル", "ストII", "ストIIダッシュ", "ヴァンパイアセイバー", "ヴァンパイア", "サムライスピリッツ", "ストIIX", "餓狼伝説2")

  // 以下をmyFilter2を使って書き直せ
  val suto2 = myFilter2(kakugee) { str-> str.startsWith("ストII") }

  println(suto2)
}
```
{% endcapture %}
{% include collapse_quote.html body=myfilter-q2-a title="解答例" %}


**課題6: 各要素に渡された変形をほどこしてその結果をリストにしたものを返す、myMapを作ろう**

要素を変形する関数を引数にとって、変形した結果をリストにするmyMapを作ろう。
これも良く分からなければmain関数を見て必要なものを考えてください。


{% capture myMap-q1 %}
// TODO: ここにmyMapを書く

// 以下はいじらない
fun main() {
  val numbers = listOf(1, -2, 3, -4, 5, -6)
  val doubled = myMap(numbers) { x -> x * 2 }
  val tripled = myMap(numbers) { x -> x * 3 }
  println(doubled)
  // [2, -4, 6, -8, 10, -12]
  println(tripled)
  // [3, -6, 9, -12, 15, -18]

}
{% endcapture %}
{% include kotlin_quote.html body=myMap-q1 %}

{% capture myMap-q1-hint %}
やりたい事をとりあえずmainの中でfor文で書いてみよう。
それを見ながら関数に抜き出す方法を考えよう。
{% endcapture %}
{% include collapse_quote.html body=myMap-q1-hint title="ヒント" %}


{% capture myMap-q1-a %}
```kotlin
// TODO: ここにmyMapを書く
fun myMap(list: List<Int>, f: (Int)->Int) : MutableList<Int> {
  val ret = mutableListOf<Int>()
  for(a in list) {
    val a2 = f(a)
    ret.add(a2)
  }
  return ret
}

// 以下はいじらない
fun main() {
  val numbers = listOf(1, -2, 3, -4, 5, -6)
  val doubled = myMap(numbers) { x -> x * 2 }
  val tripled = myMap(numbers) { x -> x * 3 }
  println(doubled)
  // [2, -4, 6, -8, 10, -12]
  println(tripled)
  // [3, -6, 9, -12, 15, -18]

}
```
{% endcapture %}
{% include collapse_quote.html body=myMap-q1-a title="解答例" %}

## setOnClickListenerを考え直す

これまで、以下のようなコードを書いてきた。

```kotlin
findViewById<Button>(R.id.button1).setOnClickListener {
  findViewById<TextView>(R.id.label1).text = "むぇ〜〜"
}
```

これはラムダ式を渡していて、トレーリングラムダを使っています。
トレーリングラムダを使わないコードにすると以下です。

```kotlin
findViewById<Button>(R.id.button1).setOnClickListener({ findViewById<TextView>(R.id.label1).text = "むぇ〜〜" })
```

変数にするともう少し分かりやすくなるかもしれない。

```kotlin
val f = { findViewById<TextView>(R.id.label1).text = "むぇ〜〜" }
findViewById<Button>(R.id.button1).setOnClickListener(f)
```

fというラムダ式を作って、それをsetOnClickListenerに渡していたのでした。