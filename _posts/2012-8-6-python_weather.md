---
layout: post
title: python利用google天气api写天气预报
tags: [python]
category: 动手
---

昨天开始利用google的天气api写一个天气预报的小应用，现在部署在sae上，
定时向指定的邮箱发送天气预报邮件,配合139邮箱就能使用手机接收.
由于完成初步，参数都是写死的，也不留链接啦。

说说期间遇到的问题：

由于google天气返回的是xml格式的数据
在weather后加上地址就行啦，hl=zh_cn 表示返回的xml data值为中文

所以先找了python解析xml的模块，python有很多 ｘｍｌ的解析模块
开始因为 xml 读取的字符编码问题 用好几个模块，每次的初始化都报错
（xml 字符串得到的方式: xml = urllib2.urlopen(url).read())

后来用beautifulsoup这个模块解析不报错，官方文档上说它先把数据转成utf-8的编码格式

我刚开始也认为应该进行转码（获得的字符串是accii的），转为unicode时认为google
返回的xml应该是utf-8的，但 xml.decode('utf-8') 一直报错，万没想到的是 ta居然是
gb2312的，  xml.decode('gb2312').encode('urf-8')  , 解决，
具体解析ｘｍｌ代码：
https://github.com/liuzhe0223/code/blob/master/python/google_weather.py

