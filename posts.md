---
layout: page
title: Posts
---

<ul>
	{% for post in site.posts %}
		<li style ="list-style-type: none;">
			{{ post.date | date:"%Y.%m.%d" }}: <a href = "{{ post.url | relative_url }}">{{ post.title }}</a>
		</li>
	{% endfor %}
</ul>

<!--
<h2>Categories</h2>

{% capture site_tags %}{% for tag in site.tags %}{{ tag | first }}{% unless forloop.last %},{% endunless %}{% endfor %}{% endcapture %}
{% assign tags_list = site_tags | split:',' | sort %}

<ul>
  {% for item in (0..site.tags.size) %}{% unless forloop.last %}
    {% capture this_word %}{{ tags_list[item] | strip_newlines }}{% endcapture %}
    <a href="./tags.html#{{ this_word }}"><span>{{ this_word }}</span></a> [<span>{{ site.tags[this_word].size }}</span>]
  {% endunless %}{% endfor %}
</ul>
