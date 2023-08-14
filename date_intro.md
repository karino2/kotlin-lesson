---
title: "日付を扱う、Date入門"
layout: page
---
時間を競うタイピング練習ソフトや投稿の日付など、時間や日時を扱う事はちょくちょくあります。
AndroidではDateというものを使います。importはjava.util.Dateです。

{% capture date_intro1 %}

import java.util.Date

fun main() {
  val dt = Date()

  println(dt)
}
{% endcapture %}
{% include kotlin_quote.html body=date_intro1 %}
