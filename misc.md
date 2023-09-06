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

`!=`は`==`を反転したものなので、以下のように`==`を反転させても同じ事が書ける。

{% capture code_noteq2 %}
fun main() {

  for(i in 0..<10) {
    if(!((i%3) == 0))
      println("${i}は3で割り切れない")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=code_noteq2 %}

けれどカッコが多くなってくると理解が難しくなってくるので、元の`!=`の方が良いでしょう。

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

慣れないうちは元のコードの方が理解しやすいかもしれませんが、慣れてくるとこちらのコードの方が、
違っているのが文字列だけだというのがひと目見て分かるので良いと思います。
積極的に使っていって頑張って慣れていきましょう。

## 2番目の区画は下でも良い

以前、[コードの置き場所入門](code_location_intro.md)では、メンバ変数やメンバ関数を書く2番目の区画、
というものを説明しました。

復習のために簡単に見直すと、最初にAndroid StudioでNew Projectを作った時は、以下のようなコードになっています。

```kotlin
import ...

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

このonCreateの上が2番目の区画なのでした。

```kotlin
import ...

class MainActivity : AppCompatActivity() {
    //
    // 2番目の区画
    //
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

最初のうちはどこに書くかの間違いを減らすべく限定した場所にしか書かないようにしていましたが、
そろそろ作っているものも複雑になってきたので、もう少し他の場所にも書き始めて良いと思います。

という事で、もう一つ2番目の区画を紹介しましょう。それはonCreateの後です。

```kotlin
import ...

class MainActivity : AppCompatActivity() {
    //
    //
    //
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
    //
    // ここも2番目の区画（New！）
    //
}
```

この下側にもメンバ変数やメンバ関数を置いて良い。

しかも、後ろに置いたものでもonCreateの中で使えます。
これは通常の変数には無い、メンバ変数だけの特徴です。

まだ出てきてないものを使うのは読みにくく感じる場合もあるので気をつけたい所ですが、
kotlinプログラム言語としては下の区画に置いたものも使う事が出来ます。

## showMessageの中身を軽く見てみる

これまで、ちょっとしたテストの為に、以下の関数をコピペしてもらっていました。

```kotlin
fun showMessage(msg: String) { Toast.makeText(this, msg, Toast.LENGTH_LONG).show() }
```

分からない部分もあるあろうけれど、分かる部分もあるので少し見てみます。

まず、showMessageは関数です。そして仮引数はmsgでString型です。それは以下の部分だけを見れば分かります。

```kotlin
fun showMessage(msg: String) 
```

そして関数の中を見ると以下のように書いてあります。

```kotlin
Toast.makeText(this, msg, Toast.LENGTH_LONG).show()
```

このうち、msgが仮引数ですね。

これは、Toastというものを作って表示するコードになります。
Toastというのはトースト、つまり食パンの事ですが、Androidではふわっと一定時間表示されて消える奴の事をトーストといいます。

これはトースター（ポップアップトースターという奴）で食パンを焼くと最後にぽん、と飛び出す感じに似てるからだと思います（そんな似てなくね？という気もするけれど）。

`Toast.makeText` というので、テキスト、つまり文字列からトーストを作れます。makeは作るって意味です。
なお、作ったトーストのshow()というのを呼ぶとトーストが実際に表示されます。

だから、以下全体で、

```kotlin
Toast.makeText(...).show()
```

JS入門でやった`MessageBox.show()`と同じようなものになります。

あとはmakeTextの引数になりますが、それは以下のようになっています。


```kotlin
(this, msg, Toast.LENGTH_LONG)
```

2番目は仮引数、つまりなんのメッセージを表示するか、という文字列になる。

3番目はトーストをどのくらい表示するかを表します。LENGTH_SHORTだと少し表示してすぐ消える。LENGTH_LONGだとしばらく表示したあとに消える。

1番目のthisは…これは現時点ではちゃんとは説明出来ないが、現在のActivityというものを表すものです。まぁこれはまだ良く分からない、と思っておいて良い。

以上を踏まえると、以下は、

```kotlin
Toast.makeText(this, msg, Toast.LENGTH_LONG).show()
```

「msgという文字列でLONGの長さでトーストを表示する」みたいな意味になり、元の関数全体、以下は、

```kotlin
fun showMessage(msg: String) { Toast.makeText(this, msg, Toast.LENGTH_LONG).show() }
```

「引数をトーストとしてLONGの長さで表示する関数」という事になります。

最初にthisを入れるのは[ListViewに挑む！表示編](listview_disp.md)のArrayAdapterなどにも登場しています。
今後もたまに出てくるので、出てきたら「なんかたまにこれ出てくるな」くらいに思っておいてください。

## レイアウトのXMLの直接編集

この辺まで来たら、そろそろレイアウトのXMLは直接編集しても良いと思います。
以下、コツを幾つか書いておきます。

### idは（うろ）覚える

Designビューでは簡単に入力できるidが、XML側ではちょっと変な入力をしなくてはいけません。これはidだけなので、idを覚えてしまえば他は割と簡単です。
慣れれば圧倒的に早く編集できるようになります。

idをbuttonCancelにしたい場合、XMLでは以下のように書きます。

```xml
<Button
    android:id="@+id/buttonCancel"
     />
```

このように `@+id/` という変なのをつけないといけない。
ただこれも、次の「補完に頼る」をマスターすれば勝手に出てくるので選ぶだけなので、
一字一句間違えずに覚える必要はなくて、「idだけ先頭にアットマークから始まる何かをつけないといけないんだな」
とぼんやり覚えている程度でOKです。

### 補完に頼る

例えば以下のようなXMLを入力したいとしましょう。

```xml
<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:id="@+id/buttonCancel"
    android:text="キャンセル" />
```

この時には、まず

1. `<`を入力し、Butくらいまで打つ
2. Buttonというのを一覧から選びEnter
3. layout_widthとlayout_heightを聞いてくるのでwraくらいまで打ってEnter, wrapくらいまで打ってEnterと二回やる
4. idと打って一覧から選ぶ
5. 一覧から`@+id/`を選ぶ
6. buttonCancelと打つ
7. texくらいまで打って一覧から選ぶ
8. キャンセルと入力する
9. 最後に `/` を入力する（子供が居ない奴の場合、LinearLayoutなど子供がいるものは`>`を入力）

という感じでやっていきます。
コツとしては、`andoird:text` などを入力したい時には`android:`は打たないで、`tex`から始める事です。

また、最初のButtonの所を選ぶ時には変なのが前についてない奴を選びましょう。LinearLayoutとかはandroidx.appcompat.widget.LinearLayoutCompatというのも一覧に出てくるけれど、
間違ってこちらを選ばないように。

あと最後の9も最初のうちはなれないと思うので注意しておきましょう。慣れればすぐにできるようになります。

慣れてくるとこっちの方が圧倒的に早く入力できるので、そろそろ慣れてしまいましょう。

### layout_widthとlayout_heightはその都度考える

match_parentかwrap_contentのどちらかを選ぶと思いますが、どちらなのかは毎回ちょっと考えて打ちます。
ここを毎回考えるのは、むしろレイアウトの理解を深めるのでXMLを直接編集するメリットです。

### 課題： XMLを直接編集して以下を作れ

NewからHelloXml1を作り、レイアウトを以下にせよ。

- LinearLayoutでorientation vertical
  - TextView, idはlabel1, textは「はろー」
  - LinearLayoutでorientationはHorizontal
     - ButtonでidはbuttonCancel, textは「キャンセル」
     - ButtonでidはbuttonOk, textは「おっけー」

何度かやれば慣れると思います。