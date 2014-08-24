---
layout: post
title: python使用google天气api
tags: [python]
category: blog
---

昨天开始利用google的天气api写一个天气预报的小应用，现在部署在sae上。定时向指定的邮箱发送天气预报邮件,配合139邮箱就能使用手机接收。

说说期间遇到的问题：

由于google天气返回的是xml格式的数据在weather后加上地址，hl=zh_cn 表示返回的xml data值为中文。

所以使用python解析xml的模块，python较多的xml的解析模块。开始因为xml读取的字符编码问题 用了几个模块，初始化报错:

```python
（xml 字符串得到的方式: xml = urllib2.urlopen(url).read())
```

后来用beautifulsoup这个模块解析不报错，官方文档上说它先把数据转成utf-8的编码格式。顺利完成。

刚开始也认为应该进行转码（获得的字符串是accii的），转为unicode时认为google返回的xml应该是utf-8的，但`xml.decode('utf-8')`一直报错，万没想到的是居然是gb2312的:


```python
     xml.decode('gb2312').encode('urf-8')
    #解决
```

具体解析xml代码：
https://github.com/liuzhe0223/code/blob/master/python/google_weather.py

