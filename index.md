---
layout: page
title: liuzhe哲的笔记
tagline: Supporting tagline
---
{% include JB/setup %}


<ul class="posts">
<h2 style="font-weight:bold;">学习</h2> 
  {% for post in site.posts %}
	<br />
	{% if post.category == '学习' %}
    	<li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
	{% endif %}
  {% endfor %}
	<br />
	<br />
<h2 style="font-weight:bold;">闲</h2> 
   {% for post in site.posts %}
	<br />
	{% if post.category == '闲' %}
    	<li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
	{% endif %}
{% endfor %}
</ul>
