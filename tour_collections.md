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

なお、演算子というのはほとんどの場合無くても全く同じ意味になります。
例えば「`in`演算子は要素があるかをチェックします」と「`in`は要素があるかをチェックします」は全く同じ意味です。
なので、良く分からない時は「演算子」という言葉を消してしまってもだいたいはOKです。
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

あと、解説には無いけれどセットもfor文で順番に要素を処理出来ます。

使い方が難しいけれど、思いついた例題を一つやってみます。

**課題0： 項目の一覧を作れ**

以下のように、食事に使った場所と値段の書いたリストがあります。

```kotlin
val content = """
松屋, 890
日高屋, 570
セブン, 1020
松屋, 480
サイゼリア, 580
すた丼, 680
ガスト, 530
バーミヤン, 980
松屋, 530
日高屋, 570
サイゼリア, 780
"""
```

これから、食事に作った場所の一覧を作れ。
ただし、例えば松屋に何度か行っているけれど、一覧の中には一回だけ出るようにする事。

こういう時にセットを使います。

なお、[テキスト処理入門](text_op_intro.md)のtrimなども使います。

{% capture map-keys-q0 %}
fun main() {

val content = """
松屋, 890
日高屋, 570
セブン, 1020
松屋, 480
サイゼリア, 580
すた丼, 680
ガスト, 530
バーミヤン, 980
松屋, 530
日高屋, 570
サイゼリア, 780
"""

  // TODO: 以下にfor文などを書いて、tableを求めよ
  val table = setOf<String>()


  // 以下は書き換えない
  println("ちゃんとサイズは7つ？ ${table.size == 7}")
  println("---")
  
  for(name in table) {
    println(name)
  }
}
{% endcapture %}
{% include kotlin_quote.html body=map-keys-q0 %}

{% capture map-keys-q0-hint %}
何も考えずにセットにaddしていけば、同じものをaddしても無視されるので一覧が出来る。

テキスト処理としては
```kotlin
  val trim = content.trim('\n')
  val lines = trim.split("\n")
```
としてから、linesをfor文で回す。さらに中で","でsplitする。
{% endcapture %}
{% include collapse_quote.html body=map-keys-q0-hint title="ヒント" %}


{% capture map-keys-q0-a %}
```kotlin
fun main() {

val content = """
松屋, 890
日高屋, 570
セブン, 1020
松屋, 480
サイゼリア, 580
すた丼, 680
ガスト, 530
バーミヤン, 980
松屋, 530
日高屋, 570
サイゼリア, 780
"""

  // TODO: 以下にfor文などを書いて、tableを求めよ
  val table = mutableSetOf<String>()
  val trim = content.trim('\n')
  val lines = trim.split("\n")
  for(line in lines) {
      val arr = line.split(",")
      table.add(arr[0])
  }

  // 以下は書き換えない
  println("ちゃんとサイズは7つ？ ${table.size == 7}")
  println("---")

  for(name in table) {
    println(name)
  }
}
```
{% endcapture %}
{% include collapse_quote.html body=map-keys-q0-a title="解答例" %}



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

### keysとvaluesとfor文

マップには`keys`というプロパティと`values`というプロパティがあります。
プロパティは詳細は[クラス入門](tour_class_intro.md)でやりますが、
とりあえずマップの中の変数の事です。

- `keys`はそのマップに入っているキーをすべてセットとして返します。
- `values`はそのマップに入っている値をすべてなんらかのコレクションとして返します（リストのようなものと思ってOKなんだけど実際はリストじゃないのでこんな変な言い方になる）

これらは新たなコレクションだ、というのがポイントです。
つまりfor文で回せます。

