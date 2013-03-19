---
layout: page
title: liuzhe哲的笔记
tagline: Supporting tagline
---
{% include JB/setup %}


<ul class="posts">
  {% for post in site.posts %}
	<h2 style="font-weight:bold;">学习</h2> 
	<br />
	{% if post.category == '学习' %}
    	<li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
	{% endif %}
  {% endfor %}
	<br />
	<br />
   {% for post in site.posts %}
	<h4>闲</h4> 
	<br />
	{% if post.category == '闲' %}
    	<li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
	{% endif %}
	{% endfor %}
</ul>
