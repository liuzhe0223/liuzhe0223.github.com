---
layout: post
title: 理解unix进程python
tags: python
category: 学习
---



进程皆有标识
-------------

{% highlight python %}

os.getpid()

{% endhighlight %}

进程皆有父
------------

{% highlight python %}

os.getppid()

{% endhighlight %}

进程皆有文件描述符
-----------------

{% highlight python %}

file = open('test', 'r')
print file.fileno()

{% endhighlight %}

在unix系统中万物皆文件，在进程中打开一个文件（设备，管道，套接字等）会获得一个该进程中唯一的文件标识符，文件描述符不会在无关进程之间共享。在进程中打开的文件，其文件描述符会从3开始，进程会分配一个最小的未使用的非负整数。号码从3开始说明 0 1 2 已经被占用，其实每个unix进程都有三个打开的资源，分别是 标准输入 标准输出和标准错误


{% highlight python %}

sys.stdin.fileno()
=> 0
sys.stdout.fileno()
=> 1
sys.stderr.fileno()
=> 2
    
{% endhighlight %}

进程皆有资源限制
-----------------

{% highlight python %}

resource.getrlimit(resource.RLIMIT_NOFILE)
=> (1024, 4096)  #这是我机器的返回

{% endhighlight %}

返回为一个元组，第一个返回值为软限制，第二个返回值为硬限制。软限制其实不算限制，超过软限制程序会抛出异常，进程可以修改软限制，但硬限制只有在进程有足够的权限时才能修改。我们来试着修改一下限制,修改为的两个参数尽量不要超过原来硬限制的值（当前硬限制的值), 也就是说如果修改后已修改后的为准：

{% highlight python %}

resource.setrlimit(resource.RLIMIT_NOFILE,(4000,4096))
resource.getrlimit(resource.RLIMIT_NOFILE)
=> (4000,4096)
    
{% endhighlight %}

其它相关使用资源的查询与修改类似，比如现在进程所能衍生的进程的限制，参照 http://docs.python.org/2/library/resource.html


进程皆有环境
---------------

环境指的是环境变量，python读取环境变量：

{% highlight python %}

os.environ.get('PATH')
=>
'/home/vagrant/.rvm/gems/ruby-2.0.0-p247/bin:/home/vagrant/.rvm/gems/ruby-2.0.0-p247@global
/bin:/home/vagrant/.rvm/rubies/ruby-2.0.0-p247/bin:/home/vagrant/.rvm/bin:/usr/local/sbin:/
usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/opt/vagrant_ruby/bin'

{% endhighlight %}


进程皆有参数
-----------------

进程可以访问 sys.argv 的数组来获得命令行传进来的参数

```python
import sys
print(sys.argv[0])
print(sys.argv[1])
```

编写命令行程序的话可以用optparse 
http://docs.python.org/2/library/optparse.html


未完。。。
