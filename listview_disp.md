---
title: "ListViewに挑む！表示編"
layout: page
---
Android初心者の墓場、またはAndroid開発者かそうでないかを分ける試金石、ListViewに挑みます。
二回に分けて作業していきます。前篇のここでは、リストのデータを表示する事にチャレンジ。

ここは直接会って説明するのをメインにするので、覚書程度にしておく。（そのうち動画にしたい）

## メンバ変数の雑な説明と表示するデータ

最初、新規のプロジェクトを作ると以下のようなコードがあります。

```kotlin
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
```

このonCreateの外に定義する変数をメンバ変数といいます。（メンバ変数が何かはクラスの話が必要になってくるので、現時点ではこの説明で納得してください）

例えば、以下のlistDataはメンバ変数です。

```kotlin
    val listData = listOf("１つ目のアイテム", "２つ目のアイテム", "３つ目のアイテム", "４つ目のアイテム", "５つ目のアイテム")

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
```

以後、「メンバ変数としてXXを定義してください」と言ったら、このonCreateの外に変数XXを定義する、と理解してください。

ここから先はこのlistDataを表示するのを目指します。

## ListViewをレイアウトに置く

Layout側にListViewを置き、idをlistViewにする

画面いっぱいになるようにする。

## 新しいレイアウトを定義して、以下の要件を満たすようにする

- ファイル名はlist_item.xml
- トップはLinearLayout
- TextViewを置く
- idはitemLabelとする

## メンバ変数にArrayAdapterのgetView差し替えたものを作る

```kotlin
val adapter = object: ArrayAdapter<String>(this, R.layout.list_item, listData) {
            override fun getView(position: Int, convertView: View?, parent: ViewGroup): View {
                val view = convertView ?: layoutInflater.inflate(R.layout.list_item, null)

                val data = listData[position]
                view.findViewById<TextView>(R.id.itemLabel).text = data
                return view
            }
        }
```

なお、getViewをoverrideするので本当はコンストラクタのR.layout.list_itemはなんでも良い（使わないので）。

## onCreateでListViewにadapterをセットしてitemのクリックリスナーを作る

```kotlin
findViewById<ListView>(R.id.listView).adapter = adapter
findViewById<ListView>(R.id.listView).setOnItemClickListener { parent, view, position, id ->
    val selectedText = view.findViewById<TextView>(R.id.itemLabel).text
    Toast.makeTex(this@MainActivity, "${selectedText}が選ばれました", Toast.LENGTH_SHORT).show()
}
```

Toastのところはとりあえずテスト用に何か表示したいだけなのでここでは説明はしません。