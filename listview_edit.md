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

具体的にはlistDataからその要素をremoveAtで削除してnotifyDataSetChangedを呼べば良い…のだけど、getViewの中からadapterを触ろうとするとエラーになります。

### adapterの型解決の問題を解決する

adapterのgetViewの中からadapterを触るとエラーになる。
これは少しややこしい事が理由なので説明は難しいのだけれど、解決策は簡単です。

こんな感じのコードがあったとして、

```kotlin
    val adapter by lazy { object: ArrayAdapter<String>(this, R.layout.list_item, listData) {
        override fun getView(position: Int, convertView: View?, parent: ViewGroup): View {
            val view = if(convertView == null) layoutInflater.inflate(R.layout.list_item, null) else convertView
            val data = listData[position]


            view.findViewById<TextView>(R.id.itemLabel).text = data
            view.findViewById<Button>(R.id.itemButton).setOnClickListener {
                listData.removeAt(position)
                adapter.notifyDataSetChanged()
            }
            return view
        }
      }
    }

```

この1行目の最初の以下の部分を、

```kotlin
    val adapter by lazy 
```

このadapterに、型をつけてやればよい。型は`ArrayAdapter<String>`です。つまり、以下のよううに、adapterの後にコロンで`ArrayAdapter<String>`を足せば良い。

```kotlin
    val adapter : ArrayAdapter<String> by lazy 
```

これで動きます。

これはかなりややこしい話なので、「getViewでadapterを使いたい時はこうしないといけない」と丸暗記してください。