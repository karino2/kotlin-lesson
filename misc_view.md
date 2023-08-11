---
title: "EditText、チェックボックス、スイッチなどを使ってみる"
layout: page
---

TextViewとButton 10回の修行を終えたら、次は他のViewも幾つか使ってみる。

## EditText入門

<iframe width="560" height="315" src="https://www.youtube.com/embed/4MGwDtuVH7Q" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

とりあえずこの動画の通り繰り返してください。コードだけ貼っておきます。

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        findViewById<Button>(R.id.buttonSubmit).setOnClickListener {
            findViewById<TextView>(R.id.label1).text = findViewById<EditText>(R.id.edit1).text
        }

        findViewById<Button>(R.id.buttonClear).setOnClickListener {
            findViewById<TextView>(R.id.label1).text = "むぇ〜〜"
        }
    }
}
```
