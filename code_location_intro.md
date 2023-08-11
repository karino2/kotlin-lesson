---
title: "コードの置き場所入門"
layout: page
---
次の「ListViewに挑戦！」あたりから、中括弧がたくさん出てきてどこに何を置いたらいいのかが分からなくなりがちです。
そこでここでは、とりあえず指示されている内容をそのまま進める事が出来るようになる、程度の理解を目指して、コードを置く場所を幾つか考えてみます。

## 新規作成で出来るコードを改めて見直す

これまで、なんとなく新規作成された後になんとなくコードを挟んできましたが、ここらへんで新規作成された時のコードを見直してみるのも有用でしょう。

AndroidStudioで新規作成をすると、MainActivity.ktというファイルには以下のような感じで書いてあると思います。

```kotlin
import ...

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

ここで、onCreateというのが生成されている、というのに着目してください。これまではsetContentViewの行の下にfindViewByIdとかしてきて、
このonCreateという行はほとんど目に入れてなかったと思いますが、この辺からonCreateの中かどうか、というのが重要になってきます。

この最初に生成されるコードのどこに自分のコードを追加していくのか、というのが重要になってくるので、
このコードを良く見ておいてください。

### 最初のコードの三つの場所を意識する

先ほどのコードの中には、大きく三つのコードを追加する場所があり、そのうち二つしかこのシリーズでは使いません。
まず三つの場所を意識するのが大切です。

1番目は「class MainActivity」の行より上（3行目より上のスペース）、3番目はonCreateの中です。

```kotlin
import ...

//
//  1番目
//
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main) // 3番目、ここに改行入れて下の行に書く
    }
}
```

盲点になりがちなのが2番目です。2番目というのは、「class MainActivityの行」と、「override fun onCreateの行」の「間」になります。
最初に生成されるコードではここにはスペースが無いですが、以下のようにここに追加する事がこれからは出てきます。

```kotlin
import ...

class MainActivity : AppCompatActivity() {
    //
    // 2番目、ここは空行が無かったのでわかりにくいが、ここにいろいろ追加する事もある。
    //
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

つまり、最初に新規プロジェクトを作成して以下のコードが出てきたら、

```kotlin
import ...

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

以下のように考えられるのが大切です。

```kotlin
import ...

//
// 1番目の区画
//

class MainActivity : AppCompatActivity() {
    //
    // 2番目の区画
    //
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        //
        // 3番目の区画
        //
    }
}
```

以下、この区画の考え方について見ていきます

### 1番目の区画には何も書かない！

作業をしているのを見ていて一番良くある間違いが、1番目の区画に何かを書いてしまう事です。
この区画には、このシリーズでは当面は一切何も書きません。（data classまで行くとここに書く事になる）

間違えて書く事があまりにも多いので、現時点では「1番目の区画には何も書かない！」と強く覚えてしまうのがいいでしょう。

### 2番目の区画はメンバ変数とメンバ関数を書く

2番目の区画にはメンバ変数とメンバ関数を書きます。メンバ変数は[簡単な電卓を作ってみよう](simple_calc.md)でちょっとだけ出てきました。
現時点では

- 「メンバ変数としてAを定義する」と言われたら、二番目の区画の事だと分かる
- なんかクリックとかが終わった後も値を覚えておくのに使う

くらいに思っておけば十分です。

メンバ関数については現時点では何なのかは分からなくて良くて、例えば

**メンバ関数として以下をコピペしてください**

```kotlin
fun showMessage(msg: String) { Toast.makeText(this, msg, Toast.LENGTH_LONG).show() }
```

と言われたら、2番目の区画にこの行をコピペすれば良い、という事が分かれば十分です。

2番目の区画はメンバ変数とメンバ関数というものを置くところ、これが重要です。

### 3番目の区画に、これまでコード（findViewByIdして何かするコード）を書いていた

これまで3番目の区画しか出てこなかったのであまり意識してなかったところですが、
いろいろ出てくると「これまでやってたコードはそもそもどこに置いていたんだっけ？」と良くわからなくなる日がやがて来ます。
そこでこの辺でしっかりと見直しておくといい。

これまで、ボタンとかにsetOnClickListenerとかしていたのは「3番目の区画」です。

そしてこれは「onCreateの中」と呼びます。

ここまでは一箇所しか足す場所が無かったので、これを指示する事が無かったので「onCreate」というのはあまり聞き覚えが無いと思いますが、
ここからは「onCreateの中」というのは頻繁に出てくるので、

- 3番目の区画は「onCreateの中」と呼ぶ
- findViewByIdとかこれまで書いていたのは3番目の区画
- 何か最初に実行したいコードはonCreateの中（後述）

くらいを理解しておくと良いでしょう。

### 何か最初に実行したい時は3番目の区画

このサイトの「forループ入門」とか「List入門」などは、全てmainの中、

```kotlin
fun main() {

}
```

で実行していると思います。一方でAndroidではこれに相当するものが無くて、
例えば「メンバ変数にMutableListを作って、100個くらいアイテムを入れてください」と言われた時に、
どこにfor文を書いたらいいかわかりにくいと思います。

こういうのは、「onCreateの中」、つまり、3番目の区画になります。

メンバ変数を定義するのは2番目の区画、値を初期化するのは3番目の区画なので注意が必要です。

以下のようになります。

```kotlin
import ...
// 1番目の区画には何も書く事は無い
class MainActivity : AppCompatActivity() {
    //
    // 2番目の区画にメンバ変数を定義
    val mlist = mutableListOf<String>()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        //
        // 3番目の区画でfor文とか回して値を詰める
        //
        for(i in 1..<100) {
          mlist.add("${i}番目のアイテム")
        }

    }
}
```

{% capture comment1 %}
**そのほかにも書けそうな場所あるけど？**  
さて、三番目の区画には、二行良くわからない行があります。以下の二行です。

```kotlin
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
```

この間とかには書かないのか？という疑問が生まれる人もいるかもしれません。
ようするに以下の二箇所です。

```kotlin
    override fun onCreate(savedInstanceState: Bundle?) {
        // 
        // こことか
        //
        super.onCreate(savedInstanceState)
        //
        // ここにも書いたりしないの？
        //
        setContentView(R.layout.activity_main)
    }
