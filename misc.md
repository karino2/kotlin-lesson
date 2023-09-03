---
title: "その他ほそぼそとした事"
layout: page
---
次の実践編に入る前に、説明しておきたかったがどこにも入れられなかったような細々とした事をここにまとめておきます。

## ifで使う `&&`と`||`と`!`

if文の所で、二つの条件が両方とも正しかったら、というのと、どちらかが正しかったら、という説明をしていなかったのだが、
この二つの機能はよく使うので見ておく。（たまに課題のヒントなどで書いてはいた）

`&&`で、その左右の両方がtrueだったらtrueを、`||`でその左右のどちらかがtrueだったらtrueを返す。

{% capture code_andor0 %}
fun main() {
  val trueFlag1 = true
  val trueFlag2 = true
  val falseFlag1 = false
  val falseFlag2 = false

  println(trueFlag1 && falseFlag1)
  println(trueFlag1 || falseFlag1)
  println(trueFlag1 && trueFlag2)
  println(trueFlag1 || trueFlag2)
  println(falseFlag1 && falseFlag2)
  println(falseFlag1 || falseFlag2)
}
{% endcapture %}
{% include kotlin_quote.html body=code_andor0 %}

これは普通if文で使う。例えば以下のような感じで使う

```kotlin
if(findViewById<Switch>(R.id.switch1).isChecked && findViewById<Switch>(R.id.switch2).isChecked) {
  findViewById<TextView>(R.id.label1).text = "両方オンにされています！"
} else if (findViewById<Switch>(R.id.switch1).isChecked || findViewById<Switch>(R.id.switch2).isChecked) {
  findViewById<TextView>(R.id.label1).text = "片方だけオンです！"
}
```

他にもEditTextが何も入力されていないかチェックされていなかったら、とかのようなものもたまにある。

```kotlin
if(!findViewById<Switch>(R.id.switch1).isChecked || findViewById<EditText>(R.id.edit1).text == "") {
  showMessage("スイッチがオンになってないかEditTextが空です。")
  return
}
// 以下実際の処理を行う
// ...
```

なお、条件が複雑になってくると読みにくくなってくるので、分かりやすい名前をつけた変数に一旦入れると良い事もある。

```kotlin
val ryouhouChecked = findViewById<Switch>(R.id.switch1).isChecked && findViewById<Switch>(R.id.switch2).isChecked
val docchirakaChecked = findViewById<Switch>(R.id.switch1).isChecked || findViewById<Switch>(R.id.switch2).isChecked

if(ryouhouChecked) {
  findViewById<TextView>(R.id.label1).text = "両方オンにされています！"
} else if (docchirakaChecked) {
  findViewById<TextView>(R.id.label1).text = "片方だけオンです！"
}
```

この例に限らず、読みやすくするために変数を使う、というのはそろそろ覚えて欲しい技です。

### `!`で結果を反転

trueだったらfalseを、falseだったらtrueを返す、という`!`というものもあります。notと読む事が多いですね。

{% capture code_not0 %}
fun main() {
  val trueFlag1 = true
  val falseFlag1 = false

  println(!trueFlag1)
  println(!falseFlag1)
}
{% endcapture %}
{% include kotlin_quote.html body=code_not0 %}

両方が選ばれている、みたいな条件が正しくない時、などに使います。
どの範囲を反転させるかをカッコではっきりさせるのが良いでしょう。

```kotlin
if(!(findViewById<Switch>(R.id.switch1).isChecked && findViewById<Switch>(R.id.switch2).isChecked)) {
  showMessage("どちらかがチェックされてません！")
  return
}
```

ちなみに、 `!(a && b)` と `(!a) || (!b)` は同じ意味になります。
高校生でド・モルガンの法則という名前で習いますが、
かっこいい名前の割には大したことない奴ですね。

ただこの`!`はあんまり使いすぎると複雑な条件になってしまい、
実は同じ事をもっと単純に表せる、というのはよくあります。`!` があんまり出てきた時は、「同じ事を `!` 無しで表せないか？」と考えてみると、
もっとずっとシンプルな例が見つかる事もよくあります。

### `!=`で等しくない

ついでに文字列などが、等しくない、という事を表す`!=`もやっておきます。これまで使っていたかもしれませんが、なんか説明していなかった気がしたので。
文字列が等しくない、というのはたまに必要になります。

