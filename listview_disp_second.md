---
title: "ListViewに挑む！表示編、二回目用"
layout: page
---
かなり難しいところなので何度か繰り返して欲しいListView、
けれど二回目なら最初から自分のレイアウトで試した方がいいと思うので、そのための手順を書いておきます。

## ListViewをレイアウトに置く

Layout側にListViewを置き、idをlistViewにする

画面いっぱいになるようにする。

## 新しいレイアウトを定義して、以下の要件を満たすようにする

- ファイル名はlist_item.xml
- トップはLinearLayout
- TextViewを置く、idはitemLabelとする
- Buttonを置く、idはitemButtonとする、textは"ボタン"とかかな（なんでもいい）


## データ100件くらいのlistDataを作る

メンバ変数のところに空のMutableList型のlistData変数をmutableListOfで作って、onCreateで100件くらいfor文でデータを入れる。

## メンバ変数にArrayAdapterをlazyで作る

adapterをメンバ変数として定義します。getViewをオーバーライドしたりするのでかなり難解ですが、以下みたいな感じのコードを書きます。

```kotlin
val adapter by lazy { object: ArrayAdapter<String>(this, R.layout.list_item, listData) {
            override fun getView(position: Int, convertView: View?, parent: ViewGroup): View {
                val view = if(convertView == null) layoutInflater.inflate(R.layout.list_item, null) else convertView

                val data = listData[position]
                view.findViewById<TextView>(R.id.itemLabel).text = data
                return view
            }
        }
    }
```

## showMessageを作る

メンバ関数として、以下をコピペする。

```kotlin
fun showMessage(msg: String) { Toast.makeText(this, msg, Toast.LENGTH_LONG).show() }
```

### onCreateでListViewにadapterをセットする

ListViewをfindVewByIdで取り出して、adapterをセットします

```kotlin
findViewById<ListView>(R.id.listView).adapter = adapter
```

### ボタンが押された時の処理も書いてみる

getViewの中で、以下みたいな感じで何か書く（showMessageでアイテムの内容を使った何かを表示するのがいいかもしれない）

```
    view.findViewById<Button>(R.id.itemButton).setOnClickListener = ...
```

