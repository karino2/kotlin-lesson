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

公式リファレンスはこちら＞[Canvas#drawRect](https://developer.android.com/reference/android/graphics/Canvas#drawRect(float,%20float,%20float,%20float,%20android.graphics.Paint))

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

- 左
- 上
- 右
- 下

の位置を指定しているのでした。

このRectと同じ意味だけれど位置の指定がFloatであるRectFというものも存在します。

```kotlin
  val rf = RectF(10.0F, 10.0F, 200.0F, 200.0F)
```

このように作ったrfを渡してもdrawRectを呼ぶ事ができます。

```kotlin
  canvas.drawRect(rf, p)
```

続きはあとで書く
