---
title: "ツアー副読本：関数"
layout: page
---

[関数（ツアー） - Kotlin 日本語リファレンス](https://karino2.github.io/kotlin-web-site-ja/docs/kotlin-tour-functions.html)を読んでいく時の補助教材です。

## 最初に読む時

初めてツアーをやる時は、ラムダ式の手前まで読んだら次のクラスに進む方がいいと思います。
クラスが終わってからラムダ式に戻ってきましょう。

単一式関数までは特に補足無しで読むだけで分かるでしょう。

## ラムダ式

ラムダ式の所は他の言語でラムダ式や関数オブジェクトを使った事が無い人には難解な説明かもしれません。

ラムダ式とは何か、という事について以下に動画を作りました。

<iframe width="560" height="315" src="https://www.youtube.com/embed/dtOIfBsLbAM?si=rMDcX0gnMm0hoG2Q" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### filterがなんか中括弧なんだけど？

「他の関数に渡す」の所で、例が以下のようなコードになっています。

```kotlin
val numbers = listOf(1, -2, 3, -4, 5, -6)
val positives = numbers.filter { x -> x > 0 }
val negatives = numbers.filter { x -> x < 0 }
println(positives)
// [1, 3, 5]
println(negatives)
// [-2, -4, -6]
```

filterの所、なんか`filter()`といいつつ呼ぶ時は`filter {}`になっててどういう事？
という感じがしますが、
これは後に出てくる「トレーリングラムダ」という省略記法です。

現時点では、このコードは以下と同じなので、以下が理解出来れば良いです。

{% capture kotlin-tour-lambda-filter %}
fun main() {
    //sampleStart
    val numbers = listOf(1, -2, 3, -4, 5, -6)
val positives = numbers.filter( { x -> x > 0 } )
val negatives = numbers.filter( { x -> x < 0 } )
    println(positives)
    // [1, 3, 5]
    println(negatives)
    // [-2, -4, -6]
    //sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=kotlin-tour-lambda-filter %}

これはnumbersのfilterというメソッドにラムダ式を渡しています。
ラムダ式は `{ x -> x > 0 }` と `{ x -> x < 0 }` の部分です。

この後に囲み記事で書いてある「もしラムダ式が唯一の関数のパラメータの場合〜」というのは、
この事を言っています。

次のmapの例も同じです。

### 「関数の型」のあたりがなんだかわからん

「関数の型」のあたりは解説がやや難しいかもしれない。