---
title: "ここまで学んだ事を組み合わせる"
layout: page
---
もうここまでやったら次の[簡単な電卓を作ってみよう](simple_calc.md)をやったらええんちゃうの？
と思ってやらせたら、「突然難しくなりすぎだろ！」とフィードバックがあったので、間に少し挟む事にする回です。
アニメで言う所の総集編回ですね。

ここまで序盤でAndroid Studioを使っていろいろやり、その後JS入門したりkotlinの勉強をしてきました。

そこでここでは、その両者を組み合わせるとどういう感じか、というのをやってみたいと思います。
新しい事は出てきません。

## 課題： EditTextとチェックボックスの例をやりなおそう

しばらくJSとkotlin触っていると、意外と忘れているものです。
という事で、以下のページの二つの繰り返しやった奴を、もう一回ずつやりましょう。

[EditText、チェックボックスなどを使ってみる](misc_view.md)

## 課題: EditTextと文字列を連結して表示しよう

[EditText、チェックボックスなどを使ってみる](misc_view.md)とほとんど同じ内容ですが、文字列の連結を使ってみましょう。

以下のようなレイアウトを作って、

- EditText
- TextView
- Button

ボタンが押されたら、EditTextの内容の前に 「前に追加：」を、内容の後に「：後に追加」を足したものをTextViewに表示しよう。

つまり、EditTextに`ほげほげ`と入力されたら、`"前に追加：ほげほげ：後に追加"`をTextViewに表示します。

ヒント: これまではfindViewByIdしたものをそのままTextViewに入れたりしていましたが、一度変数に入れる練習をするといいかも。また、この辺からは、`.text`はtoStringするようにした方がいいかもしれません。
つまり以下のような感じ。

```kotlin
val s = findViewById<TextView>(R.id.edit1).text.toString()
val s2 = s+"ふがふが"
findViewById<TextView>(R.id.label1).text = s2
```

## 課題: EditTextとSwitchを使って文字を連結して表示しよう

以下のようなレイアウトを作って

- Switch
- EditText
- TextView
- Button

ボタンが押された時に

- スイッチがオンだったら、EditTextの前に「スイッチがオン：」を付け加えてTextViewに表示
- スイッチがオフだったら、EditTextの内容をそのままTextViewに表示

をしよう。

## 課題: Switchを二つ置いて組み合わせてみよう

以下のようなレイアウトを作って

- Switch
- Switch
- TextView
- Button

ボタンが押された時に二つのスイッチの状態に応じて、下のテキストをTextViewに表示せよ。

- "スイッチがどっちもオフ"
- "上のスイッチだけオン"
- "下のスイッチだけオン"
- "両方のスイッチがオン"

### 論理和と論理積

ここでは、ifを入れ子にして解決するのが意図した答えですが、`&&`を使ってもいいです。
`&&`で「両方ともtrueだったら」という意味に、`||`で「どちらかがtrueだったら」という意味になります。

これをそれぞれ論理和と論理積といいますが、難しい言葉なのであまり使われず、「あんどあんど」とか「おあおあ」とか言われている事が多い気がする。

{% capture code_logicaland1 %}
fun main() {
  val flag1 = true
  val flag2 = false

  if(flag1 && flag2) {
    println("どっちもtrue")
  }

  if(flag1 || flag2) {
    println("どっちかがtrue")
  }

}
{% endcapture %}
{% include kotlin_quote.html body=code_logicaland1 %}


## 課題: EditTextを二つ置いて、足し合わせよう

以下のように並べたレイアウトを作って、

- EditText
- EditText
- TextView
- Button

上二つのEditTextに入力された数字を足したものを下のTextViewに表示してみよう。
数字以外が入れられた時のことは考えなくていいです。

ここで、EditTextのtextをそのままtoInt()しようとしても出来ないはずです。
textを一旦toString()してからtoInt()する必要があります。

## 課題: EditTextを二つ置いて、連結してみよう

以下のように並べたレイアウトを作って、

- EditText
- EditText
- TextView
- Button

上二つのEditTextに入力されたテキストをつなぎあわせたものをTextViewに表示しよう。

## 課題: EditTextを二つ置いて、間に文字列を挟んで連結してみよう

ほとんど上と同じですが、 二つのEditTextのテキストの間に「：間です：」を挟もう。
つまり、１つ目に「ほげ」、二つ目に「いか」と入力されたら、`"ほげ：間です：いか"`をTextViewに表示しよう。