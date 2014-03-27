---
layout: post
title: docker app 打包笔记
tags: python, docker
category: blog
---

docker介绍 
============

https://www.docker.io/

我的需求
==========

此次希望将要发布的app和执行环境进行打包, app使用python djnago开发。

这次我打算打包的步骤为：


*  自己制作docker的base image
*  配置app执行环境
*  将制作好的容器导出成文件，用于其他机器的导入


开始执行
=========

制作base image
----------------

首先我们先提取一个最小的rootfs, 我现在使用的系统是ubuntu12.04, 我想制作一个12.04的也就是`precise`的rootfs:

```bash
sudo debootstrap precise ubuntu

#precise 为 /usr/share/debootstrap/scripts 下存在的debootstrap script
#ubuntu 就是当前目录下的一个目录
```

制作过程比较长，制作完成后我们来把它导入到docker中成为一个image:

```bash
tar -C ubuntu -c . | sudo docker import - ubuntu
```

完成后我们就制作好了一个docker的base image, 我们可以同做下面这个命令来查看:

```bash
sudo docker images
```


配置app环境
------------

首先我要把app代码拷进容器中，这个是可以通过dockerfile 中的ADD来做的，不过我还没有仔细看dockerfile，就先手动通过qiniu上传在下载到运行中的容器中的.

首先我们要用docker使用我们前面制作好的image跑起一个shell

```bash
sudo docker run -i -t ubuntu bash
=>
root@210ae349a593:/# 
```

然后我们就可以在这个启动中的bash中来执行我们的常用操作了,我把上传的代码wget下来到 `/docker_app`, 对我把它放在了启动的容器的根目录下的`docker_app`,现在我要安装我app的依赖。

我先手动wget下来python的setuptools和pip，手动安装好.因为依赖中有MySQL-python,如果要用pip来安装的话要编译，编译的话需要安装mysql的一些lib,会使image变大，所以直接用apt-get安装了二进制的，并在requirments.txt中删掉了MySQL-python. 因为依赖中还有一个Pillow需要编译，所以忍痛装上的gcc和python-dev.

然后在docker_app目录下

```
pip install -r requirments.txt -i http://pypi.douban.com/simple
```

修改app配置文件中的数据库的地址，我们要查看我们本机docker0虚拟网卡的ip, 我机器的显示是:

```bash
ifconfig

=>

docker0   Link encap:Ethernet  HWaddr 76:71:61:ca:84:1b  
          inet addr:172.17.42.1  Bcast:0.0.0.0  Mask:255.255.0.0
          inet6 addr: fe80::c10:64ff:fe74:c0c6/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:617463 errors:0 dropped:0 overruns:0 frame:0
          TX packets:678907 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:56062519 (56.0 MB)  TX bytes:267105247 (267.1 MB)
```
所以我就把数据库ip改成 172.17.42.1, ok，环境配置好的，我们保持这个shell session，然后打开另一个shell session执行以下命令查看现在运行的这个容器的id:

```bash
sudo docker ps

=>

CONTAINER ID        IMAGE             .... 
210ae349a593        ubuntu:latest     ....
```

然后我们将这个容器commit 成 docker_app:

```bash
sudo docker commit 210ae349a593 docker_app
```

我们再次执行 `sudo docker images`, 就会发现我们生成的 docker_app 的image

下面我们 exit 刚才我们配置环境的那个 shell, 来测试一下我们配置的环境的image.

```bash
sudo docker run -i -t -d -p 8000:8000 docker_app python /docker_app/manage.py runserver 0.0.0.0:8000
```
这样我们就把本地8000端口和任务容器的8000端口做了映射，接下来我们就可以在本地浏览器上访问我们 `http://127.0.0.1:8000` 来访问我们的应用了, 我们还可以查看容器的输出日志, `容器id` 为我们`sudo docker ps` 出来的 `CONTAINER ID`:

```bash
sudo docker logs -f <容器id>

=>

Validating models...

0 errors found
March 27, 2014 - 14:14:10
Django version 1.6.1, using settings 'app.settings'
Starting development server at http://0.0.0.0:8000/
Quit the server with CONTROL-C.

```


导出我们制作好的image
-----------------------

docker 本身是有push 来上传的 index.docker.io 的，使用是用pull来拉取，但是由于在天朝，网络不给力。所以就导出成文件吧，docker官方也有给出自建image server的方案，貌似项目还不太成熟，以后再研究.

现在我们 docker ps 出刚才运行的那个容器的 id, 将它导出

```bash
sudo docker export <容器id>  >  docker_app.tar
```

完成后我们刚才制作的容器就导出成了 docker_app.tar 文件, 以后在其他机器部署的时候执行导入

```bash
cat docker_app.tar | sudo docker import - docker_app
```
管道后面的 docker_app 为导入后image命名，自己指定
