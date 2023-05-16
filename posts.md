---
layout: page
title: Posts
---

<!-- ## Posts -->

<!-- 
<ul>
    {% for post in site.posts %}
        <li>{{ post.date | date:"%b. %Y" }}: <a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>
-->

<ul>
    {% for post in site.posts %}
        <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>