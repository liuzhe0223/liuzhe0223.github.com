---
layout: post
title: angularjs使用笔记
tags: python, django
category: blog
---

高大上的angularjs听说很早啦，开始写一点的时间按是13年暑假在×××pod实习的时候，那时同事分享过，所以在一个项目中使用下（作为一个前端经验甚少的我写的这个东西，在你不打开view-source时)。其实当时并没有开始去了解，最近开始希望学习下，做一些笔记。


angularjs简介
--------------

http://angularjs.org/

yeoman
-------

http://yeoman.io/

这次就从开始就使用这个工具，具体使用请看官网。我们使用angular的脚手架 https://github.com/yeoman/generator-angular, 这样生成目录和一些文件时可以方便写，同时也可以不同话时间纠结文件目录组织了。

我现在开始在一个已有json输出的项目上开始做web app. 开发的时候一边开个原有项目，一边使用grunt serve. 这样会在web app 数据请求的时候出现要去跨域请求，所以就需要grunt serve 能够将web app的请就转发到我们的后端server上。这时我们需要用到`grunt-connect-proxy`。

安装`grunt-connect-proxy`:

```sh
npm install grunt-connect-proxy
```
或者写道`package.json`中然后执行 `npm install`

配置: 参考 http://www.hierax.org/2014/01/grunt-proxy-setup-for-yeoman.html

然后我配置完的Guntfile.js  https://gist.github.com/liuzhe0223/9594439

做登录
--------

后台只有session认证，我们就从这里开始



未完。。