```

それぞれここに置いた方がいいシチュエーションも無くは無いのですが、かなりレアだしこのシリーズではたぶんそういうケースは出てこないので、
この時点では「これらの場所にはコードは書かない」とおぼえてしまって良いでしょう。

また、onCreateの後、具体的には以下には書かないのか？という疑問も出てくるかもしれません。

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
    //
    // ここ
    // 
}
```

ここはプログラム言語としては2番目の区画と同じ意味になりますが、現時点ではここにも「書かない」と思っておいていいでしょう。
こちらはこのシリーズの後半では書く事も出てきますが、そこまで行く頃にはもっとこの辺の事情にも詳しくなっているはずなので、混乱する事も無いでしょう。

それまでは、「ここにはコードは書かないもの」とおぼえてしまってOKです。
{% endcapture %}
{% include myquote.html body=comment1 %}

## setOnClickListenerとgetViewの中

さて、新規に作られるところには3つの区画があり、そのうち1番目の区画は使わない、残り二つの区画を使う、という話をしました。

ですがさらにもう一つ、最初の時点では存在しない第四の区画があります。
それがsetOnClickListenerとgetViewの中です。getViewに関しては「ListViewに挑戦」で出てきます。

まずはsetOnClickListenerの中、を見てみましょう。

これまで3番目の区画に以下のようなコードを書いてきました。

```kotlin
import ...
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        findViewById<Button>(R.id.button1).setOnClickListener { findViewById<TextView>(R.id.label1).text = "ほげほげ" }
    }
}
```

この、setOnClickListenerの後の中括弧の中も別の区画になります。これを4番目の区画、と呼ぶ事にしましょう。

このように改行が無いと区画がわかりにくいので、以下のように改行を入れます。

```kotlin
import ...
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        findViewById<Button>(R.id.button1).setOnClickListener { 
          //
          // この中が4番目の区画
          //
          findViewById<TextView>(R.id.label1).text = "ほげほげ"
        }
    }
}
```

4番目の区画は呼び名だけ覚えておけば現時点ではいいでしょう。
これは「Buttonのオンクリックリスナーの中」と呼びます。

なお、次のListViewで出てくるgetViewは以下のようなコードになります。

```kotlin
class MainActivity : AppCompatActivity() {
  //
  // ここは2番目の区画
  //
  val adapter by lazy { object: ArrayAdapter<String>(this, R.layout.list_item, listData) {
            override fun getView(position: Int, convertView: View?, parent: ViewGroup): View {
              //
              // ここが4番目の区画
              //
            }
        }
    }

  override fun onCreate(savedInstanceState: Bundle?) {
      super.onCreate(savedInstanceState)
      setContentView(R.layout.activity_main)
      //
      // ここは3番目の区画
      //
  }
}  
```

このケース、lazyのあたりにいろんな中括弧が出てきてややこしいですが、「adapterのgetViewの中」と言ったら上記の4番目の区画な事が分かるといいと思う。
これは次の「ListViewに挑戦」をやった時に見直してみてくれ。

## まとめ

- 3つの区画+4番目の区画がある
- 1つめの区画は使わない
- 2つめの区画はメンバ変数とメンバ関数というのを置く
- 3つめの区画はこれまでコードを置いていたところで、以後「onCreateの中」と呼ぶ
  - listに最初に値をつめたりはここでやる
- 4つめの区画はsetOnClickListenerとgetViewの中