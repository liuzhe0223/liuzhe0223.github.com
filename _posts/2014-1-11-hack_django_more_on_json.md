---
layout: post
title: 使django更适用于json
tags: python, django
category: blog
---

django不合口
============

django 是我还没认识web是什么时开始接触的一个框架，但确是我接触的python框架中唯一一个至今也没用于一个完整项目的框架，从此大概看出对于我对django的感觉, 不合口，特别是使用过rails一次之后。

现在接近毕业，正在实习中，现在项目中使用的正是这个"姜哥", 不知觉的就会想到在rails中的一些小但很实用的东西在django中实现，并使它用的更方便一点。（以下若有错误希望高手指正)

开始添加简单代码更好的使用django
==================================

现在需要实现的是使用django作为json输出单页应用的后端，仅这几天看文档没有发现django中有对此的比较方便的实现。扫了眼一个扩展，django-restful，对扫了眼，在功能上可能实现的不错但怎么看文档都不合眼，所以没有深入去看。现在开始试着添加少部分的代码就能使得django对json输入输出更友好些。其实这些尝试前段时间就开始啦，尽量使用middleware来完成一些需要的特性，这样可以不用在view里多些一些代码啦，开了个库https://github.com/liuzhe0223/addons-to-django,开始慢慢完善吧。

未完。。
