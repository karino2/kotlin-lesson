---
title: "メニュー"
layout: page
---
ここではメニューの作り方について簡単に説明します。

といってもメニューは簡単なので大して説明する事はありません。細かい解説は動画の方でしてあるので、以下の説明で分からない所があれば動画を見てください。
以下ではメニューを追加する手順を簡単に説明します。

なお、画面の上に出るタイトルバーをAndroidではActionBarと呼びます。

<iframe width="560" height="315" src="https://www.youtube.com/embed/v09KfD041nw?si=NgwxAWIbqRhkkELA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## 手順1: menuのxmlを作る

- resの所で右クリックしてNewからAndroid Resource Fileを選ぶ
- 二行目のResource Typeをmenuにして、一行目のファイルの名前をmenu_mainとしてOKを選ぶ
- Menu Itemというのをドラッグアンドドロップしていく
- Menu Itemのattributeを右側の一覧で設定していく

Menu Itemのattributeで設定するのは以下の4つとなります。

- id ... いつものヤツ。この講座ではactionXxxという名前でメニューのidをつける事にします
- title ... 表示されるテキスト
- icon ... ActionBarに表示される場合にアイコンになる、保存とか良くあるヤツは幾つか最初から用意されているので選ぶだけでOK
- showAsAction ... 三つポチの中に入るか、ActionBarに表示するかを選ぶ
  - always ... いつもActionBarに出す。その画面のメインの操作のものはこれを指定する。
  - ifRoom ... スペースがあればActionBarに出し、無ければ三つポチの中に入れる。分からなければこれを指定しておけばOK
  - never ... 普段はユーザーの目に入れたくないものなど、いつも三つポチの中に入れる。たまにしか使わないヤツはここに入れる。

## 手順2: テーマから「.NoActionBar」を取る

res/values/themes/themes.xml というファイルが二つあると思うので、その中の以下の行を探し出して、

```xml
    <style name="Base.Theme.HelloMenu" parent="Theme.Material3.DayNight.NoActionBar">
```

最後の「.NoActionBar」を削る。ドットごと削る事に注意。

```xml
    <style name="Base.Theme.HelloMenu" parent="Theme.Material3.DayNight">
```

## 手順3: onCreateOptionsMenuという所を作ってmenuInflaterとか言うヤツを呼ぶ

ここは覚えゲー。二番目の区画で、「onCreateOp」くらいまで打つと一覧が出るので、それを選ぶ。
二番目の区画については以下を参照。

[コードの置き場所入門](code_location_intro.md)

で、onCreateOptionsMenuを選ぶと以下のコードが生成される。

```kotlin
    override fun onCreateOptionsMenu(menu: Menu?): Boolean {
        return super.onCreateOptionsMenu(menu)
    }
```

これを、以下のように書き換える。

```kotlin
    override fun onCreateOptionsMenu(menu: Menu?): Boolean {
        menuInflater.inflate(R.menu.menu_main, menu)
        return true
    }
```

これでメニューが表示されるようになりました。

## 手順4: メニューが押された時の処理を、onOptionsItemSelectedという所に書く

先ほどのonreateOptionsMenuと同様、二番目の区画で「onOp」くらいまで打つと一覧が出るので、OptionsItemっぽいのを選ぶと以下が作られる。

```kotlin
    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        return super.onOptionsItemSelected(item)
    }
```

このitemのitemIdというヤツでどのメニューが選ばれたのかを判断し、それぞれ処理を書く。処理が終わったらreturn trueする決まり。

```kotlin
    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        if(item.itemId == R.id.actionSave) {
          // ここに保存ボタンが押された処理を書く

          return true
        } else if(item.itemId = R.id.actionSetting) {
          // ここに設定ボタンが押された処理を書く

          return true
        }
        return super.onOptionsItemSelected(item)
    }
```
