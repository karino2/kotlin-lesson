---
title: "簡単な電卓を作ってみよう"
layout: page
---
Androidアプリの入門として、足し算とクリアしか無い電卓を作ってみよう。
簡単な方針を書いておきます。

## レイアウト

一番外側のLinearLayoutに、以下のような子供を持たせる。

1. 現在入力中の数字を表示するTextView
2. 「9, 8, 7, C」 のボタンを持ったLinearLayout
3. 「6, 5, 4, +」 のボタンを持ったLinearLayout
4. 「3, 2, 1, =」 のボタンを持ったLinearLayout
5. 「0」のボタン

という感じでどうでしょう？

Cはクリアです。
TextViewのテキストは最初は"0"にしておいてください。

## 数字ボタンを押したら数字が入力されるようにする

次に、数字ボタンを押したら、TextViewに数字が追加されていくようにします。
数字を追加していく、といいつつ、TextViewに入れるのは文字列です。

だから"0"とか"1"とかの文字列を追加していく感じになります。

文字列に数字を足していくのは、イメージとしては以下のような感じで出来ます。

{% capture str_add %}
fun main() {
  var s = "0"

  s += "1"
  s += "2"
  s += "3"

  println(s)
}
{% endcapture %}
{% include kotlin_quote.html body=str_add %}

ただしTextViewのtextは少しString型と違う型（ほとんど同じだけど）なので、足す前にtoString()してやる必要があります。
例えば数字の9ボタンを押されたら、以下のような感じにします。

```kotlin
  val s = findViewById<TextView>(R.id.result).text.toString()
  val s2 = s + "9"
  findViewById<TextView>(R.id.result).text = s2
```

これでもいいですし、もう少し短く同じ事を書くと以下みたいになるでしょうか。

```kotlin
findViewById<Button>(R.id.buttonNine).setOnClickListener { 
  findViewById<TextView>(R.id.result).text = findViewById<TextView>(R.id.result).text.toString() + "9"
}
```

少しややこしいね。頑張って解読してくれ。

これだと数字の一番左に余分な"0"が入っちゃいますが、まずはそれでいいでしょう。

## Cボタンを実装

Cボタンを押されたらTextViewの文字列を"0"にします。これは簡単でしょう。

## 文字列が0だったら特別扱いしよう

現状は"0"のあとに数字の1を押すと"01"になってしまっていると思います。
ですが電卓では、"1"になるのが正解です。

また、"0"の時に0のボタンを押しても普通は何もしませんが、現状は"00"になってしまうと思います。

これらをif文を使って直しましょう。

ここまでで、数字を入力していってCでクリアする事が出来るようになりました。

## 「+」を実装しよう

「+」は一番難しいところです。
そこでいくつかヒントを出しておきます。

### まずはやりたい事の整理

まずやりたい事を整理すると、以下の4ステップになります。

- ステップ1: "123"と入力する
- ステップ2: "+"を押す
- ステップ3: "456"と入力する
- ステップ4: "="を押すと"579"と表示されて欲しい

ステップ3で、"4"を入力した瞬間にTextViewをクリアする必要がありますが、これはちょっと手抜きで2の時点で"0"を入れてしまってもいいです。（この時点では何を言っているかわからなくても、実装すれば意味は分かると思う）

問題はステップ3の時点ではもう123はTextViewには入ってないのに、ステップ4では123と456の両方が必要、という事です。
ここが一番難しいところですが、その周辺の事も少しヒントを出しておきます。

### 文字列と数字の変換

`findViewById<TextView>(R.id.result).text` で取れるのは文字列なのだけど、これの足し算は連結になってしまいます。
例えば以下は、3になってくれません。

{% capture str_add %}
fun main() {
  val s1 = "1"
  val s2 = "2"

  println(s1+s2)
}
{% endcapture %}
{% include kotlin_quote.html body=str_add %}

これを3になるようにするには、文字列の足し算では無くIntの足し算にする必要があります。

文字列から数字に変換するのはtoIntというのを使います。

{% capture str_to_int %}
fun main() {
  val s = "123"
  val num = s.toInt()

  // 文字列と数字を足すと連結になっちゃう。(123+4で127にならない)
  println(s+4)

  // toInt()で数字にしたら足し算になる。
  println(num+4)
}
{% endcapture %}
{% include kotlin_quote.html body=str_to_int %}

少し課題として以下をやってみましょう。

**以下のTODOのところの2行を書き換えて、結果を579にせよ**

{% capture str_to_int_add_q1 %}
fun main() {
  val s1 = "123"
  val s2 = "456"

  // TODO: 以下の二行の右辺だけを書き換えよ
  val num1 = s1
  val num2 = s2

  // 以下は書き換えない
  println(num1+num2)
}
{% endcapture %}
{% include kotlin_quote.html body=str_to_int_add_q1 %}

**以下のTODOのところの2行の右辺に「追記だけ」して結果を19にせよ**

右辺に足すだけで結果を19にしてください。例えば"10"を10にする、とかはダメです（ダブルクオートを削除しているので）。

{% capture str_to_int_add_q1 %}
fun main() {
  // TODO: 以下の二行の右辺に追記だけする。消してはいけない
  val s1 = "10"
  val s2 = "9"

  // 以下は書き換えない
  println(s1+s2)
}
{% endcapture %}
{% include kotlin_quote.html body=str_to_int_add_q1 %}


### 計算結果を覚えておく方法

さて、先ほどのステップ2で値をクリアする時にステップ1の値を覚えておく必要がある、といいました。
このためには関数の外の変数を使います。

最初が以下のようになっているのが、

```kotlin
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
```

このonCreateの「外に」以下のように変数を用意しておくと値を覚えておけます（この説明はそのうちしますので今はそういうものと思って使ってください）。

```kotlin
    var genzainoAtai = 0

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
```

このgenzainoAtaiという変数に数字を保存しておく事が出来る。

保存したくなったら、以下のようにすれば良い。

```kotlin
findViewById<Button>(R.id.buttonAdd).setOnClickListener { 
  genzainoAtai = findViewById<TextView>(R.id.result).text.toInt()
  findViewById<TextView>(R.id.result).text = "0"
}
```

これだと、「123+456+789」などのように二回以上連続で+を押された時が普通の電卓と挙動が違いますが、まずはそれでもいいでしょう（直したければ直してもいいです）

### 「=」の処理を書く

最後にイコールの処理を書きます。
イコールが押されたら、genzainoAtaiとTextViewの値を足してTextViewに入れます。
また、この時にgenzainoAtaiを0にしておくと良いでしょう。

### CボタンでもgenzainoAtaiをクリアする

Cボタンのクリアでも、genzainoAtaiを0にしましょう。

これで足し算が出来る電卓が完成です。

## 発展問題：「+」を押された時に表示は前のままにしておく方法

「+」を押された瞬間にTextViewは"0"にクリアしちゃっていいと思うけれど、
もうちょっと電卓っぽい挙動にしたいなら、+を押した時点では表示は足した結果を出すのが正しい。
そうすると次に数字を押された時は特別扱いが必要。

どうやるかというと、Booleanのフラグを持たせる。

以下のコードに

```kotlin
    var genzainoAtai = 0

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
```

さらにフラグを追加する。

```kotlin
    var genzainoAtai = 0
    var isNextClear = false

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
```

次数字を入力された時はクリアしなくてはいけない時は、このisNextClearにtrueを入れておいて、
各数字のボタンのClickListenerの中でこのフラグを見て処理を変える。

10回繰り返さなくてはいけないのでだるいからここまではやらなくてもいい。関数の話を終えた後にやったらいいかもしれない。
