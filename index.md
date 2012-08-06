---
layout: page
title: lidashuang
tagline: Thoughts, Writings and Dreams
---
{% include JB/setup %}

#### 最近发布的文章

<ul class="post">
{% for post in site.posts limit:10 %}
	    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
</ul>
