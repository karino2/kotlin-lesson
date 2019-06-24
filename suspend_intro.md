---
title: "使うだけに絞ったsuspend関数入門"
layout: page
---


## はじめに

このkotlin lessonではhttpにはfuelを使ってもらっている。
だからコルーチンというかawaitResponseResultとかを使ってもらっている。

で、コルーチンの解説なんて死ぬほどあるだろうから適当にググって適当に書いてください、と言った所、みななんか全然分かってない感じのコードを書いてくる。

何故だ？解説とかあるだろ？と思ってググると、「確かにこれじゃ入門者は使えないな…」というのばかり。
という事で、あまり分かってない人がとりあえず使うのに必要となる事だけを解説してみます。

hello worldとかでは無くて、そういうのを動かした後に読む文書です。


### この文書を書こうと思ったきっかけ

こんなコードが来ました（多少解説の為に変更しています）。

詳細はいいとして、putContentInnnerはsuspend関数で中にawaitResponseResultなどを呼んでいる。
そしてそれをputContentという関数でラップしていて、この中でlaunchが呼ばれている。

```
class ContentSender {
    // ... 中略 ...

    private suspend fun putContentInner(apiUrl: String, branchName: String, fname: String,
     base64Content: String, accessToken:String) : Response {
        val (_, _, result) = "$apiUrl?ref=$branchName".httpGet()
            .header("Authorization" to "token ${accessToken}")
            .awaitResponseResult(Content.Deserializer(), Dispatchers.IO)

        val contParam = arrayListOfContentParameter(branchName, fname, base64Content)

        result.fold(
            { cont -> contParam.add("sha" to cont.sha) },
            { _ /* err */ -> {} }
        )


        val json = jsonBuilder {
            val obj = beginObject()
            contParam.map { (k, v) -> obj.name(k).value(v) }
        }

        val (_, resp, _) = apiUrl.httpPut()
            .body(json)
            .header("Authorization" to "token ${accessToken}")
            .header("Content-Type" to "application/json")
            .awaitStringResponseResult(scope = Dispatchers.IO)
        return resp
    }

    fun putContent(apiUrl: String, branchName: String, fname: String, base64Content: String, accessToken: String) {
        job = Job()
        launch {
            val resp = putContentInner(apiUrl, branchName, fname, base64Content, accessToken)

            val sendCode: Int = when(resp.statusCode) {
                200, 201 -> 2
                else -> 1
            }
            prefs.edit().putInt("success_post",sendCode).commit()
        }
    }
}
```

そしてこれをActivityから呼んでいた。

```
class MainActivity : AppCompatActivity() , CoroutineScope {
    // ... 中略 ...
    fun onClick() {
        val base64Content = readBase64(fileName)
        val sender = ContentSender(this)
        sender.putContent(apiUrl, "master", fileName, base64Content, accessToken)
        finish()
    }
```

これではputContentが全てを送信する前にfinishが呼ばれてしまうのでまずい。
これがなぜ悪いのか、どう直すべきなのか、を理解する程度の説明をするのがこの文書の目的です。

### この文書で解説する事、しない事

[公式の入門](https://kotlinlang.org/docs/reference/coroutines/coroutines-guide.html)に書いてあるような事は説明しません。
全体像なども解説はしません。
内部の仕組みも解説しません。

その代わり、呼び出しのフローがどうなって何がどこのスレッドで実行されるか、という事を理解して、
上記のコードがなぜ悪いのか、どう直すべきなのかを理解出来る程度を目指します。
良くある普通のケースでどうなるのか、を話すので、「dispatcherとか使えばそうじゃないケースも作れるよ」とかそういうのは書きません。

あくまで最初に見様見真似で使い始める時に必要な最低限の事だけ書きます。
ある程度使って慣れてきたら、ちゃんと内部の仕組みを理解するドキュメントに進んでください。

[菊田さんが挙げている文書](https://yoheikikuta.github.io/study_kotlin_coroutine/) でだいたいいいと思います。

解説する事をまとめると以下になります。

- suspend関数を利用者の視点で理解する（仕組みは知らん）
- suspend関数を自分で書いて公開出来るようにする（ただし中では他のsuspend関数呼ぶだけ）
- 呼び出し時にどこがどのスレッドで動くかを理解する

# suspend関数とはなにか

## 非同期な関数

### 非同期な関数とはどういう物か

### 非同期な関数には別のスレッドが前提とされている

### コールバックはどこのスレッドで呼ばれるか

## suspend関数は非同期な関数で使われる

### 関数の内部の実行スレッド

### 別のスレッド

### コードの分割とキュー



