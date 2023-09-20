---
title: "サンドボックス"
layout: page
---

kotlin playgroundのサンプルです。

{% capture cod1 %}
class Contact(val id: Int, var email: String)

fun main(args: Array<String>) {
//sampleStart
    val contact = Contact(1, "mary@gmail.com")
    println(contact.id)
//sampleEnd
}
{% endcapture %}
{% include kotlin_quote.html body=cod1 %}
