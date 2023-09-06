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

