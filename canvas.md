---
title: "Canvasでいろいろ描画"
layout: page
---
カスタムビューはonDrawで画面を描きます。
そしてonDrawではCanvasというもののメソッドを呼び出して図形や画像を描きます。
だから最終的にはCanvasをマスターするのがカスタムビューをマスターする、という事になります。

ここでは、四角を描く以外の、良く使うCanvasの機能を見ていきましょう。

英語ですが、公式リファレンスはこちら。[Canvas  -  Android Developers](https://developer.android.com/reference/android/graphics/Canvas)

## 四角を描くdrawRect

[はろーCustomView](hello_customview.html)では以下のように、drawRectを使いました。以下の3の所ですね。

```kotlin
    override fun onDraw(canvas: Canvas) {
        // 1はここ
        val r = Rect(10, 10, 200, 200)

        // 2はここから
        val p = Paint()
        p.color = Color.RED
        p.style = Paint.Style.FILL
        // ここまで

        // 3はここ
        canvas.drawRect(r, p)
    }
```

このように四角を描くのはdrawRectです。

CanvasのdrawRectには、以下の３つがあります。

- Rectを指定する `drawRect(r: Rect, p: Paint)`
- RectFを指定する `drawRect(rf: RectF, p: Paint)`
- Floatで座標を指定する `drawRect(left: Float, top: Float, right: Float, bottom:Float, p: Paint)`

公式リファレンスはこちら（英語）＞[Canvas#drawRect](https://developer.android.com/reference/kotlin/android/graphics/Canvas#drawRect(float,%20float,%20float,%20float,%20android.graphics.Paint))

### FloatとRectF

図形の描画は、だいたい座標をInt（整数）で指定するものと、小数ありのFloatで指定するものの２つがあります。
両者はだいたい同じ挙動です。
小数も許すものの方が端の処理がより正確です。

Kotlinでは小数は今まではDoubleを使ってきたし、普通はDoubleを使いますが、グラフィックス周辺だけはFloatを使います。
普通、`0.0`と書くとKotlinではDoubleとなってしまうので、Floatの0だよ、と示すためには最後にFをつけます。`0.0F`とやるとFloatになります。

Floatについての詳細は、以下の[数値 - Kotlin 日本語リファレンス](https://karino2.github.io/kotlin-web-site-ja/docs/numbers.html)の「浮動小数点型」を参照ください。

さて、drawRectでは整数で作ったRectというのを渡していました。
Rectというのは上記のコードの以下の部分ですね。

```kotlin
  val r = Rect(10, 10, 200, 200)
```

これは四角のうち

1. 左
2. 上
3. 右
4. 下

の位置を指定しているのでした。

このRectと同じ意味だけれど位置の指定がFloatであるRectFというものも存在します。

```kotlin
  val rf = RectF(10.0F, 10.0F, 200.0F, 200.0F)
```

このように作ったrfを渡してもdrawRectを呼ぶ事ができます。

```kotlin
  canvas.drawRect(rf, p)
```

## 円を描くdrawCircle

円はdrawCircleで描きます。

```kotlin
    override fun onDraw(canvas: Canvas) {
        val p = Paint()
        p.color = Color.RED
        p.style = Paint.Style.FILL

        canvas.drawCircle(5.0F, 10,0F, 100.0F, p)
    }
```

drawCircleの所を抜き出すと以下のようになります。

```kotlin
canvas.drawCircle(5.0F, 10,0F, 100.0F, p)
```

引数は

1. 中心のx座標（この場合は5.0F）
2. 中心のy座標
3. 半径
4. Paintオブジェクト

となっています。
これで、左から5.0Fピクセル、上から10.0Fピクセルの位置に、半径100.0Fピクセルの円を描きます。

公式リファレンス（英語）はこちら＞[Canvas#drawCircle](https://developer.android.com/reference/kotlin/android/graphics/Canvas#drawcircle)

## drawTextとテキスト関連のPaintの設定

テキストを描くのはdrawTextです。
ただdrawTextは位置と書く文字を指定するくらいで、サイズや色はPaintオブジェクトで設定します。

例えば以下のような感じです。

```kotlin
    override fun onDraw(canvas: Canvas) {
        val paint = Paint()
        paint.color = Color.BLUE
        paint.textSize = 100.0F

        canvas.drawText("ほげほげ", 70.0F, 500.0F, paint)
    }
```

まずはdrawTextの行を見てみましょう。

```kotlin
canvas.drawText("ほげほげ", 70.0F, 500.0F, paint)
```

drawTextの引数は順番に

1. 文字列
2. x座標（テキストを書き始める位置）
3. y座標（テキストを書き始める位置）
4. Paintオブジェクト

となっています。2番目と3番目の引数で描き始めの位置を指定する訳ですね。
文字のサイズや色は4番目のPaintオブジェクトで指定しています。

### テキストの大きさと色はPaintで指定

色と文字の大きさを指定しているのは以下の部分です。

```kotlin
    val paint = Paint()
    paint.color = Color.BLUE
    paint.textSize = 100.0F
```

色はdrawRectの時と一緒なので説明は省きます。
大きさは`textSize`というもので指定しているのが分かります。

Canvasのテキスト描画はそんなに高機能では無いので、簡単な事だけに留めておくのが無難です。

公式リファレンス（英語）はこちら＞[Canvas#drawText](https://developer.android.com/reference/kotlin/android/graphics/Canvas#drawtext)

## 画像を描くdrawBitmapとBitmapのロード

あとで書く