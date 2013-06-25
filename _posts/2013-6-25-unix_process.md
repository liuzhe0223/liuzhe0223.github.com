---
layout: post
title: 理解unix进程
tags: unix pyhton
category: 学习
---


进程皆有标识
-------------
```
os.getpid()
```

进程皆有父
------------

```
os.getppid()
```

进程皆有文件描述符
------------------

```
file = open('test', 'r')
print file.fileno()
```

>在unix系统中万物皆文件，在进程中打开一个文件（设备，管道，套接字等）会获得一个该进程中唯一的文件标识符，文件描述符不会在无关进程之间共享。在进程中打开的文件，其文件描述符会从3开始，进程会分配一个最小的未使用的非负整数。号码从3开始说明 0 1 2 已经被占用，其实每个unix进程都有三个打开的资源，分别是 标准输入 标准输出和标准错误

```
sys.stdin.fileno()
=> 0
sys.stdout.fileno()
=> 1
sys.stderr.fileno()
=> 2
```

进程皆有资源限制
-----------------

```
resource.getrlimit(resource.RLIMIT_NOFILE)
=> (1024, 4096)  #这是我机器的返回
```

>返回为一个元组，第一个返回值为软限制，第二个返回值为硬限制。软限制其实不算限制，超过软限制程序会抛出异常，进程可以修改软限制，但硬限制只有在进程有足够的权限时才能修改。我们来试着修改一下限制,修改为的两个参数尽量不要超过原来硬限制的值（当前硬限制的值), 也就是说如果修改后已修改后的为准：

````
resource.setrlimit(resource.RLIMIT_NOFILE,(4000,4096))

resource.getrlimit(resource.RLIMIT_NOFILE)
=> (4000,4096)
``
