---
title: "あらすじ"
layout: page
---

プログラム素人がAndroidアプリ開発に興味を持つ、というのは結構ありがちに思う。
けれど、kotlinの入門はどれも他の言語の経験を前提にしたものばかりで、こうした層に手頃なものが無い。

今ちょうどプログラム素人の友人にAndroidアプリ開発を教えていて、
それを補う話をちょこちょこ書いていこうと思っているので、
それをひとまずここに蓄積していって、そのうち入門サイトに仕上げられたらいいな、と思って始めて見る。

教えてる内容とだいたい同じような内容の動画をyoutubeに以下のプレイリストで上げる。

<iframe width="560" height="315" src="https://www.youtube.com/embed/videoseries?list=PL3J_mLcl4YCdi2bLHtynt7Ohni1_NQJiF" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

また、途中の段階からは、とりあえず[算数で挫折した人向けのJavaScript入門](https://karino2.github.io/js-introduction/)を6.3くらいまではやってもらった上で、
それを補う感じで書いていく。

対象はプログラム素人で、kotlinを教えるというよりはkotlinを題材にプログラムを教える、という感じにしたい。

また、あくまでAndroidのアプリ開発を前提にkotlinを教える。
サーバーサイドkotlinなどにも通じるように、という事はせずに、Androidを前提にする。

以前[公式のkotlin tour](https://kotlinlang.org/docs/kotlin-tour-welcome.html)をやってもらったところ、さっぱりわからん、
となったので、これと同じくらいの内容をちゃんと初心者向けに説明していくのを目標とする。

より具体的にはListViewとかでアプリを書くのに必用な程度のkotlin理解を目指す。

## 以下ベテラン向けの話

以下はこのコースをやる人というよりは、このテキストを使って教える人向けの話です。

このサイトでの説明は、少ない知識でいろんあアプリを自分で書けるようになる、というのを目標にしています。
だからもっといい書き方がある時も、より原始的な書き方で同じ事が出来る場合はそうしています。

例えばmapなどを使って簡潔に書ける時もforとvarを使って書くようにしています。
また、Null safetyなどの話はしません。
whenで書ける事もif elseで書いています。
文字列のListもjoinで済むところをvarで足したりしています。

Android側の話もなるべくLinearLayoutのみでレイアウトするようにしています。
RelativeLayoutやConstraintsLayoutの方が良いケースでもLinearLayoutに固執しています。

つまりkotlinを使った良いコードを書く書き方では無く、
なるべく覚える事を少なくやりたい事を表現出来るように、と考えています。

ある程度こうした書き方でいろいろ書いた後に、もっとエレガントな書き方に進む方が良い、と思っているのでこうしています。