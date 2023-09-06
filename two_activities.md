---
title: "複数アクティビティで「てきすとでっき」もどき"
layout: page
---
ここからは実践編になります。
実際にアプリを作りつつコードの書き溜めを始めます。

まずはてきすとでっきのようなアプリを作ります。

[てきすとでっき - Google Play のアプリ](https://play.google.com/store/apps/details?id=io.github.karino2.textdeck&hl=ja)

ファイル選択の所はしばらくやらない予定なので、それ以外で現時点で作れ無さそうな部分はどこでしょうか？

1. Newやアイテムをクリックすると別の画面に行く奴
2. 上の所のメニュー

この二つくらいでしょう。

そこでまずは1の、複数画面を行ったり来たりする所をやります。
この画面をActivityと呼びます。

## 二つのアクティビティを行ったり来たりしてみる

てきすとでっきもどきからは少し離れて、もっと単純に二つのActivityを行ったり来たりするアプリ、
HelloTwoActivityを作ってみましょう。

### いつも通り1つめのActivityを作る

いつも通りNew ProjectでHelloTwoActivityというのを作り、
いつも通りEditTextとButtonを画面に置きます。idは適当に決めてください。

とりあえずボタンが押されたらEditTextの内容をshowMessageするようにしてみましょう。

### 2つめのActivityを作る

左側のappとかjavaを右クリックしてNewから、真ん中よりちょい下のActivityを選び、

![New Activityのメニューのスクリーンショット](imgs/new_activity.png)

その中の「Empty Views Activity」を選びます。（なんか聞いた事ある名前ですね）

