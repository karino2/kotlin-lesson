---
title: "ListViewに挑む！編集編"
layout: page
---
アイテムを追加する。
表示編の続きでいいです。

## EditTextとボタン二つ置く

ボタンは一つはSubmit、一つはクリアとする。

## メンバ変数のlistをMutableListに変更

listOfをmutableListOfにする

## Submitのonclickの処理を書く

mutable listにedit textの内容をaddしてedit textはクリア、
さらにadapterの`notifyDataSetChanged()`を呼ぶ

## クリアのonclickの処理を書く

mutable listのclearを呼び出して、adapterの`notifyDataSetChanged()`を呼ぶ