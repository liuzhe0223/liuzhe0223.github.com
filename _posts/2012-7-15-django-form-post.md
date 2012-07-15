---
layout: post
title: django1.4 form post提交时 CSRF verification failed. Request aborted.
tags: django, python
---

在对应表单模板 \<form action="#" method='post'\> 后加上 \{% csrf_token %\} 标签并用在view中加入 RequestContext 解析模板
