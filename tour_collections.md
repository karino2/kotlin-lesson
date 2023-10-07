---
title: "ツアー副読本：コレクション"
layout: page
---

[コレクション（ツアー） - Kotlin 日本語リファレンス](https://karino2.github.io/kotlin-web-site-ja/docs/kotlin-tour-collections.html)を読んでいく時の補助教材です。

## コレクションのページの概要

コレクションとは、要素をグルーピングして持つものです。
具体的には以下のような物達のことです。

- リスト
- セット
- マップ

このうち、リストはここまで良く使ってきたのでだいたい分かると思います。

[List入門](list_intro.md)にも比較的詳しい話をしました。

セットはさっぱり分からず、マップはちょっと分かるけれどあんま使ってないので良く分からないこともある、くらいの感じだと思います。
ここでは、

- リストが良く分かる
- セットはなんに使うのかは分からんけどどういう物かはぼんやりと分かる
- マップはまぁまぁ分かる

くらいの理解を目指しましょう。
ようするにマップを集中的に学ぶ所、と考えると良い。

## リスト

リストにはだいたい以下の話があります。

- 作成
   - listOfとmutalbeListOf
- 要素へのアクセス
   - インデックスでのアクセス`[]`
   - `first()`と`last()`
- 要素の追加と削除
   - `add()`と`remove()`
- そのほか
   - `count()`
   - `in`演算子

これくらいが分かっていれば十分でしょう。
以下簡単に補足します。

{% capture comment1 %}
**演算子って何よ？**  
演算子、という言葉が出てきます。これは何か、というのはちょっと説明が難しいですが、既に知っているものなのでそれを並べてみます。
`+`は演算子です。`-`も`*`も`/`もそうです。`&&`とかもそうだし、条件式の所に書くビックリマーク、例えば`if(!(a == 3))`とかの`!`も演算子です。
`==`も演算子です。演算子というのはこういう類のヤツです。
記号っぽい見た目で、なんか計算をするもの。

`in`も演算子です。`"hoge" in list` と書くと、この結果はtrueかfalseになります。

演算子は関数との境界がかなり曖昧なので、`+`とか`-`とか`&&`とか`==`とかそういう感じのを演算子と呼ぶ、
とふんわり覚えておくのがいいでしょう。
{% endcapture %}
{% include myquote.html body=comment1 %}



### MutableListからListへのキャストの話

なんか以下みたいなコードが書いてあって、

```kotlin
  val shapes: MutableList<String> = mutableListOf("triangle", "square", "circle")
  val shapesLocked: List<String> = shapes
```

それがキャストとかいう難しそうな言葉だ、と書いてある所があります。
ここはそれほど重要でも無いですが、大して難しい話でも無いので理解しておきましょう。

まず一行目でmutableリストを作っています。

```kotlin
  val shapes: MutableList<String> = mutableListOf("triangle", "square", "circle")
```

で、二行目ではこれを違う変数に代入しているだけに見えます（し、実際そうです）。

```kotlin
  val shapesLocked: List<String> = shapes
```

何これ？意味あるのか？となると思いますが、このshapesLockedの方は型が`List<String>`になる、というのが違いです。
ようするに、以下のようには出来ますが、

{% capture cast-immutable-ex1 %}
fun main() {
//sampleStart
  val shapes: MutableList<String> = mutableListOf("triangle", "square", "circle")

  // mutableなリストなのでadd出来る
  shapes.add("ほげほげ")

  println(shapes)
//sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=cast-immutable-ex1 %}

以下のようには出来ない、ということです。

{% capture cast-immutable-ex2 %}
fun main() {
//sampleStart
  val shapes: MutableList<String> = mutableListOf("triangle", "square", "circle")
  val shapesLocked: List<String> = shapes

  // shapesLockedはイミュータブルなのでadd出来ない：エラー
  shapesLocked.add("ほげほげ")

  println(shapesLocked)
//sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=cast-immutable-ex2 %}

同じリストだけど入れてる変数の型を変えるとやることが制限出来る訳です。
もちろん、イミュータブルなリストでも出来ることは出来ますし、

{% capture cast-immutable-ex3 %}
fun main() {
//sampleStart
  val shapes: MutableList<String> = mutableListOf("triangle", "square", "circle")
  val shapesLocked: List<String> = shapes

  // for文で回したりは出来る
  for(item in shapesLocked) {
    println(item)
  }
//sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=cast-immutable-ex3 %}

しかもmutableな方のリストに追加すれば、immutableな方の変数にも結果は反映されます（同じリストなので）

{% capture cast-immutable-ex4 %}
fun main() {
//sampleStart
  val shapes: MutableList<String> = mutableListOf("triangle", "square", "circle")
  val shapesLocked: List<String> = shapes

  println("足す前は：")
  println(shapesLocked)

  // shapesの方に足す
  shapes.add("むぇ〜〜〜")

  // shapesLockedの方を見てもたされている（同じリストなので）
  println("足した後は：")
  println(shapesLocked)
//sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=cast-immutable-ex4 %}

なんでこんなことをするか？というと、
追加とか削除というのはバグりやすい処理なので、
コードの「この範囲でだけしかそういうことはしていません」とはっきりさせておく方がバグが起こりにくいからです。

具体的にはonCreateの所でだけファイルから読んでリストを作って、
ListViewの側では表示しかしない、
みたいな時には、ListView側にはこのshapesLockedの方を使うようにしておけば、
うっかりadapterの中のgetViewで書き換えてしまったりしない、ということですね。

そして以下の行。

```kotlin
  val shapesLocked: List<String> = shapes
```

右側はMutableListで左側はListと違う型になっているのですが、
こういう風に違う型に変換することをキャスト、といいます。
上の例では代入の所でキャストしていますが、他にもキャストの仕方はいろいろあります（それは将来出てくるので現時点では気にしなくてOK）

ここでは

- Mutableなリストをイミュータブルな型の変数に入れることが出来る
- そうするとイミュータブルな型の変数側ではaddとかremoveができなくなる
- そういうのをキャストって呼ぶらしい

くらいが分かればOKです。

### firstとlastがextension関数とか良く分からないことを言ってくるんだけど？

良く分からない言葉が出てくると、全然分からん！って気分になりますが、
こういうのは言葉に触れさせておいてだんだんと慣れさせるのが目的なので、
出てくる都度分かってるかどうかをあんまり気にする必要はありません。

ここで重要なのは、`first()`と`last()`が何をするか、ということです。
例えば以下のようなリストがあったとして、

```kotlin
val list = listOf("ストII", "ガロスペ", "空手道", "ワールドヒーローズ")
```

とあった時に、`list.first()`と`list.last()`がそれぞれ何か？というのが分かっていればOKです。
ちなみにこれがこの場合は`list[0]`と`list[3]`と同じ、と分かっていれば完璧。

{% capture first-last %}
fun main() {
//sampleStart
  val list = listOf("ストII", "ガロスペ", "空手道", "ワールドヒーローズ")
  println("list.first()は？")
  println(list.first())

  println("list.last()は？")
  println(list.last())
//sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=first-last %}

extension関数というのは、なんかそういうのがあるらしいな、なんだか知らんけど。
と思っておけばOK。
自分たちの使っているものの中にはそういうものが混ざっているということですね。

## セット

セットはあまり使わないので、良く分からないままでいいと思います。
とりあえず以下くらいをふわっと分かっておけばOK。

- setOfとかで作れるらしい
- addとかremove出来るらしい
- inで入っているかどうかを確認出来るらしい
- なんでリストじゃダメなの？というのは良く分からん

## マップ

マップは良く使うのだけれど、ここまであまり出てこなかったのでこの辺でしっかりやっておきたい。
マップに関しては[コレクション概要](collection.md)でちょっとやったけれど、その後使う所が無かった。
ここで少し見ていきます。

### ツアーのページに書いてある内容

マップに関しては以下の話がおおまかに書いてあります。

1. 作成の仕方(mapOfとmutableMapOfだが、`to`というのを使う所が新しい)
2. キーで値を取る方法`[]`
3. 要素の追加と削除、`put`と`remove`
  - `put`が引数２つ
  - `remove`はキーで削除
4. `keys`と`values`
5. そのほか
  - `count()`
  - `containsKey()`

このうち、1と2と3は書いてあることはそんなに難しくは無いと思いますし、上の「コレクション概要」で既にやった部分もあるので分かると思います。
ただし2についてはツアーには無い話で少し追加で説明する事があるので説明します。

新しくて解説がもっと欲しいのは4と5の二番目の`containsKey()`の２つだと思います。
この２つはここで重点的に学んでおきたい。

### キーで値を取る`[]`の補足

ツアーの説明では`[]`にキーの値を入れると良いと書いてあって、[JS入門の第六回の辞書](https://karino2.github.io/js-introduction/ch06.html)などとほとんど同じ内容に見えますが、
ちょっと落とし穴があります。

それは辞書の`[]`はnullableだという事です。
nullableに関してはツアーの最後のNullセーフティでやるので、この時点では、`[]`の後ろには`!!`をつける、とおぼえておください。
例を挙げます。

{% capture map-access %}
fun main() {

  val nedan = mapOf("りんご" to 340.0, "みかん" to 390.0, "ジャイアントコーン" to 140.0)

  val goukei = nedan["りんご"]!! + nedan["みかん"]!! + nedan["ジャイアントコーン"]!!
  println(goukei)
}
{% endcapture %}
{% include kotlin_quote.html body=map-access %}

