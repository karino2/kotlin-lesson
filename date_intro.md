---
title: "日付を扱う、Date入門"
layout: page
---
時間を競うタイピング練習ソフトや投稿の日付など、時間や日時を扱う事はちょくちょくあります。
AndroidではDateというものを使います。importはjava.util.Dateです。

このweb上のplayground（下の奴）では、自分でimportを手書きしないといけないので、最初に`import java.util.Date`と書くようにしてください（これはコピペでいいです）。
AndroidStudio上ではいつもAlt+Enterとか一覧から選ぶとかで勝手にimportされます。

簡単なDateの使い方を見てみましょう。以下のようになります。

{% capture date_intro1 %}

import java.util.Date

fun main() {
  val dt = Date()

  println(dt)
}
{% endcapture %}
{% include kotlin_quote.html body=date_intro1 %}
