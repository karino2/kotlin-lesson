---
title: "日付を扱う、Date入門"
layout: page
---
時間を競うタイピング練習ソフトや投稿の日付など、時間や日時を扱う事はちょくちょくあります。
AndroidではDateというものを使います。importはjava.util.Dateです。

## Dateの最初の例

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

`Date()`と呼ぶと、呼んだ時点での「Dateオブジェクト」が得られます。型はDate型です。
そしてDateオブジェクトをそのままprintlnすると、いい感じの文字列として出力されます。（dt.toString()でも同じ文字列が得られる）。

なお、このページ上だとUTCの時間になってしまうので8時間ほど時差があるかもしれませんが、実機だと端末のタイムゾーンの時間になります。

### Dateのプロパティのうち時間とか日付を取るもの

上のように取得したdtは、`dt.year`とすると年（2023とか）とかが取れます。
2023/8/14 10:39 25秒 を例に幾つになるかを示しておきます。

- year ... 123 （2023じゃないのに注意！）
- month ... 7 （8じゃないのに注意！）
- date ... 14
- hours ... 10
- minutes ... 39
- seconds ... 25

yearは1900年を基準にした年数が帰ってきます。だから西暦にするには1900を足す必要がある。
monthも0から始まるので、月にするには1を足す必要がある。
まぁこの辺は何故？とあまり真面目に考えても仕方ないので、歴史的事情で変な事になってしまっている、くらいに思っておきましょう。

以下、試してみましょう。

{% capture date_prop1 %}

import java.util.Date

fun main() {
  val dt = Date()

  println(dt)
  println(dt.year)
  println(dt.month)
  println(dt.date)
  println(dt.hours)
  println(dt.minutes)
  println(dt.seconds)

  println("今は${dt.year+1900}年${dt.month+1}月${dt.date}日の${dt.hours}時${dt.minutes}分${dt.seconds}秒です")  
}
{% endcapture %}
{% include kotlin_quote.html body=date_prop1 %}

こういう事情をあまり手で計算するのは良くないので、DateFormatというのを使ってフォーマットするのが推奨なのですが、
このコースではこうやって手で計算する事にします。

一応DateFormatを使うコードも載せるだけ載せておきます。

{% capture date_prop2 %}

import java.util.Date
import java.text.DateFormat

fun main() {
  val dt = Date()
  val dtf = DateFormat.getDateTimeInstance()
  val result = dtf.format(dt)

  println(result)
}
{% endcapture %}
{% include kotlin_quote.html body=date_prop2 %}

このページ上だと英語になってしまいますが、スマホ上だと日本語のいい感じの日付表示になります。

**課題: Date型の引数を取り、"2023年8月14日 14:57"という感じの文字列を返す、dateFormatという関数を作れ**

結果は文字列な事に注意。

{% capture date_prop_q1 %}
import java.util.Date

// TODO: ここにdateFormat関数を作れ


// 以下はいじらない
fun main() {
  val dt = Date()
  val result = dataFormat(dt)

  println(result)
}
{% endcapture %}
{% include kotlin_quote.html body=date_prop_q1 %}

### 経過時間などを測ったり、保存したりする時に使うtimeプロパティ

{% capture date_time1 %}

import java.util.Date

fun main() {
  val dt = Date()

  println(dt)
  println(dt.time)
}
{% endcapture %}
{% include kotlin_quote.html body=date_time1 %}
