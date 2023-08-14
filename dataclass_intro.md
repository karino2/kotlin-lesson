---
title: "data class入門"
layout: page
---
今回はdata classの話をします。
クラスの話はもうちょっと後にするつもりなのだけど、とりあえずdata classは使いたいのでdata classの簡単な使い方を先に。

[Data classes - Kotlin Documentation](https://kotlinlang.org/docs/data-classes.html)

## ListViewに挑む！編集編に、投稿時間もつけたい

data classを使いたくなるシチュエーションとして、[ListViewの編集編](listview_edit.md)で作ったものに対し、
アイテム追加時の時間も表示したい、と考えたとします。

けれど、リストのアイテムは現状、String型となっている。

```kotlin
val listData = mutableListOf<String>()
```

ここに、StringとDateの二つを追加していきたい。そういう時に使うのがdata classです。

data classは、リストに二つ以上の項目を同時に追加していきたい時に使います。

## data classの簡単な例

まずdata classの例を見ていきたいと思います。

data class は以下のように使います。


{% capture dataclass_intro1 %}
data class TwoInt(val v1: Int, val v2: Int)

fun main() {
  val ti1 = TwoInt(1, 2)

  println(ti1.v1)
  println(ti1.v2)
}
{% endcapture %}
{% include kotlin_quote.html body=dataclass_intro1 %}
