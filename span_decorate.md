---
title: "Spanで文字を装飾"
layout: page
---
TextViewの一部だけ色を変えたり、下線を引いたり、打ち消し線をしたり、
太字にしたりする機能があります。
これらの装飾をするものをSpanと呼びます。

以下TextViewの文字を装飾してみますが、
自分で試す時は結果がわかりやすいようにTextViewのtextSizeは40spくらいにしておくといいでしょう。
プロジェクトの名前はHelloSpanとしましょうか。

## SpannableString入門

とりあえず以下のようなコードをいつものようにonCreateで試してみてください。

```kotlin
    val spannable = SpannableString("ほげほげいかいか")
    spannable.setSpan(StrikethroughSpan(), 2, 4, Spannable.SPAN_EXCLUSIVE_EXCLUSIVE )
    findViewById<TextView>(R.id.label1).text = spannable
```

最初の行はSpannableStringというものを作ります。
これは装飾の出来るテキストを表すものです。

次の行、

```kotlin
  spannable.setSpan(StrikethroughSpan(), 2, 4, Spannable.SPAN_EXCLUSIVE_EXCLUSIVE )
```

で、装飾を追加しています。これは打ち消し線（strike trhough）を2文字目から4文字目までの文字につける、という意味です。
最後の`Spannable.SPAN_EXCLUSIVE_EXCLUSIVE`は毎回打つものだ、と丸写ししてください。

こうして作ったspnnable　stringをいつものようにTextViewのtextにセットすると、装飾された文字が書かれます。

### 様々な修飾

以下の行のうち、

```kotlin
  spannable.setSpan(StrikethroughSpan(), 2, 4, Spannable.SPAN_EXCLUSIVE_EXCLUSIVE )
```

`StrikethoughSpan()`の所をいろいろ変える事で、装飾の内容を変えられます。
いろいろ下に並べるので試してみてください。

**文字の色を変える**

`ForegroundColorSpan(Color.RED)` で文字の色が赤になります。

**背景色を変える**

`BackgroundColorSpan(Color.GREEN)`

**太字にする**

`StyleSpan(Typeface.BOLD)`

これは日本語の文字だとちょっと違いがわかりにくいかも。

**下線を引く**

`UnderlineSpan()`

**一部だけ大きくする**

`RelativeSizeSpan(1.5f)`


## 公式のUIガイドへのリンク

[Spans  -  Android Developers](https://developer.android.com/develop/ui/views/text-and-emoji/spans)