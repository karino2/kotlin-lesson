---
title: "ListViewに挑む！編集編"
layout: page
---
アイテムを追加する。
表示編の続きでいいです。

## EditTextとボタン二つとListViewを置く

ボタンは一つはSubmit、一つはクリアとする。ボタン二つは横に並べますか。

## メンバ変数のlistをMutableListで定義

listOfのままだったらMutableListに変更してください。リストは空でもいいですし、テスト用に最初に2〜3個何か入れておいてもいいです。

## Submitのonclickの処理を書く

mutable listにedit textの内容をaddしてedit textはクリア、
さらにadapterの`notifyDataSetChanged()`を呼ぶ

## クリアのonclickの処理を書く

mutable listのclearを呼び出して、adapterの`notifyDataSetChanged()`を呼ぶ

## 課題: 「削除」ボタンもつけよう

アイテムごとの方のlayoutに「削除」というボタンを追加し、それが押されたらそのアイテムが削除されるようにしよう。

具体的にはlistDataからその要素をremoveAtで削除してnotifyDataSetChangedを呼べば良い。