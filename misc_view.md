---
title: "EditText、チェックボックスなどを使ってみる"
layout: page
---

TextViewとButton 10回の修行を終えたら、次は他のViewも幾つか使ってみる。

## EditText入門

<iframe width="560" height="315" src="https://www.youtube.com/embed/4MGwDtuVH7Q" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

とりあえずこの動画の通り繰り返してください。コードだけ貼っておきます。

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        findViewById<Button>(R.id.buttonSubmit).setOnClickListener {
            findViewById<TextView>(R.id.label1).text = findViewById<EditText>(R.id.edit1).text
        }

        findViewById<Button>(R.id.buttonClear).setOnClickListener {
            findViewById<TextView>(R.id.label1).text = "むぇ〜〜"
        }
    }
}
```

## チェックボックス入門

今回も動画メインです。冒頭でスイッチもやると言っておきながら途中で喋り疲れたのでチェックボックスだけです。

<iframe width="560" height="315" src="https://www.youtube.com/embed/xoXOrtL6mWQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

今回もコードだけ貼っておきます。

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        findViewById<Button>(R.id.button1).setOnClickListener {
            if(findViewById<CheckBox>(R.id.checkBox1).isChecked) {
                findViewById<TextView>(R.id.label1).text = "チェックされてるよ、やったね"
            } else {
                findViewById<TextView>(R.id.label1).text = "なんかチェックされてないんだけど？"
            }
        }
    }
}
```

## ここまでのコードをふわっと解説

さて、ここまで一切説明せずに作業をしてきましたが、そろそろなんとなく分かってきた部分もあると思います。
ちゃんとした説明は後半に回しますが、この時点でも軽く説明をしておきたい。

### TextViewとR.id.label1の違い

どうもTextViewと書く所と、R.id.label1と書く所があって、なんか同じような事二回書かされているような気分になっていると思いますが、
両者の違いもこの辺までちゃんと作業していたら、なんとなくは分かってきたんじゃないか。

一番違いが分かりやすいのは、TextViewが二つある時。
label1、label2とidをつけた時、TextViewと書く所はどちらもTextViewですが、idの所だけ違います。

以下の二つのコードを見比べてみましょう。

**label1の方のコード**

```kotlin
findViewById<TextView>(R.id.label1).text = "こっちはラベル1"
```

**label2の方のコード**

```kotlin
findViewById<TextView>(R.id.label2).text = "こっちはラベル2"
```

どちらも`TextView`の所は同じです。一方で二つTextViewを置いた時のどっちなのかはidの方で指定しています。（上のコードで`R.id.label1`と`R.id.label2`のところ）

### findViewByIdまではだいたい一緒、閉じカッコのあとだけ違う

TextViewのテキストを変更するコードは以下でした。

```kotlin
findViewById<TextView>(R.id.label1).text = "こっちはラベル1"
```

チェックボックスがチェックされたかどうかのコードは以下（をifの中に書いていた）でした。

```kotlin
findViewById<CheckBox>(R.id.checkBox1).isChecked
```

ボタンが押された時に何かするコードは以下でした。

```kotlin
  findViewById<Button>(R.id.button1).setOnClickListener { }
```

これら三つを比べると、`findViewById<TextView>(R.id.label1)` まではどれもあまり違いが無い。
TextViewとかidとかは違うけれど、だいたい同じ。

一方で、その後の「`.`」の後はそれぞれ結構違う。
`.text` だったり `.isChecked` だったり `.setOnClickListener` だったりして、
しかもそれぞれ`=`の左だったりifの中だったりその後に `{ }` が続いたりと結構違う。

### ボタンはなんか難しい

ボタンはsetOnClickListenerの所で `{ }` が出てきて、この中に色々書いたりするので他よりちょっと難しい気がする。

現時点ではなんか難しいが丸暗記はしたぜ、くらいに思えたらOKです。