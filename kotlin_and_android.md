---
title: "ここまで学んだkotlinとAndroidを組み合わせる"
layout: page
---
もうここまでやったら次の[簡単な電卓を作ってみよう](simple_calc.md)をやったらええんちゃうの？
と思ってやらせたら、「突然難しくなりすぎだろ！」とフィードバックがあったので、間に少し挟む事にする回です。

ここまで序盤でAndroid Studioを使っていろいろやり、その後JS入門したりkotlinの勉強をしてきました。

そこでここでは、その両者を組み合わせるとどういう感じか、というのをやってみたいと思います。
新しい事は出てきません。

## 課題: EditTextと文字列を連結して表示しよう

[EditText、チェックボックスなどを使ってみる](misc_view.md)とほとんど同じ内容ですが、文字列の連結を使ってみましょう。

以下のようなレイアウトを作って、

- EditText
- TextView
- Button

ボタンが押されたら、EditTextの内容の前に 「前に追加：」を、内容の後に「：後に追加」を足したものをTextViewに表示しよう。

つまり、EditTextに`ほげほげ`と入力されたら、`"前に追加：ほげほげ後に追加"`をTextViewに表示します。

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

## 課題: EditTextを二つ置いて、足し合わせよう

以下のように並べたレイアウトを作って、

- EditText
- EditText
- TextView
- Button

上二つのEditTextに入力された数字を足したものを下のTextViewに表示してみよう。
数字以外が入れられた時のことは考えなくていいです。

## 課題: EditTextを二つ置いて、連結してみよう

以下のように並べたレイアウトを作って、

- EditText
- EditText
- TextView
- Button

上二つのEditTextに入力されたテキストをつなぎあわせたものをTextViewに表示しよう。