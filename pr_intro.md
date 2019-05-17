---
title: "gitのPR入門"
layout: page
---

gitは柔軟な使い方が出来る反面、特定のタスクに対してやり方がたくさんある為、
どのように進めていくのかが分かりにくい部分があります。

そこでここでは、kotlin-lessonでPRを送るタイプの課題をこなす時に、
とりあえずこうやっておけばOK、というやり方を提示します。

逆にそういう意図で書くので、一番良いやり方を解説している訳では無く、覚える事が少なくてまぁまぁ悪くない、くらいのやり方になってます。
ここに書いてある事に従う必要はありませんが、何も分からない人はとりあえずこうやってみてください。

また、ちょっと難しいので最初の一回目はここまで理解せずに適当なPR送ってもOKです。
少し慣れてきた人向けのちゃんとした運用の解説になります。

## あらすじ

karino2のkotlitexに、harukawaが新しい機能、demoappを追加する、という場合を考えます。
最終的にはkarino2のkotlitexが引き取ってメンテを続ける、という形を想定します。

### おすすめのツール

SourceTreeを推奨します。コマンドラインはかったるいので。
ユーザー登録が必要になったり要らなくなったりするので今がどうなってるかは知りませんが、無料で使えるのは間違いない。

あと気持ち重い気もする。

ただUIは良く出来ているのでこれが良い。
なお、いざとなったらコマンドラインに素直に降りる、という姿勢で使うのが良いです。

### レポジトリとブランチの構成

ここでは三つのレポジトリが登場します。

1. karino2のgithubのkotlitex
2. harukawaのgithubのkotlitex
3. harukawaのローカルのkotlitex

2はgithubのweb上で、karino2のkotlitexからforkというボタンを押して作ります。

ブランチとしては、ここで考えるべきは5つあります。

1. karino2のgithubのmaster
2. harukawaのgithubのmaster
3. harukawaのローカルのmaster
4. harukawaのローカルのdemoapp
5. harukawaのgithubのdemoapp

まず大切な事。harukawaのgithubのmasterには、自分の変更はpushしてはいけません。
間違ってやってしまわないように、「harukawaのローカルのmaster」のリモートは、「karino2のgithubのmaster」にしておいてください。

（TODO: SourceTreeでのやり方を調べてこの解説を直してPRください）

そして「harukawaのローカルのmasterを、定期的にkarino2のgithubのmasterと一致させる」ようにします。
これは「karino2のgithubのmasterをharukawaのローカルのmasterにpullする」という事で行います。

### 作業の進め方

さぁ、demoappを作りましょう。
とりあえずの進め方としては、

1. karino2のgithubのmasterをharukawaのローカルのmasterにpull
2. demoapp_workとかいうブランチをローカルのマスターから作る
3. これにcommitしていく
4. たまにキリが良い所でharukawaのgithubにpushする（一日一回とか二回とかの頻度）

という感じにしましょう。demoapp_workというブランチにするかdemoappにするかは最後まで読んだ後で自分で判断してください。

さて、demoapp_workにはいろいろな試行錯誤の元に、汚い履歴が残る事になります。

そこで次にPR用のブランチを作る事になります。

## PRを作る

さて、一通り実装を試して目的の機能が作れました。
あとはこれからPRを作ります。

### karino2のmasterの最新版に合わせる

PRを作る時はいつもここから始めます。

1. karino2のmasterをharukawaのローカルのmasterにpullする
2. harukawaのdemoapp_workをharukawaのmasterにrebase
   - harukawaのdemoapp_workをチェックアウト
   - harukawaのmasterを右クリックしてrebaseを選ぶ

### 簡単な変更の場合

demoapp_workをそのままPRにします。この場合はdemoapp_workは最初からdemoappにしておく方が良いですが、別にこの辺の名前はどうでもいいです。

### 履歴の整理が必要な場合

ある程度複雑な物を作っている場合は、普通変な試行錯誤などが入ってしまいます。
PRにはそういう物は基本的には含めない。
そこで整理された履歴を持つPR用のブランチを作ります。
これをdemoappと呼びます（PRに出すブランチの名前になります）。

demoappをmasterから作るかdemoapp_workから作るかは、楽な方でいいです。

demoap_workから作ったら、だいたい[レビューしやすいコミット履歴でバグ削減](https://moneyforward.com/engineers_blog/2015/11/30/reviewable-commit-log/)に書いてあるやり方でコミットを整理します。

これでの整理が凄く大変な場合はmasterからdemoappを作り、
適当にdemoapp_workの履歴から必要な所をコピペしたりして作っていきます。

このやり方でポイントとなるのは、demoapp_workはいじらない、という事です。
ちゃんとうまくいった履歴は試行錯誤も含めてこちらに残しておく。
それを消したりしてなんかしらないけど動かなくなった、みたいな事が無いように。

うまく行かなかったのだけど残しておきたいコードのコメントとかもdemoapp_workの方に残しておく。PRには含めない。

### ローカルのブランチからPRを作る手順

ローカルにPR候補のブランチが出来たら、次にPRを作ります。

1. PR候補のブランチをharukawaのgithubにプッシュ
2. harukawaのgithubのレポジトリからcreate pull request的なメニューを選ぶ（TODO: ここ実際の画面の表記に合わせて直してPRください）

これでPRが送れます。

### 指摘された変更を反映する

PRを出すと私があーだこーだ指示を出します。
だいたいはそんな大きい物じゃないと思うので、ローカルのPR候補のブランチに変更を追加でcommitしてharukawaのgithubにpushしてください。
そうすれば自動的にPRのページにも反映されます。

### PRがマージされたら

- PRのページに「delete XXX branch」みたいなリンクが出るのでそれをぽちっとしてgithubのブランチを削除
- karino2のmasterをharukawaのmasterにpull
- haruakwaのPR用ブランチの先にmasterが出来ているのをsource treeで確認
- harukawaのローカルのPR用ブランチを削除
- harukawaのローカルとリモートのdemoapp_workを、分かりやすいプレフィクスをつけてrenameし、しばらく放置してそのうち消す（私はgomi_demoappとかtmp_demoappとかにしがち）

最後は消しちゃってもいいんですが、PR用のブランチと違いwork用のブランチは消すと履歴が無くなっちゃうので、なんとなくしばらくは残します。
ただrenameは絶対してください。すぐに意味が分からなくなるので。

また、mergeされたPRのブランチは毎回削除してください。
これは正しく手順に従っていればPRのブランチの先にmasterが出来るはずなので削除しても安全なはずです。