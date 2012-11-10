---
layout: post
title: pythonrss2
tags: python,django 
category: 学习
---

这两天去做acm省赛的志愿者，没有blog

继续上次的blog

上次之后继续看了几行python的官方文档，sql执行后应该执行一数据库链接对象的commit（）方法（假如链接对象为conn，conn.commit())。写好rss的抓取存储脚本rss.py,执行，查看数据库中有数据啦，甚是高兴，遂立即去刷页面，却发现数据库中取出的html格式的数据被原样打出，甚是不解啊，查看页面属性,看到对应的页面html源码处，取出数据两端有引号。


去查数据库中的文件，发现数据没有引号，猜想应该是django中模板解释时有错，去查看django模板的详细文档，看到模板解释时遇到html格式会转义

在i.comtent前后分别加上 autoescape off 和 endautoescape 标签，这样i.content中的html就不会被转义la:

页面刷新，漂亮的界面出来啦

等待完善rss.py，数据过滤等功能
