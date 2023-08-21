---
title: "課題: ListViewに日付に保存"
layout: page
---
これまでやってきた以下の一連の課題、

1. [ListViewに挑戦、編集編](listview_edit.md)
2. [日付を扱う、Date入門](date_intro.md)の「課題: 「ListViewに挑む！編集編」に、投稿時間をつける」
3. [data class入門](dataclass_intro.md)の「課題： 前回の日付の入門でやった、「ListViewに挑む！編集編」に、投稿時間をつけたものをdata class化しよう」

に、さらに保存するようにしよう。
毎回ちょっとずつ更新しているので最終的に何を作りたいのかが分かりにくくなってきたので、
ここでやりたい事を記述しなおします。

練習のために1から作ってもいいでしょう。

## レイアウト

レイアウトは以下のようにする

1. 入力用EditText
2. LoadボタンとSubmitボタン
3. ListView

2はLinearLayoutのhorizontalで。

入力用EditTextにテキストを入力してSubmitとすると、入力したテキストとその時点の時刻がListViewに表示され、さらにファイルに保存されるとする。
ファイル名は"listview_save_data.txt"にしましょう。

ListViewのアイテムのレイアウトは

1. TextView
2. 削除ボタンと日付のTextView

としましょう。日付のTextViewはテキストサイズを小さめで。

## ファイル保存のためにTextFileLibを追加

[ファイルをとりあえず使うだけの入門](textfilelib_intro.md)でやったのと同様にTextFileLib.ktを追加して、その他必要な作業をやります。

## data classと保存のフォーマット

data classは以下のPostで。

```kotlin
data class Post(val content: String, val created: Date)
```

保存のフォーマットは[テキスト処理入門](text_op_intro.md)の最後の方でやっていた、以下の形式とします。

```
1691126681002,これは一行目のアイテムです
1691137849935,これは,二行目のアイテムです
1691189379291,これは三行目です。別にどんな文字列でもいいですが、このフォーマットだと改行は入れられない。
```

## ソースコードの概要

`MutableList<Post>` をメンバ変数に持ち、これのArrayAdapterを作る。

onCreateの方にLoadとSubmitのsetOnClickListenerの処理を書く。

Submitの都度"listview_save_data.txt"にメンバ変数のMutableListにPostを追加してnotifyDatasetChangedをしつつ、
このリストを文字列にしてTextFileLib.writeTextする。

Loadボタンは"listview_save_data.txt"からテキストを読んでMutableListに加え直す。
ロードの時には一旦リストをクリアするようにしましょう。

getViewの方ではTextViewにPostのcontentを表示して日付のTextViewのcreatedをtoStringしたものをセットする。
さらに削除ボタンのonClickListenerではリストからアイテムを削除してnotifyDatasetChangedする。