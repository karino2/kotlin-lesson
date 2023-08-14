---
title: "data class入門"
layout: page
---
今回はdata classの話をします。
クラスの話はもうちょっと後にするつもりなのだけど、とりあえずdata classは使いたいのでdata classの簡単な使い方を先に。

[Data classes - Kotlin Documentation](https://kotlinlang.org/docs/data-classes.html)

## data class入門

data classは、複数のデータをまとめて持つのに使う機能です。
割と単純なので特に難しい事も無いのですが、実際に使う所を見ないと「なんでそんな機能あるの？」って思って終わりだと思うので、
ここまで登場しませんでした。

でも今回、ListViewの編集とDateの説明が終わったので、ようやく具体的な使い道をもとに説明出来ます。

という事で、まずは使い道から見ていきましょう。

### ListViewに挑む！編集編に、投稿時間もつけたい

data classを使いたくなるシチュエーションとして、[ListViewの編集編](listview_edit.md)で作ったものに対し、
アイテム追加時の時間も表示したい、と考えたとします。

けれど、リストのアイテムは現状、String型となっている。

```kotlin
val listData = mutableListOf<String>()
```

ここに、StringとDateの二つを追加していきたい。そういう時に使うのがdata classです。

data classは、リストに二つ以上の項目を同時に追加していきたい時に使います。
以下、それをどうやって実現するのかを見ていきましょう。

### data classの簡単な例

まずdata classの例を見ていきたいと思います。

data class は以下のように使います。

{% capture dataclass_intro1 %}
data class TwoInt(val v1: Int, val v2: Int)

fun main() {
  val ti1 = TwoInt(1, 2)

  println(ti1.v1)
  println(ti1.v2)
}
{% endcapture %}
{% include kotlin_quote.html body=dataclass_intro1 %}

以下、細かく見ていきましょう。

### data classの文法

まずdata classは最初に`data class クラス名`で始まります。
クラス名は大文字始まりで単語の区切り大文字という慣習になっています。
つまりTwoIntとかUserDataとかそういうものになります。

そして`クラス名`の後にカッコが続き、変数定義のようなものが、型付きで並びます。

代入する場合はvar、代入しない場合はvalですが、あまりvarを使う事は無いのでvalとおぼえてしまっても良いでしょう。

こうしてdata classが定義出来ます。data classは、新しい型となります。この場合はTwoInt型が出来る事になります。

次に使い方です。data classは定義すると、そのクラス名を関数のように使う事が出来て、
そのreturnはそのdata classのデータとなります。先ほどの例では`TwoInt`というのを関数のように使う事が出来て、
`TwoInt(1, 2)`とすると、TwoInt型のデータが作れる訳です。

### 各構成要素の触り方はドットと変数名

さて、`val ti1 = TwoInt(1, 2)` とti1変数にデータを入れられました。
この1とか2にはどうアクセスするか？というと、`ti1.v1`とか`ti1.v2`というように、
ドットとdata classを定義した時の変数名でアクセスします。

これで基本的な使い方は終わりです。
ちょっと幾つか例とか練習問題とかを見ていきましょう。

### data classを使った例いろいろ

幾つか例を見ていきましょう。さきほどはどっちもIntだったので、次はStringとIntの例を。
ユーザー名とスコアを持たせる、というのは割と良くあります。

{% capture dataclass_intro2 %}
data class User(val name: String, val score: Int)

fun main() {
  val user1 = User("ほげいか", 95)
  val user2 = User("karino2", 72)

  println("${user1.name}のスコアは${user1.score}点です")
  println("${user2.name}のスコアは${user2.score}点です")
}
{% endcapture %}
{% include kotlin_quote.html body=dataclass_intro2 %}

また、これをListに入れる事もできる。

{% capture dataclass_intro3 %}
data class User(val name: String, val score: Int)

fun main() {
  val mlist = mutableListOf<User>()

  mlist.add(User("ほげいか", 95))
  mlist.add(User("karino2", 72))

  for(user in mlist) {
    println("${user.name}のスコアは${user.score}点です")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=dataclass_intro3 %}

次に「ListViewに挑む！編集編」で、入力されたテキストが追加された時間をDateで保存するような場合を考える。
Dateなのでimport文を追加して、以下のような感じになる。

{% capture dataclass_intro4 %}
import java.util.Date

data class Post(val item: String, val date: Date)

fun main() {
  val mlist = mutableListOf<Post>()

  mlist.add(Post("これは一番目の項目です", Date()))
  mlist.add(Post("これは二番目の項目です", Date()))

  for(post in mlist) {
    println("「${post.item}」は${post.date}に追加されました")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=dataclass_intro4 %}

では自分でも幾つか作ってみましょう。

**課題: ユーザー名と投稿数を持つ、SNSUserを作れ**

ユーザー名はname, 投稿数はpostNumとしましょう。

{% capture dataclass_intro_q1 %}

// TODO: ここにSNSUserを作る

fun main() {
  val user1 = SNSUser("karino2", 920)
  val user2 = SNSUser("ほげいか", 312)

  println(user1.name)
  println(user1.postNum)

  println(user2.name)
  println(user2.postNum)
}
{% endcapture %}
{% include kotlin_quote.html body=dataclass_intro_q1 %}

**課題: 名前と攻撃力と防御力を持つCharacterを作れ**

名前はname, 攻撃力はattack、防御力はdefenseとしますか。

{% capture dataclass_intro_q2 %}

// TODO: ここにCharacterを作る

fun main() {
  val chara1 = Character("メタルスライム", 10, 999)
  val chara2 = Character("紙装甲", 999, 20)

  for(chara in listOf(chara1, chara2)) {
    println("${chara.name} の攻撃力は${chara.attack}、防御力は${chara.defense}です")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=dataclass_intro_q2 %}

**課題: アイテムをたくさん持つアイテムボックスのdata classを作れ**

アイテムボックスは名前がnameであって、Stringのアイテムをたくさん持つとします。とりあす`List<String>`でitemsという名前でいいでしょう。

{% capture dataclass_intro_q3 %}

// TODO: ここにItemBoxを作る

fun main() {
  val box1 = ItemBox("ドラクエのアイテムボックス", listOf("薬草", "毒消し草", "布の服", "ひのきの棒"))
  val box2 = ItemBox("FFのアイテムボックス", listOf("ポーション", "エーテル", "ミスリルソード"))

  for(box in listOf(box1, box2)) {
    println("${box.name}：")
    for(item in box.items) {
      println("  ${item}")
    }
    println("")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=dataclass_intro_q3 %}

**課題: 以下をtrueになるように変更せよ**

要素にアクセスする方もやってみます。

{% capture dataclass_intro_q4 %}

data class User(val name: String, val postNum: Int)

fun main() {
  val user1 = User("karino2", 920)
  val user2 = User("ほげいか", 312)

  // TODO: 以下の==の左側を変更してtrueになるようにせよ
  println(user1 == "karino2")
  println(user1 == 920)

  println(user2 == "ほげいか")
  println(user2 == 312)
}
{% endcapture %}
{% include kotlin_quote.html body=dataclass_intro_q4 %}

## ListViewに挑む！編集編に、投稿時間をつけよう

という事でdata classについての簡単な説明をしたので、ついにListViewの編集に、アイテム追加時の時間を追加する事にしましょう。
結構やる事は多いので、最初にやる事をリストアップしておきます。

1. list itemの方のレイアウトに、時刻表示用TextViewと追加
2. Postというdata classを作り、項目名とDateをもたせる
3. listDataをPostのMutableListにする
4. getViewで時刻表示用TextViewに時刻をセット
5. アイテムを追加するボタンのsetOnClickListenerで、Postを追加するようにして、その時点でのDate()を持たせる

以下各ステップを見ていきましょう。

### 1. list itemの方のレイアウトに、時刻表示用TextViewと追加

時刻表示用のTextViewを追加します。少しこちらのTextViewは文字のサイズを小さくして、これまでのitemLabelの文字を大きくしておきましょう。
idはitemDateとかにしますか。

### 2. Postというdata classを作り、項目名とDateをもたせる

Postというdata classで、StringのitemとDate型のdateを持たせるとしましょう。
このPostというdata classは、[コードの置き場所入門](code_location_intro.md)の「1番目の区画」に置きます。

ついに1番目の区画にコードを書く時が来ましたね。

ここでDateの所が赤くなったら、上にカーソルを移動して、Alt+Enterでimportを追加しましょう。

### 3. listDataをPostのMutableListにする

これは特に解説の必要は無いですかね。

### 4. getViewで時刻表示用TextViewに時刻をセット

itemLabelにセットしているのと同様に、itemDateにも日付をセットします。toStringでいいでしょう。

日付は、以下のような感じでセットします。

```kotlin
val data = listData[position]

view.findViewById<TextView>(R.id.itemDate).text = data.date.toString()
```

dataとかdateとかややこしいですね。dataの方の変数名はそろそろ変えた方がいいかもしれない。

### 5. アイテムを追加するボタンのsetOnClickListenerで、Postを追加するようにして、その時点でのDate()を持たせる

setOnClickListenerの中で、以下みたいな感じに書きます。

```kotlin
val text = findViewById(R.id.edit1).text.toString()
val post = Post(text, Date())
listData.add(post)
```

変数使わなくてもいいですが、このくらいは使った方が読みやすいとは思う。

## おまけ：　クラスの用語いろいろ

このシリーズはAndroid側とkotlin言語側の話が交互に続いていく感じになっていますが、
Android側の最難関はおそらくListViewです。

一方kotlin側の最難関はクラスとなります。

クラスは何が難しいって用語がめちゃくちゃ多い。
そこで割と単純なdata classの段階で用語を幾つか先取りしておいて、クラスを勉強する時に少し楽になるようにここで少し頑張っておきましょう。

### クラスとオブジェクト（またはインスタンス）

`data class TwoInt(val v1: Int, val v2: Int)`でTwoIntクラスという名前の型が出来ます。
そして、TwoIntクラスの型のデータをオブジェクトと呼びます。

少しややこしいですね。[型とは何か？](what_is_type.md)で少し触れたように、
型というのは、データのカテゴリでした。

ストII、ストIIダッシュ、スーパーストII、スパIIXなどがデータで、ストIIシリーズが型、というものでした。

つまり一つの型というカテゴリに対し、そのカテゴリにあてはまるデータが複数ある訳です。

そしてこのカテゴリには`Int`とか`String`とか`List<String>`とかがある訳だけれど、
今回新しく`TwoInt`というカテゴリを作れるようになった訳です。

ストIIシリーズという型にはいろんなストIIのタイトルがあるように、
TwoIntというカテゴリにも、`(1, 2)`というデータを持つTwoIntや、`(8, 3)`を持つTwoIntなど、いろんなデータが存在します。

このデータを、クラスの時は「オブジェクト」とか「インスタンス」と呼ぶ事になっているという訳です。
「オブジェクト」と「インスタンス」はほぼ同じ意味です。起き昇とリバーサル昇竜くらいの違いです。

最初のうちはややこしいかもしれないけれど、クラスを自分で作っていくようになるとこのインスタンスとクラスの区別みたいなのは何度も出てくるようになるので、
使っていくと慣れます。

という事で我らもこれから使っていく事にする。

以上をまとめると、以下のようになります。

- `data class TwoInt(val v1: Int, val v2: Int)`でTwoIntクラスが出来る
- `TwoInt(1, 2)` などと呼ぶと、TwoIntクラスのインスタンスが作れる
- `TwoInt(7, 3)` などと呼んでも、TwoIntのクラスのインスタンスは作れる
- 一つのTwoInt型からインスタンスはたくさん作れる
- TwoInt型とTwoIntクラスはほとんど同じ意味

とりあえず用語はややこしいので何度も使って慣れる方がいい。という事で以後バリバリ使っていきましょう。

### メンバ変数v1, v2

先ほどの定義を再掲します。

```kotlin
data class TwoInt(val v1: Int, val v2: Int)
```

このv1とv2をメンバ変数と呼びます。
メンバ変数というのはこれまでもActivityの所に定義している変数をそう呼んでいました。
あれも内部的には全く同じものです。
クラスを真面目にやると両者が同じものなのが分かりますが、とりあえずここではdata classのこれらの変数もメンバ変数と呼ぶ、
とだけ覚えておいてください。