{% capture code_noteq0 %}
fun main() {
  val label = "ふがふが"

  if(label != "ほげいか") {
    println("labelがほげいかじゃありません！")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=code_noteq0 %}

数字でも使います。
例えば奇数は2で割った余りが0では無い、と表現する事が出来るので、以下のように書ける。（割ったあまりは`%`でした）

{% capture code_noteq1 %}
fun main() {
  for(i in 0..<10) {
    if((i%2) != 0)
      println("${i}は奇数です")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=code_noteq1 %}

3で割り切れない数を書くなら以下。

{% capture code_noteq1 %}
fun main() {

  for(i in 0..<10) {
    if((i%3) != 0)
      println("${i}は3で割り切れない")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=code_noteq1 %}


### 課題： 31日ある月だったらtrueを、無い月ならfalseを返す関数を作れ

1月は31日まであります。
2月は28日までしか無いですよね。
このように、月によって31日まである月と無い月があります。

31日まで無い月は以下になります。

- 2月
- 4月
- 6月
- 9月
- 11月

2469 11を「西向く サムライ（十一を縦に並べて武士の士と読む）」などと語呂合わせで覚えたりします。

ここでは、これ以外の月ならtrueを、これらの月ならfalseを返す関数、is31を作りましょう。
引数は1〜12のIntです。

ヒント: `||`や`||`を上の月の数だけ並べる感じでどうでしょう？

{% capture code_andor_q1 %}
// TODO: ここにis31を作れ

fun main() {
  println(is31(1))
  println(!is31(2))
  println(!is31(4))
  println(is31(12))
}
{% endcapture %}
{% include kotlin_quote.html body=code_andor_q1 %}


## if else をイコールの右に書く

[ListViewに挑戦](listview_disp.md)のgetViewの所で使ってはいたけれど、ifとelseは両方の最後の文が値で型が同じだったら、
イコールの右側に使う事が出来ます。

{% capture code_ifelse0 %}
fun main() {
  val flag = true
  // val flag = false

  val label = if(flag) {
    "フラグはtrue！"
  } else {
    "フラグはfalse..."
  }

  println(label)
}
{% endcapture %}
{% include kotlin_quote.html body=code_ifelse0 %}

このように、labelの右側にif elseを置く事が出来る。
最後の文の値が重要なので、間に違うのがあってもよい。

{% capture code_ifelse1 %}
fun main() {
  val flag = true
  // val flag = false

  val label = if(flag) {
    println("こんなのがあってもOK")
    "フラグはtrue！"
  } else {
    "フラグはfalse..."
  }

  println(label)
}
{% endcapture %}
{% include kotlin_quote.html body=code_ifelse1 %}

これを行う時には、どれでも無い、というケースがあってはいけません。
だから以下はエラーになります。

{% capture code_ifelse2 %}
fun main() {
  val num = 12345

  val label = if((num%2) == 0) {
    "偶数"
  } else if((num%3) == 0){
    "3で割り切れる"
  } // どちらでも無い場合は？となるのでエラー

  println(label)
}
{% endcapture %}
{% include kotlin_quote.html body=code_ifelse2 %}

どちらでも無いケースを足せばOK。

{% capture code_ifelse3 %}
fun main() {
  val num = 12345

  val label = if((num%2) == 0) {
    "偶数"
  } else if((num%3) == 0){
    "3で割り切れる"
  } else {
    "どちらでも無い"
  }

  println(label)
}
{% endcapture %}
{% include kotlin_quote.html body=code_ifelse3 %}

これはラベルの値だけが違っていて同じTextViewに入れたいだけ、みたいな時に便利です。
例えば以下のようなコードは、

```kotlin
if(findViewById<Switch>(R.id.switch1).isChecked) {
  findViewById<TextView>(R.id.label1).text = "スイッチオン！！"
} else {
  findViewById<TextView>(R.id.label1).text = "スイッチがオフです(´・ω・｀)ｼｮﾎﾞｰﾝ"
}
```

TextViewは同じで入れる文字列が違うだけなので、以下のように書く方がスッキリします。

```kotlin
val label =if(findViewById<Switch>(R.id.switch1).isChecked) {
      "スイッチオン！！"
    } else {
      "スイッチがオフです(´・ω・｀)ｼｮﾎﾞｰﾝ"
    }

findViewById<TextView>(R.id.label1).text = label
```

なれないうちは下のコードの方が理解しやすいかもしれませんが、慣れてくるとこちらのコードの方が、
違っているのが文字列だけだというのがひと目見て分かるので良いと思います。
積極的に使っていって頑張って慣れていきましょう。


