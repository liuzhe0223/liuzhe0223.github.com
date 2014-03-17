---
layout: post
title: django更适用于json
tags: python, django
category: blog
---

django不合口
============

django 是我还没认识web是什么时开始接触的一个框架，但确是我接触的python框架中唯一一个至今也没用于一个完整项目的框架，从此大概看出对于我对django的感觉, 不合口，特别是使用过rails一次之后。

现在接近毕业，正在实习中，现在项目中使用的正是这个"姜哥", 不知觉的就会想到在rails中的一些小但很实用的东西在django中实现，并使它用的更方便一点。（以下若有错误希望高手指正)

开始添加简单代码更好的使用django
==================================

需求
------

现在需要实现的是使用django作为json输出单页应用的后端，仅这几天看文档没有发现django中有对此的比较方便的实现。扫了眼一个扩展，django-restful，对扫了眼，开始没大仔细看，现在看着还行。不过这次没有去用，看看使用我的原始方法写写我认为的rest api。

处理request的json数据
----------------------

这个json数据也就是当使用json上传时的`request.body`里的内容, 要达到的目的是要在view中能够方便的使用`request.data`这样的方式使用上传的数据。

开始的想法是同时处理json请求和普通的formdata请求,所以开始直接把两种数据都放到request.POST中,所以写了一个中间件他，添加`process_view` 方法：

```python
def process_view(self, request, view_func, view_args, view_kwargs):
    is_json = None
    if "CONTENT_TYPE" in request.META and \
            request.META['CONTENT_TYPE'] == 'application/json':
        is_json = True

    if is_json:
        request.POST = request.POST.copy()  #request.POST 为django中的有序字典实现，初始换中后request.POST只读
        for k, v in json.loads(request.body).iteritems():
            request.POST[k] = v

    return None
```

后来认为使用request.POST 在后来的处理put方法给的数据时这个名字应该不太合适, 而且html的标点提交没有put方法。最后就直接只去处理json数据:


```python
def process_view(self, request, view_func, view_args, view_kwargs):

    has_json_data = 'CONTENT_TYPE' in request.META and \
        request.META['CONTENT_TYPE'] == 'application/json'

    if has_json_data:
        try:
            data_dict = json.loads(request.body)
        except:
            return HttpResponse(
                json.dumps({
                    'code': 1,
                    'errors': {
                        'json': ['json format error']
                    }
                }),
                content_type="application/json"
            )
        request.data = data_dict

    return None
```

这样就只处理json数据并把数据存入`request.data`中


返回数据
---------

为了在view中少些写json.dumps, 就改为view function中只返回dict的数据在中间件中dump:

```python
def process_response(self, request, response):
    if isinstance(response, dict):
        return HttpResponse(json.dumps(response),
                            content_type="application/json")
    else:
        return response
```

路由
-------

路由基本是类似下边：
```
(r'^parents/(?P<parent_id>\d+)/chrilren/$', childView),               
(r'^chrildren/(?P<child_id>\d+)/$', childView),
```
view 使用的普通function view, 具体操作使用的是对request.method(get, post, put, delete) 和一些参数的一块判断为何种操作进而进行操作。
