---
title: "List入門の課題の解答"
layout: page
---
[リスト入門](list_intro.md)の課題の答えです。

**課題: 何番目の要素の値がいくつか、と出力せよ**

以下のように出力せよ

- 0番目の要素は「月曜」
- 1番目の要素は「火曜」
- ...中略...
- 6番目の要素は「日曜」

{% capture index_code3 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  // TODO: 以下を書け
  // 解答
  for(i in 0..<youbi.size) {
    println("${i}番目の要素は「${youbi[i]}」")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=index_code3 %}


**課題: 以下のコードを変更し、曜日を2個飛ばしに出力せよ**

曜日を二つ飛ばし、つまり月曜、木曜、日曜と出力されるようにしてください。

{% capture index_code6 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  // TODO: 以下を変更
  // 解答
  for(i in 0..<youbi.size) {
    if (i%3 == 0) {
      println("${i}番目の要素は「${youbi[i]}」")
    }
  }
}
{% endcapture %}
{% include kotlin_quote.html body=index_code6 %}

**課題: 日曜、金曜、水曜...と逆順の一つ飛ばしでprintlnせよ**

以下のコードを変更して、逆順でさらに一つ飛ばしになるようにしてみましょう。
一つ飛ばしは[forループ入門](for_loop.md)を参考に。

{% capture revindex_code4 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  for(a in  0..<youbi.size) {
    // TODO: 以下を修正せよ
    // 解答
    val revIndex = youbi.size - 1 - a
    if (revIndex %2 == 0) {
      println(youbi[revIndex])
    }
  }
}
{% endcapture %}
{% include kotlin_quote.html body=revindex_code4 %}

**課題: withIndex()を使って偶数の曜日だけprintlnせよ**

月、水、金、日曜が出力されれば正解です。

{% capture withindex_code2 %}
fun main() {
  val youbi = listOf("月曜", "火曜", "水曜", "木曜", "金曜", "土曜", "日曜")

  // 解答
  for((index, elem) in youbi.withIndex()) {
    if(index%2 == 0) {
      println(elem)
    }
  }
}
{% endcapture %}
{% include kotlin_quote.html body=withindex_code2 %}

**課題: 要素100個のリストをfor文で作れ**

要素が以下の百個のリストを作れ。最初が「0番目」じゃなくて「1番目」なのに注意。

"1番目", "2番目", "3番目", ... , "100番目"

なお、出力は「100番目」になるのが正解。

{% capture mlist_100 %}
fun main() {
  // 以下を書き換える
  // 解答
  val mlist = mutableListOf<String>()

  // ここでfor文を使って100個の文字列をmlistに入れる。
  // 解答2
  for(i in 0..<100) {
    mlist.add("${i+1}番目")
  }


  // 以下はいじらない
  println(mlist[99])
}
{% endcapture %}
{% include kotlin_quote.html body=mlist_100 %}

**課題: MutableListから1番目と3番目の要素を削除せよ**

mlistには0番目から5番目までのアイテム（つまり6個のアイテム）が入っています。
ここから、1番目と3番目のアイテムを削除してください。

1番目の要素を削除すると２番目以降の要素が一つずれる事に注意してください。
結果として「0番目のアイテム」、「2番目のアイテム」「4番目のアイテム」、「5番目のアイテム」が出たら正解です。

{% capture mlist_remove2 %}
fun main() {
  val mlist = mutableListOf<String>()
  for(i in 0..5) {
    mlist.add("${i}番目のアイテム")
  }

  // TODO: ここで1番目と3番目の要素を削除せよ
  // 解答
  // 先に1番目を消すと3番目が一つずれて2番目になるので、以下のように逆順にするか、removeAt(1), removeAt(2)とする。
  mlist.removeAt(3)
  mlist.removeAt(1)


  // 以下はいじらない
  for((index, elem) in mlist.withIndex()) {
    println("${index}番目の要素は「${elem}」です");
  }
}
{% endcapture %}
{% include kotlin_quote.html body=mlist_remove2 %}
