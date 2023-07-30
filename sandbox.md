---
title: "サンドボックス"
layout: page
---

kotlin playgroundのサンプルです。

{% capture cod1 %}
class Contact(val id: Int, var email: String)

fun main(args: Array<String>) {
    val contact = Contact(1, "mary@gmail.com")
    println(contact.id)
}
{% endcapture %}
{% include kotlin_quote.html body=cod1 %}
