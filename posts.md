---
layout: page
title: Posts
---

## Misc posts and media

{% for post in site.posts %}
  <ul><li>{{ post.date | date:"%b. %Y" }}: <a href="{{ post.url }}">{{ post.title }}</a></li></ul>
{% endfor %}

