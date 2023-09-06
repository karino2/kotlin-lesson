---
title: "複数アクティビティ"
layout: page
---
ここではActivityというのものと、それを行ったり来たりする方法を学びます。

## てきすとでっきのようなアプリを作りたい

まずは用途から。てきすとでっきのようなアプリを作りたい。

[てきすとでっき - Google Play のアプリ](https://play.google.com/store/apps/details?id=io.github.karino2.textdeck&hl=ja)

ファイル選択の所はしばらくやらない予定なので、それ以外で現時点で作れ無さそうな部分はどこでしょうか？

1. Newやアイテムをクリックすると別の画面に行く奴
2. 上の所のメニュー

この二つくらいでしょう。

そこでまずは1の、複数画面を行ったり来たりする所をやります。
この画面をActivityと呼びます。

## 二つのアクティビティを行ったり来たりしてみる

まずは二つのActivityを行ったり来たりするアプリ、
HelloTwoActivityを作ってみましょう。

### いつも通り1つめのActivityを作る

いつも通りNew ProjectでHelloTwoActivityというのを作り、
いつもと同じ感じでEditTextとButtonを画面に置きます。idは適当に決めてください。TextViewじゃなくてEditTextなのに注意。

とりあえずボタンが押されたらEditTextの内容をshowMessageするようにしてみましょう。

### 2つめのActivityを作る

左側のappとかjavaを右クリックしてNewから、真ん中よりちょい下のActivityを選び、

![New Activityのメニューのスクリーンショット](imgs/new_activity.png)

その中の「Empty Views Activity」を選びます。（なんか聞いた事ある名前ですね）

そしてActivity Nameは「SecondActivity」にしてください。

するとレイアウトに、これまでのactivity_main.xmlの隣に、activity_second.xmlというものができるはずです。
このactivity_second.xmlもいつものようにLinearLayoutにした後に、以下のような感じにします。

- LienarLayout vertical
  - TextView
  - LinearLayout horizontal
    - 「キャンセル」ボタン
    - 「修飾」ボタン

{% capture comment1 %}
**ボタンの指示**  
そろそろボタンは「textをキャンセルに、idをbuttonCancelにしたボタン」と言わずに、「キャンセル」のボタン、とだけ言う事にします。

「キャンセル」のボタン、とったらtextに「キャンセル」を、idにはそれと分かるもの、例えばこの場合ならbuttonCancelなどをつけてください。

「修飾」のボタン、といったら、textに「修飾」を、idにはbuttonModifyとかがいいと思いますが、英語が難しいようならローマ字でbuttonSyuusyokuとかでもいいです。
{% endcapture %}
{% include myquote.html body=comment1 %}

### MainActivityからSecondActivityに移動する

今回の一番意味の分からない所はここだと思います。

１つ目のActivityのshowMessageしていた所を以下のように変更する。

```kotlin
  val intent = Intent(this, SecondActivity::class.java)
  startActivity(intent)
```

新しいActivityを開始するのは`startAcitivity`で行いますが、このstartActivityに、「どんなActivityを始めたいか？」を指示するものをインテント、といいます。
新しいActivityの始め方は、基本的には以下の2ステップになります。

1. Intentを作る
2. 作ったインテントでstartActivityする

Intentの作り方は、とりあえずおまじないのthisと、行き先を渡して作ります。
行き先は `SecondActivity::class.java` という変な表記の仕方で、これは覚えるしかありません。

ただSecondActivityは２つ目のActivityとしてさきほど作ったActivityの名前な事は分かるでしょう。

これでボタンを押すと２つ目のActivityに移動できるようになりました。
また、戻るのジェスチャー（古いAndroidだと戻るボタン）で前のActivityに戻る事もできるはずです。

### SecondActivityから元のアクティビティに戻る

前の画面に戻るには、今のアクティビティを終了させる必要があります。

とりあえず何もせずに終了するなら`finish()`を呼びます。

SecondActivityのキャンセルボタンが押されたら`finish()`を呼んでみましょう。
これでキャンセルボタンを押したら戻れるようになったと思います。

## 2つのアクティビティの間にデータをやりとりしてみる

なんか書く

### データを送る

Intentを作るところで、`intent.putExtra("TEXT_DATA", findViewById<EditText>(R.id.edit1).text.toString())` とかして、

SecondActivityのonCreateで、以下みたいな事をする

```kotlin
if (intent != null) {
  val str = intent.getStringExra("TEXT_DATA")
  findViewById<TextView>(R.id.label1).text = str
}
```

戻りはstartActivityForResultとかsetResultの説明をする。

### データを送り戻す

startActivityを`startActivityForResult(intent, 123)`に変えて、
SecondActivityの方でbuttonModifyが押されたら

```kotlin
  val intent = Intent()
  intent.putExtra("RESULT_DATA", "修飾！ " + findViewById<TextView>(R.id.labelResult).text.toString())
  setResult(RESULT_OK, intent);
  finish()
```

して、１つ目のActivityで、

```kotlin
    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        if(requestCode == 123 && resultCode == RESULT_OK && data != null) {
            findViewById<EditText>(R.id.edit1).setText(data.getStringExtra("RESULT_DATA"))
        }
        super.onActivityResult(requestCode, resultCode, data)
    }
```

する。なんかsetTextしないとダメだったが、TextViewにするべきかなぁ。まぁどっちでもいい。

そのうち説明を真面目に書く。