---
layout: page
title: Posts
---

## Posts

{% for post in site.posts %}
  <ul><li>{{ post.date | date:"%b. %Y" }}: <a href="{{ post.url }}">{{ post.title }}</a></li></ul>
{% endfor %}

