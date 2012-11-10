---
layout: post
title: python抓取rss
tags: python,django
category: 学习
---


马伟伟同学说过想做一个rss的采集，这几天找出时间，看feedparser这个rss的抓取模块

前几天了解feedparser抓回来的字典树，树比较大，先做个简单的，title，link，简短的content，和时间，分别用到  字典树中的，entries[i].title,entries[i].link,entries[i].summary,entries[i].published。

今天做的是将 抓回来的 数据存入数据库，然后用python的django框架将数据展示出来，直接拿bootsrap的一个example做首页（也只打算先做这一个页），展示的部分比较简单的做好le。剩下的就是将抓取的数据存入数据库，数据库用的sqlite3（麻雀虽小，五脏俱全），做测试方便。feedparser抓取信息，向数据库中存时遇到问题，我用for循环来存数据，涉及到sql语句中出现变量，就使用python中字符串中的变量情况来凑sql语句（insert into xxx values(xxx,'%s'....) % (a,..)),一直error，google啊，最后还是在python的官方文档中看到了正确的写法:

purchases = [a,b,c,d,e] #abcde是变量
c.executemany('INSERT INTO stocks VALUES (?,?,?,?,?)', purchases)

在ipython中通过循环执行sql语句，显示数据已存入数据库，但在实际文件中，并没有数据，可能是先缓存着，还没有写入真实文件，到时间啦，明天再看。。。
