---
title: "ファイルをとりあえず使うだけの入門"
layout: page
---
テキストファイルに読み書きします。
けれどAndroidのファイル周りは複雑なので、まずは私が書いたコード（TextFileLibと呼ぶ）を使う事にし、あとでStorage Access Frameworkを使う所までいったら真面目に学ぶ事にします。

## TextFileLibを使う手順

という事で私の書いたTextFileLibを使う手順を。

なお、TextFileLibはgetExternalStoragePublicDirectoryを使っていて、これは非推奨なので将来のスマホでは動かないかもしれません。

これは意味を理解するというよりは手順を覚える事が大半なので、まずは手順を覚えてください。

### AndroidManifest.xmlに以下を書く

```
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" android:maxSdkVersion="28" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" android:maxSdkVersion="28"/>
```

### TextFileLibというkotlinファイルを追加

app＞java＞自分のパッケージ を開いてい、右クリックして New＞Kotlin Class/File を選ぶ。

次にObjectを選び、名前にTextFileLibと入れてEnter。

### 以下をTextFileLibにコピペする

生成されたコードを削除して（一番上のpackageの行だけ残す）、以下をコピペする。

```kotlin
import android.os.Environment
import java.io.BufferedReader
import java.io.BufferedWriter
import java.io.File
import java.io.FileReader
import java.io.FileWriter
import java.io.IOException

object TextFileLib {
    @Throws(IOException::class)
    fun ensureDirExist(dir: File) {
        if (!dir.exists()) {
            if (!dir.mkdir()) {
                throw IOException()
            }
        }
    }
    private fun fileAtDefaultDir(fileName: String): File {
        val path = Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_DOCUMENTS)
        ensureDirExist(path)
        return File(path, fileName)
    }

    fun isFileExist(fileName: String) : Boolean {
        return try {
            val file = fileAtDefaultDir(fileName)
            file.isFile
        } catch(e: IOException) {
            false
        }
    }


    fun writeText(fileName: String, body: String) : Boolean {
        return try {
            val file = fileAtDefaultDir(fileName)
            BufferedWriter(FileWriter(file)).use {
                it.write(body)
            }
            true
        }
        catch (e: IOException) {
            false
        }
    }

    /*
        ファイルが無い時は""を返す
     */
    fun readText(fileName: String) : String {
        return try {
            val file = fileAtDefaultDir(fileName)
            if (!file.isFile) return ""
            BufferedReader(FileReader(file)).use {
                it.readText()
            }
        }
        catch(e: IOException) {
            ""
        }

    }
}
```

### 動作確認のコードを書く

いつも通り適当にボタンを置いて、以下のようなコードを書く

```kotlin
  findViewById<Button>(R.id.buttonWrite).setOnClickListener {
      TextFileLib.writeText("hogehoge.txt", "これはテストです。\nテスト・テスト")
      showMessage(TextFileLib.isFileExist("hogehoge.txt").toString())
  }

  findViewById<Button>(R.id.buttonRead).setOnClickListener {
      showMessage(TextFileLib.readText("hogehoge.txt"))
  }
```

Android 10以上のスマホならこれで動くはずです。
Android 9以下だとこれでは、writeTextが失敗して何も書けないはずです。

### 権限を付与(Android９以下のスマホ)

インストールされてるアプリをランチャーから長押しして、情報＞権限 を選び、ストレージのスイッチをオンにする

これでもう一回起動して試すと今度は書けるはずです。

### 生成されたファイルを確認

適当なファイルマネージャーでDocumentsの下にファイルが作られているのを確認。

## TextFileLibの簡単な説明

手順を覚えたら、使い方を見ていきます。

TextFileLibはテキストファイルを読み書きするための簡易的なライブラリです。
ライブラリとは「みんな」が使いそうなコードを集めたものです。
「みんな」が誰を指すかは状況によって、その会社内だったらそのプロジェクト内だったり世界中の多くの人だったりします。

ライブラリは、最初のうちはコードの中身は気にせず使い方だけ調べて使っていくものです。

以下ではTextFileLibの使い方を簡単に見ていきます。

TextFileLibには三つの関数があります。

- `fun isFileExist(fileName: String) : Boolean`
- `fun writeText(fileName: String, body: String) : Boolean`
- `fun readText(fileName: String) : String`

それぞれを簡単に見ていきます。

### isFileExistでファイルが存在するかを調べる

isFileExistはある名前のファイルが存在するかを調べます。
以下のように使います

```kotlin
val v = TextFileLib.isFileExist("mytextfile.txt")
```

「mytextfile.txt」というファイルがあればtrueを、無ければfalseを返します。

### writeTextで文字列をファイルに書き込む

writeTextはファイルの名前と書き込む文字列の二つを引数にとります。

`fun writeText(fileName: String, body: String) : Boolean`

たとば以下のようにすると「mytextfile.txt」というファイルに文字列を書き込みます。

```kotlin
val text = "ほげほげいかいか"

TextFileLib.writeText("mytextfile.txt", text)
```

なお、ファイルを書き込むのは、権限によっては出来なかったりします。書き込めない時はfalseが返ってきます。

### readTextで指定したファイルの文字列を読み込む

readTextで指定したファイルの文字列を読み込んで、文字列として返します。

`fun readText(fileName: String) : String`

ファイルが存在しなかったり権限が足りなかったりテキストファイルでは無いファイルを開くと「""」を返します。
読めなかったのか空のファイルなのか、この関数では区別出来ないけれど、入門の段階ではだいたい読めるのでいいでしょう。

## 入力された文字をファイルに書き込むアプリを作ろう

ファイル名とテキスト入力の二つのEditTextとSaveというボタンを持ったレイアウトを作って、
処理を書こう。

これは何回か繰り返してください。あとで手順動画も作ります。