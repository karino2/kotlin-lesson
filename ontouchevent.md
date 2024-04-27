---
title: "onTouchEventでタッチを処理"
layout: page
---
ここではカスタムビューでタッチを処理する方法を学びます。

カスタムビューでタッチした座標を取得するには、onTouchEventを使います。
簡単にonTouchEventの使い方を知るために、
タッチされた場所に四角を描く（移動する）、というアプリを作ってみましょう。

## 前準備

まず、[はろーCustomView](hello_customview.html)でやったように、カスタムビューを作って四角を書く所まではやったとします。
とりあえず、以下でやっておきましょう。

- アプリ名： HelloTouch
- カスタムビューの名前: HelloTouchView

## onTouchEventをオーバーライドする

onDrawの上あたりで「onTouchEve」くらいまでタイプして、一覧からonTouchEventを選ぶ。
するとこうなるはずです。

```kotlin
    override fun onTouchEvent(event: MotionEvent?): Boolean {
        return super.onTouchEvent(event)
    }
```

ちなみにMotionEventがnullの事はないので、MotionEventのあとのはてなマークは削除しましょう。
つまりこう変更します。

```kotlin
    override fun onTouchEvent(event: MotionEvent): Boolean {
        return super.onTouchEvent(event)
    }
```

タッチに関わる処理が行われた時に、この関数が呼ばれます。
このMotionEventという引数にタッチに関する情報が入っています。

## MotionEventでとりあえずしっておくべきもの

MotionEventはいろいろな情報を持っていますが、一番最初は以下の2つだけ知っていればいいでしょう。

- xとy
- action

xとyはタッチされた座標が入っています。ただし良く使う整数のIntでは無く、Floatという小数が使える型で入っています。
あまり深く考えず、「`toInt()`を呼んで使う」と思っておけばいいでしょう。

actionはそのイベントの種類です。具体的には以下の３つを知っておけば十分でしょう。

- MotionEvent.ACTION_DOWN
- MotionEvent.ACTION_UP
- MotionEvent.ACTION_MOVE

ACTION_MOVEだけ補足しておくと、タッチしたまま指を画面から離さずドラッグした場合などにこれが来ます。

## onTouchEventの例とreturn

とりあえずマウスをタッチされた点を覚えておく、という例だと以下になります。

```kotlin
    var posX = 0
    var posY = 0

    override fun onTouchEvent(event: MotionEvent): Boolean {
        if (event.action == MotionEvent.ACTION_DOWN) {
            posX = event.x.toInt()
            posY = event.y.toInt()
            return true // 処理した場合はtrueを返す約束
        }
        return super.onTouchEvent(event) // 処理しなかった場合はsuper.onTouchEventを呼び出して結果を返す約束
    }
```

このように`event.action`や`event.x`などを使って処理をします。

onTouchEvent関数は、そのイベントの処理を行ったらtrueを返し、それ以外はsupoerのonTouchEventを呼んで結果を返す約束となっています。
falseを返すと何が起こるのか、などは、自分でレイアウトを作るなどのより進んだトピックでないと出てこないので、
この時点ではあまり考えないで

- 自分で処理したら`return true`
- 処理しなかったら `return super.onTouchEvent(event)`

とするようにしましょう。

ではこのposXとposYを使って、タッチした位置に四角を描いてみます。

## 課題： タッチした位置に四角を描いてみよう

[はろーCustomView](hello_customview.html)では、以下のように四角を描いていました。

```kotlin
    override fun onDraw(canvas: Canvas) {
        val r = Rect(10, 10, 200, 200)
        val p = Paint()
        p.color = Color.RED
        p.style = Paint.Style.FILL
        canvas.drawRect(r, p)
    }
```

ここでは先程onTouchEventで保存したposXとposYから、幅200、高さ100の四角を描いてみましょう。


{% capture draw-rect-on-touchpos-hint %}
onDrawを描くだけでは四角は移動しません。それはタッチをしてもonDrawは呼ばれないからです。
タッチの都度onDrawを呼んでもらうためにはinvalidateを呼ぶ必要があります。
{% endcapture %}
{% include collapse_quote.html body=draw-rect-on-touchpos-hint title="ヒント" %}

## 課題: タッチした位置にちょっとずつ四角が近づいていくようにしよう（難しい）

前回の[invalidateで四角を動かそう](invalidate_animation.html)の知識と合わせて、タッチした点にちょっとずつ近づいていくコードを書きましょう。
まず現在の四角の位置と最後にタッチされた位置を別々に覚えておきます。
そして毎フレームごとに四角の位置をちょっとずつタッチされた位置に近づけていきます。

近づける方法は、高校数学のベクトルが分かるのならそれで普通に書いてもらっってOKです。

数学とかわからんがな、という人は、

- 四角の位置のXとタッチの位置のXを比較
   - タッチが右なら四角の位置を1px右にずらす
   - タッチが左なら四角の位置を1px左にずらす
- 四角の位置のYとタッチの位置のYを比較
   - Xと同様に処理

という感じでいいでしょう。

{% capture comment1 %}
**数学を使って遠いと早くしたり、最初は遅くだんだん早くしたり**  
ちなみに数学が得意なら、タッチした点と四角が遠いと早く、近くではゆっくり近づくようにしたり、
加速度を持ってだんだん早くなっていく感じの移動なども書けます。初代マリオのBダッシュなどはだんだん早くなりますね。

距離というのはだいたい以下のように書けます。

```kotlin
val kyori = sqrt((touchX-rectX)* (touchX-rectX)+(touchY-rectY)* (touchY-rectY))
```

このkyoriが遠いとpxをたくさん動かして、近いとちょっとだけ動かすようにします。
単純にkyoriに適当な大きさを掛けるとkyoriに応じて速度を変えられますが、
ぎゅいーんという感じにしたければ、kyoriの二乗、つまりkyori*kyoriを適当に何倍かした値を使う方がいいかもしれません。

この辺は数学が得意な人に手伝ってもらってもいいでしょう。
{% endcapture %}
{% include myquote.html body=comment1 %}


