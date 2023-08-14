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

では、hoursなどを使う課題をやってみましょう。

**課題: Date型の引数を取り、"2023年8月14日 14:57"という感じの文字列を返す、formatDateという関数を作れ**

結果は文字列な事に注意。二つ上のコード例のprintlnを参考にやってみましょう。

ちなみにformatというのは書式、みたいな意味で、いい感じな文字列に変換する時に使われる英単語です。

{% capture date_prop_q1 %}
import java.util.Date

// TODO: ここにformatDate関数を作れ


// 以下はいじらない
fun main() {
  val dt = Date()
  val result = formatDate(dt)

  println(result)
}
{% endcapture %}
{% include kotlin_quote.html body=date_prop_q1 %}

**課題: ListViewに挑戦、の課題で、ボタンを押した時にshowMessageする所で、タップされた時点での日付と時間を追加せよ**

[ListViewに挑む！表示編、二回目用](listview_disp_second.md)で、アイテムのボタンを押されたら、以下のような感じのコードを実行していたと思います。（文字列は適当）

```kotlin
showMessage("${data}のボタンが押されたよ")
```

これを、上で作ったformatDateを使って、以下のように変更してみます。

```kotlin
val dt = Date()
val sdate = formatDate(dt)
showMessage("${data}のボタンが押されたよ (${sdate})")
```

ボタンを適当に押してみて、ちゃんと時間が表示されているか確認してください。

## 経過時間などを測ったり、保存したりする時に使うtimeプロパティ

Dateというのは、内部的には1970年1月1日0時0分0秒を基準に、そこからどれだけ過ぎたのか、という秒数を保持しています。
より正確に言うとミリ秒で保存しています。

これを取り出すのが`time`プロパティです。
試してみましょう。

{% capture date_time1 %}

import java.util.Date

fun main() {
  val dt = Date()

  println(dt)
  println(dt.time)
}
{% endcapture %}
{% include kotlin_quote.html body=date_time1 %}

なんで1970年なのか、とかは気にしても仕方ない謎の数字と思っておいてください。そういうものです。

### timeの型はIntでは無くLong

1970年から現在が何msec経ったか？というのはめちゃくちゃ大きな数字です。
こんな大きな数字は普段使っている数字、つまりIntという範囲には収まりません。

そこでこのtimeをおさめる変数の型は、Longという、容量を倍使う整数の型になっています。
細かい事はおいといて、timeをListなどに保存する時は`List<Int>`では無く`List<Long>`な事を覚えておいてください。

また、細かい計算をする時にもたまにこのIntをあふれてしまう事があります。

例えばミリ秒を年数に変換する事を考えます。つまりtimeの値から、1970年から何年後なのかを計算してみたい。

ミリ秒、という事なので、1秒は1000ミリ秒、1分は60秒なので60*1000ミリ秒、と考えていく。
閏年を無視すると、一年が365日で1日が24時間で1時間が60分で1分が60秒で1秒が1000ミリ秒なので、
`365*24*60*60*1000` で割ると何年たっているかが分かる。

でも、これはちょっとIntには収まらないので、これをそのまま計算しようとするとたまにIntをはみ出して変な結果になってしまったりする。
そういう時は一旦1000で割ったものを変数に入れて、そのあと残りで割るとかやるとうまく行ったりする。

例えば以下みたいな感じ。

{% capture date_time2 %}

import java.util.Date

fun main() {
  val dt = Date()

  println(dt)
  println(dt.time)
  val sec = dt.time/1000
  val year = sec/(365*24*60*60)
  println(year)
}
{% endcapture %}
{% include kotlin_quote.html body=date_time2 %}

2023年現在で試すと53と出るので、1970年から53年経ってるという事であってそう。

こういう計算をやってもらう事はこのシリーズでは無いけれど、timeはめっちゃでかい値なのでLongという型になっていて、
気をつけないで計算するとたまにIntを経由して変な値になる事がある、と知っておくと良いです。

### 特定の時間に戻すにはtimeにセットする

保存したLongの数値を元にDateオブジェクトを作りたい時は、Longの値を代入すれば良い。
これは現時点では意味がわからないかもしれないけれど、後で使う事になるのでそこまで行ったら見直そう。

さっき自分が試した時はUTCの8月14日の朝6時27分だったのが`1691994462475`だったので、これをセットするとUTCの8月14日、6時27分が得られる。
試してみましょう。

{% capture date_time3 %}

import java.util.Date

fun main() {
  val dt = Date()
  dt.time = 1691994462475

  println(dt)
}
{% endcapture %}
{% include kotlin_quote.html body=date_time3 %}

こうやって大きい数字を覚えておくだけでその日、というのを復元出来るので、投稿日をファイルとかに保存する時などに便利です。

### 経過時間はtimeを引くと分かる

タイピング練習ソフトなどで、全部終わるまでの時間を測って時間でスコアを作りたい、みたいな事がよくある。

そういう時にはtimeの結果を引けば、何msecかかったかが分かる。

{% capture date_time4 %}

import java.util.Date

fun main() {
  val beginTime = Date().time

  var sum = 0
  for(i in 1..100000) {
     for(j in 1..100000) {
        sum +=i
     }
  }

  println(sum)

  val endTime = Date().time

  val diff = endTime - beginTime
  println("${diff}msec、つまり${diff/1000}秒かかりました")
}
{% endcapture %}
{% include kotlin_quote.html body=date_time4 %}

これを使って、[OffTyping - Google Play のアプリ](https://play.google.com/store/apps/details?id=com.livejournal.karino&hl=ja)のようなアプリを作る事が出来ます。
（Enter押さないでハンドルするのはまだやってないのでEnterを押さないといけないけれど）


[Date (Java Platform SE 8 )](https://docs.oracle.com/javase/8/docs/api/java/util/Date.html#getTime--)

