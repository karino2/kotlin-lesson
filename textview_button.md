---
title: "最初の一歩、TextViewとButtonを使ってみよう！"
layout: page
---
さてはじまりました、Android開発の最初の一歩。
TextViewを置いて、Buttonを置く、という事をやってもらいます。

まずはこちらの動画を見てください。

<iframe width="560" height="315" src="https://www.youtube.com/embed/cAKwRGI9zK8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

この動画で述べている、10回繰り返してもらう作業の手順をここに書いておきます。

## ステップ1: プロジェクトを新規作成

NewからProjectを選ぶ。
名前をHello2とかにする。
Finishを押す。

すると前のプロジェクトのウィンドウの下に隠れて新しいプロジェクトが出来ると思うので、前のウィンドウを閉じて新しいプロジェクトを前に出す。

## ステップ2: レイアウトの変更

次にレイアウトで、以下の作業をします。

- LinearLayoutにする
- TextViewにidを設定
- Buttonを追加してidを設定

以下詳細。

### 2-1. LinearLayoutにする

レイアウトの右上からCodeを選びxml表示にして、上の方の「`<androidx.constraintlayout.widget.ConstraintLayout`」みたいなところの、「`<`」より後ろを消す。
そして同じ場所で「Linear」くらいまでタイプして、
一覧からLinearLayoutを選ぶ。

次に「app:」で始まっているものを全て消す。

### 2-2. TextViewのidを設定

レイアウトの右上のところのDesignというのを押してデザインモードにして、
左下のところからTextViewを選ぶ。

そして右側のidというところにlabel1と打ってEnterを押す

### 2-3. Buttonを追加してidを設定

左上のCommonという所のButtonを選んでドラッグアンドドロップする。

ここでなんか横幅いっぱいになったら、以下のどちらかをしてください。

- layout_weightというのに1が入っていたらこれを削除
- layout_widthがmatch_parentになっていたらwrap_contentに変更

そしてidにbutton1を入れてEnter。何か聞かれたらRefactorというボタンを押す。

## ステップ3: ソースにsetOnClickListenerを追加

ソースの方を開いて、以下のようになっている所の、

```kotlin
import ...

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

setContentViewの行の下に、以下を追加する。

```kotlin
findViewById<Button>(R.id.button1).setOnClickListenr { findViewById<TextView>(R.id.label1).text = "ほげほげ" }
```

つまり、こうなる。

```kotlin
import ...

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        findViewById<Button>(R.id.button1).setOnClickListenr { findViewById<TextView>(R.id.label1).text = "ほげほげ" }
    }
}
```

## ワンポイントアドバイス

- 全部タイプせずになるべく一覧から選ぶ（一覧が出ない時は何か間違っている事が多いので早く気付ける）
- とにかく繰り返す。飽きるまで繰り返す。
- 最終的には何も見ないでもスラスラ出来るようになるのを目指す（けれど回数が重要なので考え込まずに最初は見ながらひたすら繰り返す）

## 発展課題：ボタンとTextViewの数を増やす

飽きてきたらTextViewを二つ置いてButtonも二つ置いて、それぞれのボタンで違うTextViewの表示を変更したりしてみてください。
基本的には`R.id.button1`の所と`R.id.label1`の所を変えるだけで他のボタンやTextViewに対して同じ事が出来るはずです。

具体的にはこんな感じ。

```kotlin
import ...

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        findViewById<Button>(R.id.button1).setOnClickListenr { findViewById<TextView>(R.id.label1).text = "ほげほげ" }
        findViewById<Button>(R.id.button2).setOnClickListenr { findViewById<TextView>(R.id.label2).text = "いかいか" }
    }
}
```

また、こんな風にするのも試してみて欲しい。

```kotlin
import ...

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        findViewById<Button>(R.id.button1).setOnClickListenr { 
          findViewById<TextView>(R.id.label1).text = "１つ目のボタン"
          findViewById<TextView>(R.id.label2).text = "押された。"
        }
        findViewById<Button>(R.id.button2).setOnClickListenr { 
          findViewById<TextView>(R.id.label1).text = "２つ目のボタン"
          findViewById<TextView>(R.id.label2).text = "２つ目のテキスト"
        }
    }
}
```

テキストはいろいろ変えて試してみて欲しい。

この手の作業を繰り返して何度もタイプする事で手に覚え込ますのが大切。
