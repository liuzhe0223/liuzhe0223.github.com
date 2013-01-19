---
layout: post
title: python_socket_server
tags: python
category: 学习

---

做java课程设计, 做的IM, 现在回来看一下python的网络编程相关的部分先写了个基本的socket通信

server部分的代码如下:

{% highlight python %}
	import socket
	import threading

	class NetBase:
		def __init__(self):
			self.client_thread_lists = []
			self.sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
			self.sock.bind(('',8081))
			self.sock.listen(1)
			print "server start"


		def get_message(self):
			while(True):
				clientsock, clientaddr = self.sock.accept()
				print "got a client"
				thr = got_client(clientsock)
				self.client_thread_lists.append(thr);
				thr.start()
				print "start a thread"


    class got_client(threading.Thread):
    
    	def __init__(self, clientsocket):
    		threading.Thread.__init__(self)
    		self.clientsock = clientsocket
    
    	def run(self):
			print self.clientsock.getpeername()
    		while(True):
    			print self.clientsock.recv(1024)
    
    
    
    if __name__ == "__main__":
    	server = server.NetBase()
    	server.get_message()
{% endhighlight %}

需要导入网络模块socket, 为了实现多客户端的链接使用threading来实现多线程,python socket 需要指定两个参数,及地址族和套接字类型, 参数中的socket.AF_INET就是指定地址族,socket.AF_INET指的是使用ipv4协议, 后面的参数socket.SOCK_STREAM指定套接字类型为 面向链接的可靠字节流. server端绑定本地地址8081端口(self.sock.bind('',8081),并开始监听, get_message方法中 self.sock.accept() 当有客户端请求连接时得到一个server与client通信的socket, 然后把这个socket扔给自己定义的处理一客户端通信的socket的线程, 这个线程会接收来自客户端发送的消息,并将它打印出来.

client 端代码:

{% highlight python %}

    import socket
    
    class NetBase:
    	
    	def __init__(self):
    		self.sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    		self.sock.connect(("127.0.0.1", 8081))
    		print "got the server"
    	
    	def send_message(self):
    		while(True):
    			data =raw_input("send the server a message,\n")
    			print data
    			self.sock.sendall(data)
    
    
    if __name__ == "__main__":
    	client = client.NetBase()
    	client.send_message()
{% endhighlight %}

python 还内建提供了socketserver模块,包扩很多可以可以简化socket服务器的类
