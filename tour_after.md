---
title: "ツアー副読本：ツアーを終えた後は"
layout: page
---
ツアーの後には[リファレンス](https://karino2.github.io/kotlin-web-site-ja/docs/classes.html)を読んでいきたい。
リファレンスは全部を読むよりは、重要な所だけを選んで読んでいきたいところ。
ただしリファレンスは少し難しいので、書籍をベースにしつつリファレンスで補う形が良いかもしれない。

## kotlinの学習の参考書

リファレンスとの間を埋める、という目的では、「やさしいKotlin入門」をオススメします。

<iframe sandbox="allow-popups allow-scripts allow-modals allow-forms allow-same-origin" style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="//rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=karino203-22&language=ja_JP&o=9&p=8&l=as4&m=amazon&f=ifr&ref=as_ss_li_til&asins=4877834273&linkId=cf60a372a31ff7b85d5ed9751f6d6e72"></iframe>

電子版が無いですが、初学者が読んでいけるような内容にはなっていると思う。
ただし後半の難度が上がる所から解説が減ってしまっているので、
後半はリファレンスと合わせて読んでいく感じが良いかもしれない。

## ツアーのあとに読みたいリファレンス

[リファレンス](https://karino2.github.io/kotlin-web-site-ja/docs/classes.html)の左のナビゲーションバーでどこを読みたいかを説明します。

読みたい所はかなり多くなってしまうので、優先度の高、中、低の三段階で示したいと思います。

### 優先度高

- 「クラスとオブジェクト」の下のうち
  - クラス
  - 継承
    - [クラスの継承](inheritance.md)に補足解説あり
  - インターフェース
  - object式とobject宣言
- Nullセーフティ

ツアーをやったらとりあえずここまでは進めて欲しい。
Android開発でもセンサーやマイクなどのデバイスの機能を使う前にこの辺を知っておくと、理解度がだいぶ違う。

この位理解していれば中級者ですね。

### 優先度中

- 「クラスとオブジェクト」の下のうち
  - プロパティ
  - 関数的インターフェース（SAM）
  - 拡張
  - 列挙型クラス
- 分解宣言
- スコープ関数

この辺はAndroid開発で必須では無いけれど、
kotlinの勉強という点では修めておきたい内容。便利だし。

クラスはかなり分量が多くなっているけれど、この辺はkotlinの勉強という点では重要なテーマだ。

### 優先度低

- 「クラスとオブジェクト」の下のうち「inline value classes」以外の残り全部
- 「関数など」の下のうちビルダー関連以外全て
  - 「レシーバ付き関数リテラル」のあたりは分からなかったら飛ばしてもOK
- 「標準ライブラリ＞コレクション」の下全部

優先度低は、最初の段階で全部読む必要は無くて、必要になったらその周辺だけ読む、
という感じでちょっとずつ増やしていくのがいいと思います。

Kotlin的にコードを書くにはコレクションのあたりは読みたい。

## Android開発の書籍

ここまではkotlinの勉強の話でしたが、Android側の書籍なども紹介しておきます。

Androidの入門として、このサイトを復習しつつ無い知識を補うには、「Androidアプリ開発の教科書」は順番にやっていけると思う。

<iframe sandbox="allow-popups allow-scripts allow-modals allow-forms allow-same-origin" style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="//rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=karino203-22&language=ja_JP&o=9&p=8&l=as4&m=amazon&f=ifr&ref=as_ss_li_til&asins=B0BN9VMS9M&linkId=c1fc30e8c4943203688beedac2d8c073"></iframe>

ただし後半は分からない所も残るとは思う。これは他の入門書もだいたいそういうものなので、サンプルを動かす程度で先に進むしかない。
典型的なAndroidの入門書という感じで、一冊くらいはそういうのをやっておいても良いとは思うし、
他も大差無いのでとりあえずなにかやってみるならこれで良いんじゃないか。

より辞書的に、必要な所だけを調べたり学んだりするには「はじめてのAndroidアプリ開発 Kotlin編」が良いと思う。

<iframe sandbox="allow-popups allow-scripts allow-modals allow-forms allow-same-origin" style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="//rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=karino203-22&language=ja_JP&o=9&p=8&l=as4&m=amazon&f=ifr&ref=as_ss_li_til&asins=B09MHF7F6N&linkId=98e05a5fd14b243fe2a1f9d356506903"></iframe>

こちらは順番にやっていくには分量が多いが、CustomViewなど割としっかり解説されていて、
入門者が手元に置いておいて公式リファレンスの代わりに使うには良いと思う。