---
title: "Spanで文字を装飾"
layout: page
---
TextViewの一部だけ色を変えたり、下線を引いたり、打ち消し線をしたり、
太字にしたりする機能があります。
これらの装飾をするものをSpanと呼びます。

以下TextViewの文字を装飾してみますが、
自分で試す時は結果がわかりやすいようにTextViewのtextSizeは40spくらいにしておくといいでしょう。

## SpannableString入門

以下みたいな感じを説明する。

```kotlin
    val spannable = SpannableString("ほげほげいかいか")
    spannable.setSpan(StrikethroughSpan(), 2, 4, Spannable.SPAN_EXCLUSIVE_EXCLUSIVE )
    findViewById<TextView>(R.id.label1).text = spannable
```

## 公式のUIガイドへのリンク

[Spans  -  Android Developers](https://developer.android.com/develop/ui/views/text-and-emoji/spans)