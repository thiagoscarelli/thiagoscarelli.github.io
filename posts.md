---
layout: page
title: Posts
---

## Posts

<ul>
  {% for post in site.posts %}
    <li>{{ post.date | date:"%b. %Y" }}: <a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>


{% for post in site.posts %}
  <ul><li>{{ post.date | date:"%b. %Y" }}: <a href="{{ post.url }}">{{ post.title }}</a></li></ul>
{% endfor %}

