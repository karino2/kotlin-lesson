---
title: "ファイルをとりあえず使うだけの入門"
layout: page
---
テキストファイルに読み書きします。
けれどAndroidのファイル周りは複雑なので、まずは私が書いたコードを使う事にし、あとでStorage Access Frameworkを使う所までいったら真面目に学ぶ事にします。

という事で私の書いたTextFileLibを使う手順を。

なお、TextFileLibはgetExternalStoragePublicDirectoryを使っていて、これは非推奨なので将来のスマホでは動かないかもしれません。

## TextFileLibを使う手順

とりあえず手順を書いておくので、何も考えずに何度もやって覚えてください。

### AndroidManifest.xmlに以下を書く

```
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" android:maxSdkVersion="28" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" android:maxSdkVersion="28"/>
```

### TextFileLibというkotlinファイルを追加

app＞java＞自分のパッケージ を開いてい、右クリックして New＞Kotlin Class/File を選ぶ。

次にObjectを選び、名前にTextFileLibと入れてEnter。

### 以下をTextFileLibにコピペする

生成されたコードを削除して、以下をコピペする。

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