{% capture map-keys %}
fun main() {

  val nedan = mapOf("りんご" to 120.0, "みかん" to 50.0, "ジャイアントコーン" to 140.0)

  // キーで回す
  for(hinmoku in nedan.keys) {
    println("品目： $hinmoku")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=map-keys %}

値の方も同様に回せます。

{% capture map-values %}
fun main() {

  val nedan = mapOf("りんご" to 120.0, "みかん" to 50.0, "ジャイアントコーン" to 140.0)

  // 値で回す
  for(kakaku in nedan.values) {
    println("値段： $kakaku")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=map-values %}

値の方で回すと、それに対応するキーが分からないのであまり使いません。
キーで回せば、そのキーに対応する値は`[]!!`で取れるので、だいたいはキーで回します。

少し練習問題をやってみましょう。

**課題1: 以下の買い物をした時の合計金額を求めよ**

- りんご: 120円を4個
- みかん: 50円を10個
- ジャイアントコーン: 140円を3個

それぞれマップに入れておくので、それを使って合計金額を計算してください。

なお、値段のマップには豚バラが入っているが、これは購入してない（個数の方に入っていない）のに注目してください。
実際はこのnedanの方のマップはそのスーパーの全品目が入っていたりするイメージです。

{% capture map-keys-q1 %}
fun main() {

  val nedan = mapOf("りんご" to 120, "みかん" to 50, "ジャイアントコーン" to 140, "豚バラ" to 390)
  val kosuu = mapOf("りんご" to 4, "みかん" to 10, "ジャイアントコーン" to 3)

  // TODO: 以下にfor文などを書いて、goukeiを求めよ
  val goukei = 0

  println("全部で${goukei}円です")
}
{% endcapture %}
{% include kotlin_quote.html body=map-keys-q1 %}

{% capture map-keys-q1-hint %}
買った物の方のキーでfor文を回す。要素へのアクセスは`kosuu[key]!!`のようにビックリマーク２つをつけるのを忘れないように。
{% endcapture %}
{% include collapse_quote.html body=map-keys-q1-hint title="ヒント" %}


{% capture map-keys-q1-a %}
```kotlin
fun main() {

  val nedan = mapOf("りんご" to 120, "みかん" to 50, "ジャイアントコーン" to 140, "豚バラ" to 390)
  val kosuu = mapOf("りんご" to 4, "みかん" to 10, "ジャイアントコーン" to 3)

  // TODO: 以下にfor文などを書いて、goukeiを求めよ  
  var goukei = 0
  for(hinmoku in kosuu.keys) {
    goukei += nedan[hinmoku]!!*kosuu[hinmoku]!!
  }

  println("全部で${goukei}円です")
}
```
{% endcapture %}
{% include collapse_quote.html body=map-keys-q1-a title="解答例" %}


**課題2: 8月の体重の平均を求めよ**

日付は[日付を扱う、Date入門](date_intro.md)を見直しましょう。
以下のtaijuuマップのうち、8月の体重の平均を求めてください。

なお、答えは63.375kgになるはずです。

まずはtaijuuのkeysでfor文を回し、その値と`.month`の値をprintlnする所から始めるといいでしょう。
英語で出力される場合の月の名前を以下に書いておきます。

- Jul: 7月
- Aug: 8月
- Sep: 9月

{% capture map-keys-q2 %}
import java.util.Date

fun main() {
  val taijuu = mapOf(
    Date(1689622309473) to 62.7,
    Date(1691622309473) to 63.1,
    Date(1692122309473) to 63.8,
    Date(1692622309473) to 62.4,
    Date(1693122309473) to 64.2,
    Date(1693622309473) to 65.3
  )

  // TODO: 以下にfor文などを書いて、heikinを求めよ
  val heikin = 0.0

  println("8月の体重の平均は${heikin}kgです")
}
{% endcapture %}
{% include kotlin_quote.html body=map-keys-q2 %}

{% capture map-keys-q2-hint %}
まずは最初に出したヒントの通り
```kotlin
for(dt in taijuu.keys) {
  println(dt)
  println(dt.month)
}
```
としてみる。
月が0始まりなのに注意。（8月は7になる）

次に平均を求めるためには、エントリの数と合計の２つが必要になる。
とりあえずgoukeiとkosuuとかいう名前の変数にするといいでしょう。
if文で8月だったらkosuuの方に1を足してgoukeiの方に体重を足す。
{% endcapture %}
{% include collapse_quote.html body=map-keys-q2-hint title="ヒント" %}


{% capture map-keys-q2-a %}
```kotlin
import java.util.Date

fun main() {
  val taijuu = mapOf(
    Date(1689622309473) to 62.7,
    Date(1691622309473) to 63.1,
    Date(1692122309473) to 63.8,
    Date(1692622309473) to 62.4,
    Date(1693122309473) to 64.2,
    Date(1693622309473) to 65.3
  )

  // TODO: 以下にfor文などを書いて、heikinを求めよ
  var kosuu = 0
  var goukei = 0.0
  for(dt in taijuu.keys) {
    if(dt.month == 7) {
      kosuu += 1
      goukei += taijuu[dt]!!
    }
  }
  val heikin = goukei/kosuu

  println("8月の体重の平均は${heikin}kgです")
}
```
{% endcapture %}
{% include collapse_quote.html body=map-keys-q2-a title="解答例" %}

**課題3: 課題1と同じ内容を、自分でマップを書く所から書け**

自分で書いてみる方が覚えるので、自分で同じ内容を書いてみましょう。

値段は以下です。

- りんご: 120円
- みかん: 50円
- ジャイアントコーン: 140円
- 豚バラ: 390円

買った個数は以下です。

- りんご: 4個
- みかん: 10個
- ジャイアントコーン: 3個

値段をnedan、個数をkosuuというマップで作り、買った金額の合計を求めましょう。


{% capture map-keys-q3 %}
fun main() {

  // TODO: ここでマップを作る

  // TODO: 以下にfor文などを書いて、goukeiを求めよ
  val goukei = 0

  println("全部で${goukei}円です")
}
{% endcapture %}
{% include kotlin_quote.html body=map-keys-q3 %}

{% capture map-keys-q3-a %}
```kotlin
fun main() {

  // TODO: ここでマップを作る
  val nedan = mapOf("りんご" to 120, "みかん" to 50, "ジャイアントコーン" to 140, "豚バラ" to 390)
  val kosuu = mapOf("りんご" to 4, "みかん" to 10, "ジャイアントコーン" to 3)

  // TODO: 以下にfor文などを書いて、goukeiを求めよ  
  var goukei = 0
  for(hinmoku in kosuu.keys) {
    goukei += nedan[hinmoku]!!*kosuu[hinmoku]!!
  }

  println("全部で${goukei}円です")
}
```
{% endcapture %}
{% include collapse_quote.html body=map-keys-q3-a title="解答例" %}

### マップにあるキーが含まれているかのチェック

マップにあるキーが含まれているかは、`containsKey()`でチェック出来ます。
また、`keys`がセットを返すので、セットにあるかどうかを`in`でチェックしても同じ事が出来ます。

つまり、「`containsKey()`」と「`keys`に`in`を使う」は、同じ動作となります。

"豚バラ"を買ったかどうかを以下のようにチェック出来ます。

{% capture map-containsKeys %}
fun main() {

  val kosuu = mapOf("りんご" to 4, "みかん" to 10, "ジャイアントコーン" to 3)

  if(kosuu.containsKey("豚バラ")) {
    println("豚バラを買いました。")
  } else {
    println("豚バラを買ってません。")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=map-containsKeys %}

同じ事を`keys`に`in`を使って書いてみます。

{% capture map-keys-in %}
fun main() {

  val kosuu = mapOf("りんご" to 4, "みかん" to 10, "ジャイアントコーン" to 3)

  if("豚バラ" in kosuu.keys) {
    println("豚バラを買いました。")
  } else {
    println("豚バラを買ってません。")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=map-keys-in %}

少し練習問題をやってみましょう。

**課題4: 買ってないものの一覧が入ったリストを作れ**

kosuuに買ったものの個数が入っています。
nedanにある品目のうち、買ってないものの一覧をkaiwasureリストにすべて入れよ。

{% capture map-keys-q4 %}
fun main() {

  val nedan = mapOf("プロテインバー" to 178, "りんご" to 120, "みかん" to 50, "ジャイアントコーン" to 140, "豚バラ" to 390)
  val kosuu = mapOf("りんご" to 4, "みかん" to 10, "ジャイアントコーン" to 3)

  // TODO: 以下にfor文などを書いて、kaiwasureに買ってないものを入れよ。kaiwasureの型は変えてもOK
  val kaiwasure = listOf<String>()

  println(kaiwasure)
}
{% endcapture %}
{% include kotlin_quote.html body=map-keys-q4 %}

{% capture map-keys-q4-hint %}
nedanのkeysで回してcontainsKeyを使う。含まれていない、なので`!`を使う。
要素を追加していくので、listOfではダメでmutableListOfに直さないといけない。
{% endcapture %}
{% include collapse_quote.html body=map-keys-q4-hint title="ヒント" %}


{% capture map-keys-q4-a %}
```kotlin
fun main() {

  val nedan = mapOf("プロテインバー" to 178, "りんご" to 120, "みかん" to 50, "ジャイアントコーン" to 140, "豚バラ" to 390)
  val kosuu = mapOf("りんご" to 4, "みかん" to 10, "ジャイアントコーン" to 3)

  // TODO: 以下にfor文などを書いて、kaiwasureに買ってないものを入れよ
  val kaiwasure = mutableListOf<String>()
  for(hinmoku in nedan.keys) {
    if(!kosuu.containsKey(hinmoku)) {
      kaiwasure.add(hinmoku)
    }
  }

  println(kaiwasure)
}
```
{% endcapture %}
{% include collapse_quote.html body=map-keys-q4-a title="解答例" %}

**課題5: 月ごとの体重のエントリの個数を求めよ**

taijuuに入っているデータの、月ごとの体重の平均を入れたマップ、heikinマップを作りたい。
taijuuは以下。

```kotlin
  val taijuu = mapOf(
    Date(1689122309473) to 62.1,
    Date(1689622309473) to 62.7,
    Date(1691622309473) to 63.1,
    Date(1692122309473) to 63.8,
    Date(1692622309473) to 62.4,
    Date(1693122309473) to 64.2,
    Date(1693622309473) to 65.3,
    Date(1693822309473) to 64.8
  )
```

ちなみにたぶん答えは以下みたいになる。

```
7月の平均体順は62.400000000000006kgです
8月の平均体順は63.375kgです
9月の平均体順は65.05kgです
```

ちょっと難しいので、小問に分けて進めます。

一応最終的に解きたい問題をここに書いておきますが、まずは5-1, 5-2等を進めてみてください。
いやいや、こんくらい余裕だが？というならここで解いてしまってもいいです。
（答えは5-3にあります）

{% capture map-keys-q5-0 %}
import java.util.Date

fun main() {
  val taijuu = mapOf(
    Date(1689122309473) to 62.1,
    Date(1689622309473) to 62.7,
    Date(1691622309473) to 63.1,
    Date(1692122309473) to 63.8,
    Date(1692622309473) to 62.4,
    Date(1693122309473) to 64.2,
    Date(1693622309473) to 65.3,
    Date(1693822309473) to 64.8
  )

  // TODO: 以下にfor文などを書いて、heikinを求めよ
  val heikin = mapOf(0 to 62.1)


  // 以下は書き換えない
  for(month in heikin.keys) {
    println("${month+1}月の平均体順は${heikin[month]}kgです")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=map-keys-q5-0 %}


**課題5-1: 月ごとに幾つエントリがあるかを集計せよ**

まずは、7月に幾つ体重の入力があるか、8月に幾つ、9月に幾つあるか、
というのを集計しましょう。

月をキー(`month`の値そのままで、0始まりでいいです)、エントリの個数を値に持つ、kosuuマップを作りましょう。
なお、nullableにまつわる面倒な事があるので、マップに要素を追加するのはputを使うのがおすすめです。

{% capture map-keys-q5-1 %}
import java.util.Date

fun main() {
  val taijuu = mapOf(
    Date(1689122309473) to 62.1,
    Date(1689622309473) to 62.7,
    Date(1691622309473) to 63.1,
    Date(1692122309473) to 63.8,
    Date(1692622309473) to 62.4,
    Date(1693122309473) to 64.2,
    Date(1693622309473) to 65.3,
    Date(1693822309473) to 64.8
  )

  // TODO: 以下にfor文などを書いて、kosuuを求めよ
  val kosuu = mapOf(0 to 0)


  // 以下は書き換えない
  println(kosuu)
}
{% endcapture %}
{% include kotlin_quote.html body=map-keys-q5-1 %}

{% capture map-keys-q5-1-hint %}
taijuuのkeysで回す。
型としては`mutableMapOf<Int, Int>()`などを使う。

- kosuuに既にその月があれば、kosuuのその月の値を1増やしたものをput
- 無ければ1をput

という感じになる。月があるかどうかでcontainsKeyを使う。

まずは以下みたいなコードを実行する事から始めるといいかも。
```kotlin
for(dt in taijuu.keys) {
  println(dt)
  println(dt.month)
  println(taijuu[dt]!!)
  println("---")
}
```
{% endcapture %}
{% include collapse_quote.html body=map-keys-q5-1-hint title="ヒント" %}


{% capture map-keys-q5-1-a %}
```kotlin
import java.util.Date

fun main() {
  val taijuu = mapOf(
    Date(1689122309473) to 62.1,
    Date(1689622309473) to 62.7,
    Date(1691622309473) to 63.1,
    Date(1692122309473) to 63.8,
    Date(1692622309473) to 62.4,
    Date(1693122309473) to 64.2,
    Date(1693622309473) to 65.3,
    Date(1693822309473) to 64.8
  )

  // TODO: 以下にfor文などを書いて、kosuuを求めよ
  val kosuu = mutableMapOf<Int, Int>()
  for(dt in taijuu.keys) {
      if(kosuu.containsKey(dt.month)) {
          kosuu.put(dt.month, kosuu[dt.month]!! + 1)
      } else {
          kosuu.put(dt.month, 1)
      }
  }
  
  // 以下は書き換えない
  println(kosuu)
}
```
{% endcapture %}
{% include collapse_quote.html body=map-keys-q5-1-a title="解答例" %}


**課題5-2: 月ごとの体重の合計を集計せよ**

5-1と同じ感じで、今度はgoukei というマップで体重の合計を求めます。

{% capture map-keys-q5-1 %}
import java.util.Date

fun main() {
  val taijuu = mapOf(
    Date(1689122309473) to 62.1,
    Date(1689622309473) to 62.7,
    Date(1691622309473) to 63.1,
    Date(1692122309473) to 63.8,
    Date(1692622309473) to 62.4,
    Date(1693122309473) to 64.2,
    Date(1693622309473) to 65.3,
    Date(1693822309473) to 64.8
  )

  // TODO: 以下にfor文などを書いて、goukeiを求めよ
  val goukei = mapOf(0 to 0.0)


  // 以下は書き換えない
  println(goukei)
}
{% endcapture %}
{% include kotlin_quote.html body=map-keys-q5-1 %}

{% capture map-keys-q5-2-hint %}
今回もtaijuuのkeysで回す。
体重の値は、forの変数名をdtとすると`taijuu[dt]!!`で取れる。

今回も、以下のコードを実行する事から始めるといいかも。
```kotlin
for(dt in taijuu.keys) {
  println(dt)
  println(dt.month)
  println(taijuu[dt]!!)
  println("---")
}
```
{% endcapture %}
{% include collapse_quote.html body=map-keys-q5-2-hint title="ヒント" %}


{% capture map-keys-q5-2-a %}
```kotlin
import java.util.Date

fun main() {
  val taijuu = mapOf(
    Date(1689122309473) to 62.1,
    Date(1689622309473) to 62.7,
    Date(1691622309473) to 63.1,
    Date(1692122309473) to 63.8,
    Date(1692622309473) to 62.4,
    Date(1693122309473) to 64.2,
    Date(1693622309473) to 65.3,
    Date(1693822309473) to 64.8
  )

  // TODO: 以下にfor文などを書いて、kosuuを求めよ
  val goukei = mutableMapOf<Int, Double>()
  for(dt in taijuu.keys) {
      if(goukei.containsKey(dt.month)) {
          goukei.put(dt.month, goukei[dt.month]!! + taijuu[dt]!!)
      } else {
          goukei.put(dt.month,  taijuu[dt]!!)
      }
  }
  
  // 以下は書き換えない
  println(goukei)
}
```
{% endcapture %}
{% include collapse_quote.html body=map-keys-q5-2-a title="解答例" %}

**課題5-3: 月ごとの平均を求めよ**

ここまで求めたkosuuとgoukeiを使って、月ごとの平均のマップ、heikinを作れ。

{% capture map-keys-q5-3 %}
import java.util.Date

fun main() {
  val taijuu = mapOf(
    Date(1689122309473) to 62.1,
    Date(1689622309473) to 62.7,
    Date(1691622309473) to 63.1,
    Date(1692122309473) to 63.8,
    Date(1692622309473) to 62.4,
    Date(1693122309473) to 64.2,
    Date(1693622309473) to 65.3,
    Date(1693822309473) to 64.8
  )

  // TODO: 以下にfor文などを書いて、heikinを求めよ
  val heikin = mapOf(0 to 62.1)


  // 以下は書き換えない
  for(month in heikin.keys) {
    println("${month+1}月の平均体順は${heikin[month]}kgです")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=map-keys-q5-3 %}


{% capture map-keys-q5-3-hint %}
まずはkosuuとgoukeiのマップを求めて、printlnで正しく作れているか確認しよう。
次にkosuuのkeysで回してheikinマップにputする。
{% endcapture %}
{% include collapse_quote.html body=map-keys-q5-3-hint title="ヒント" %}



{% capture map-keys-q5-3-a %}
```kotlin
import java.util.Date

fun main() {
  val taijuu = mapOf(
    Date(1689122309473) to 62.1,
    Date(1689622309473) to 62.7,
    Date(1691622309473) to 63.1,
    Date(1692122309473) to 63.8,
    Date(1692622309473) to 62.4,
    Date(1693122309473) to 64.2,
    Date(1693622309473) to 65.3,
    Date(1693822309473) to 64.8
  )

  // TODO: 以下にfor文などを書いて、heikinを求めよ
  val goukei = mutableMapOf<Int, Double>()
  val kosuu = mutableMapOf<Int, Int>()
  val heikin = mutableMapOf<Int, Double>()
  for(dt in taijuu.keys) {
      if(goukei.containsKey(dt.month)) {
          kosuu.put(dt.month, kosuu[dt.month]!! + 1)
          goukei.put(dt.month, goukei[dt.month]!! + taijuu[dt]!!)
      } else {
          kosuu.put(dt.month, 1)
          goukei.put(dt.month, taijuu[dt]!!)
      }
  }
  
  for(m in kosuu.keys) {
      heikin.put(m, goukei[m]!!/kosuu[m]!!)
  }

  // 以下は書き換えない
  for(month in heikin.keys) {
    println("${month+1}月の平均体順は${heikin[month]}kgです")
  }
}
```
{% endcapture %}
{% include collapse_quote.html body=map-keys-q5-3-a title="解答例" %}

**課題5-4: 以上をdataクラスを使ってもう一度解こう**

5-3はgoukeiとkosuuのマップを２つ使うが、これは一つにまとめられそうだ。
[data class入門](dataclass_intro.md)を使ってまとめてみる。

以下のような合計エントリ、GoukeiEntryを持つマップを作って、

```kotlin
data class GoukeiEntry(val kosuu: Int, val goukei: Double)
```

これを使って平均を求めよう。

{% capture map-keys-q5-4 %}
import java.util.Date

data class GoukeiEntry(val kosuu: Int, val goukei: Double)

fun main() {
  val taijuu = mapOf(
    Date(1689122309473) to 62.1,
    Date(1689622309473) to 62.7,
    Date(1691622309473) to 63.1,
    Date(1692122309473) to 63.8,
    Date(1692622309473) to 62.4,
    Date(1693122309473) to 64.2,
    Date(1693622309473) to 65.3,
    Date(1693822309473) to 64.8
  )

  // TODO: 以下にfor文などを書いて、goukeiを求めよ
  val goukei = mapOf<Int, GoukeiEntry>()


  // 動作確認の為printlnしておく。
  println(goukei)

  // TODO: 以下にfor文などを用いてheikinを求めよ
  val heikin = mapOf<Int, Double>()

  // 以下は書き換えない
  for(month in heikin.keys) {
    println("${month+1}月の平均体順は${heikin[month]}kgです")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=map-keys-q5-4 %}

{% capture map-keys-q5-4-hint %}
既にあるエントリを更新する方は変数を使うと楽だろう。

```kotlin
  val oldGoukei = goukei[dt.month]!!
  val newGoukei = GoukeiEntry(oldGoukei.kosuu+1, oldGoukei.goukei+taijuu[dt]!!)
```

のように作ってからputする。
{% endcapture %}
{% include collapse_quote.html body=map-keys-q5-4-hint title="ヒント" %}

{% capture map-keys-q5-4-a %}
```kotlin
import java.util.Date

fun main() {
  val taijuu = mapOf(
    Date(1689122309473) to 62.1,
    Date(1689622309473) to 62.7,
    Date(1691622309473) to 63.1,
    Date(1692122309473) to 63.8,
    Date(1692622309473) to 62.4,
    Date(1693122309473) to 64.2,
    Date(1693622309473) to 65.3,
    Date(1693822309473) to 64.8
  )

  // TODO: 以下にfor文などを書いて、goukeiを求めよ
  val goukei = mutableMapOf<Int, GoukeiEntry>()
  for(dt in taijuu.keys) {
      if(goukei.containsKey(dt.month)) {
          val oldGoukei = goukei[dt.month]!!
          val newGoukei = GoukeiEntry(oldGoukei.kosuu+1, oldGoukei.goukei+taijuu[dt]!!)
          goukei.put(dt.month, newGoukei)
      } else {
          goukei.put(dt.month, GoukeiEntry(1, taijuu[dt]!!))          
      }
  }
  
  // 動作確認の為printlnしておく。
  println(goukei)

  // TODO: 以下にfor文などを用いてheikinを求めよ
  val heikin = mutableMapOf<Int, Double>()
  for(m in goukei.keys) {
      val entry = goukei[m]!!
      heikin.put(m, entry.goukei/entry.kosuu)
  }

  // 以下は書き換えない
  for(month in heikin.keys) {
    println("${month+1}月の平均体順は${heikin[month]}kgです")
  }
}
```
{% endcapture %}
{% include collapse_quote.html body=map-keys-q5-4-a title="解答例" %}
