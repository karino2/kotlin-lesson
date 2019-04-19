---
title: "トップページ"
layout: page
---

karino2の暇つぶしプログラム教室のkotlin編のページです。

[姉妹編のC言語編](https://karino2.github.io/c-lesson/)もあります。

kotlinの入門サイトくらいは読んだがそこから先が良くわからん、という人向けに、
暇つぶし程度に教える、という事をやるサイトです。

### あらすじ

オンラインで知らない人（知ってる人でもいいけど）にコードを書かせてレビューする、
というのは、やってみたらなかなか有意義な活動だな、と思った。
ただC言語編ではいまいち自分のメリットが無いからサステイナブルでは無いなぁ、と思った。
また、皆が結構労力をかけてなかなか凄い物を作るのに、みなが同じ物を作るのでもったいないなぁ、という気がした。

そこでAndroidでは、自分が実装しようと思っている物を幾つか実装してもらってレビューする、という事をやってみたい、と思ってこのサイトを作りました。

issueに実装候補をいろいろ上げます。
それをやる人は、issueでやると宣言してください。
そうしたら、どういう方針で進めるか、とかをその人の実力に応じて解説するので実装してみてください。

それをこちらはレビューする過程でkotlinとかAndroidについて、必要そうな事を教えていきます。

あくまで教えるのがメインで、題材にこれらを使う、くらいのつもりで考えています。
ただ出来上がった物は自分が使う気満々ではありますし、出来がいまいちなら自分が直して使うつもりです。

kotlinでのAndroid開発を勉強してみたい、という人向けなので、自力で出来なくても全然OK、というよりそういう人を対象と考えてます。
レベルに応じて割と詳細にやり方や関連知識は教えていきます。
教えるより普通にやる方が断然早い、みたいなのは気にしなくて良いです（C言語編の方を見たらどのくらいの労力までは掛けても良いと思ってるか伝わるかも？）。

kotlin的にはどう書くか、みたいなのを教えていけると思っています。

### 前提条件

- 言語はkotlinでやります。
- [公式の初めてのアプリの作成](https://developer.android.com/training/basics/firstapp) くらいはやってある事を前提とする。
    - [codelabのBuild Your First Android App in Kotlin](https://codelabs.developers.google.com/codelabs/build-your-first-android-app-kotlin/index.html)でもいいです。
- kotlinは[公式のリファレンス](https://kotlinlang.org/docs/reference/)をOtherまで読んでいれば理想ですが、うろおぼえだけど何か書いてみたい、くらいの段階まで言っていればコード見ながら指導します。
- githubはPR出来るのが望ましいが、分からない人は、せめて「github 初めてのPR」とかでググって読むくらいはやっておいて欲しい。そうしたらやり方を教えます。
- 辞めるのは自由ですが、辞める時はやめる、と言ってくれると嬉しい（続きを自分が作りたい場合があるので）
- 自分が直して使いたいので、独自アプリの場合はオープンソースライセンスの何かでお願いします。（わからなければMITライセンスで）
- 個人的好みのため、Fragmentは一切使いません。

### issueの例

菊田さんに実際にやってもらってる物を例に載せておきます。
といってもほとんどslackでやり取りしていたのであまり詳細は無いですが。

- [issue1: ギターのタブ譜読み練習アプリ](https://github.com/karino2/kotlin-lesson/issues/1)


### やり方

[issueの一覧](https://github.com/karino2/kotlin-lesson/issues/)からやりたいのを選んで、「やります」とコメントを書いてください。
そうしたら詳細を教えます。
やる前に何か質問したい、とかもご自由にどうぞ。

指導などは[![kotlin-lesson@gitter](https://badges.gitter.im/karino2_program_lesson/kotlin-lesson.svg)](https://gitter.im/karino2_program_lesson/kotlin-lesson?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)でやってみようと思ってます。