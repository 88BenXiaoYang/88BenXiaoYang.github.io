---
title: 图解HTTP
date: 2019-01-25 16:45:15
tags: 网络协议
categories: 网络协议
---
笔记

### 第一章

* 协议

```
协议是指规则的约定。
协议，是相互之间通信前所定下的规则。
```

像通过发送请求获取服务器资源的Web浏览器等，都可称为客户端。Web是建立在HTTP协议上通信的。（HTTP是Web文档传输的协议）

* WWW

```
WWW这一提议是致力于全世界的研究者们进行知识共享。
WWW基本理念：
借助多文档之间相互关联形成的超文本（HyperText），
连成可相互参阅的WWW（world wide web，万维网）。

现阶段构建WWW的3项技术：
HTML、HTTP（文档传递协议）、URL
```

* TCP/IP

```
TCP/IP是互联网相关的各类协议族的总称。
TCP/IP协议族里重要的一点就是分层。
分为4层：
应用层（FTP/DNS/HTTP）、
传输层（TCP/UDP）、
网络层、
数据链路层（硬件上的范畴均在链路层的作用范围之内）
```

通常使用的网络（包括互联网）是在TCP/IP协议族的基础上运作的。利用TCP/IP协议族进行网络通信时，当客户端的请求传输到服务器的应用层，才能算真正接收到由客户端发送过来的HTTP请求。（HTTP属于TCP/IP内部的一个子集）

##### 与HTTP关系密切的协议：IP、TCP、DNS

```
- 负责传输的IP协议
- 确保可靠性的TCP协议
- 负责域名解析的DNS服务（DNS提供域名到IP地址之间的解析服务）
```

* IP和IP地址的区别

```
- IP其实是一种协议的名称，IP协议的作用是把各种数据包传送给对方。
要保证确实传送到对方那里，则需要满足各类条件，其中两个重要的条件
是IP地址和MAC地址（media access control address）。

- IP地址指明了节点被分配到的地址，MAC地址是指网卡所属的固定地址，
IP地址可以和MAC地址进行配对，IP地址可变换，但MAC地址基本上不会更改。
```

IP间的通信依赖MAC地址。ARP（address resolution protocol）是一种用以解析地址的协议，根据通信方的IP地址就可以反查出对应的MAC地址。

* TCP

```
TCP位于传输层，提供可靠的字节流服务。
为了准确无误地将数据送达目标处，
TCP协议采用了三次握手（three-way handshaking）策略。
握手过程中使用了TCP的标志（flag），SYN（synchronize）和ACK（acknowledgement）。
```

* DNS

```
DNS协议提供通过域名查找IP地址，或逆向从IP地址反查域名的服务。
```

URI用字符串标识某一互联网资源，而URL表示资源的地点（互联网上所处的位置）。可见URL是URI的子集。表示指定的URI，要使用涵盖全部必要信息的绝对URI、绝对URL以及相对URL。相对URL，是指从浏览器中基本URI处指定的URL，形如`/image/logo.gif`。

计算机既可以被赋予IP地址，也可以被赋予主机名和域名（如：www.baidu.com）。

RFC（Request for Comments，征求修正意见书），是一些用来制定HTTP协议技术标准的文档。

### 第二章
### 第三章
### 第四章
### 第五章
### 第六章
### 第七章
### 第八章
### 第九章
### 第十章
### 第十一章