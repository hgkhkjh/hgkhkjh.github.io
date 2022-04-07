---
title: http知识点
date: 2022-3-26
tags:
---
## 1.get post区别

**从缓存的角度**，GET 请求会被浏览器主动缓存下来，留下历史记录，而 POST 默认不会。
**从编码的角度**，GET 只能进行 URL 编码，只能接收 ASCII 字符，而 POST 没有限制。
**从参数的角度**，GET 一般放在 URL 中，因此不安全，POST 放在请求体中，更适合传输敏感信息。
**从幂等性的角度**，GET是幂等的，而POST不是。(幂等表示执行相同的操作，结果也是相同的)
**从TCP的角度**，GET 请求会把请求报文一次性发出去，而 POST 会分为两个 TCP 数据包，首先发 header 部分，如果服务器响应 100(continue)， 然后发 body 部分。(火狐浏览器除外，它的 POST 请求只发一个 TCP 包)

> **什么是幂等**
> 幂等通俗的来讲就是指同一个请求执行多次和仅执行一次的效果完全相等。这里来扯出幂等主要是为了处理同一个请求重复发送的情况，假如在请求响应之前失去连接，如果这个请求时幂等的，那么就可以放心的重发一次请求。所以可以得出get请求时幂等的，可以重复发送请求，post请求时不幂等的，重复请求可能会发生无法预知的后果。
## 网络模型
网络是一个复杂的系统，不仅包括大量的应用程序、端系统、通信链路、分组交换机等，还有各种各样的协议组成，那么现在我们就来聊一下网络中的协议层次。
为了给网络协议的设计提供一个结构，网络设计者以分层(layer)的方式组织协议，每个协议属于层次模型之一。每一层都是向它的上一层提供服务(service)，即所谓的服务模型(service model)。每个分层中所有的协议称为 协议栈(protocol stack)。因特网的协议栈由五个部分组成：**物理层、链路层、网络层、运输层和应用层**。我们采用自上而下的方法研究其原理，也就是应用层 -> 物理层的方式。


**应用层**
**应用层是网络应用程序和网络协议存放的分层**，因特网的应用层包括许多协议，例如我们学 web 离不开的 HTTP，电子邮件传送协议 SMTP、端系统文件上传协议 FTP、还有为我们进行域名解析的 DNS 协议。应用层协议分布在多个端系统上，一个端系统应用程序与另外一个端系统应用程序交换信息分组，**我们把位于应用层的信息分组称为 报文**(message)。

**运输层**
**因特网的运输层在应用程序断点之间传送应用程序报文，在这一层主要有两种传输协议 TCP和 UDP**，利用这两者中的任何一个都能够传输报文，不过这两种协议有巨大的不同。
TCP 向它的应用程序提供了面向连接的服务，它能够控制并确认报文是否到达，并提供了拥塞机制来控制网络传输，因此当网络拥塞时，会抑制其传输速率。
UDP 协议向它的应用程序提供了无连接服务。它不具备可靠性的特征，没有流量控制，也没有拥塞控制。我们把运输层的分组称为 报文段(segment)

**网络层**
**因特网的网络层负责将称为 数据报(datagram) 的网络分层从一台主机移动到另一台主机**。网络层一个非常重要的协议是 IP 协议，所有具有网络层的因特网组件都必须运行 IP 协议，IP 协议是一种网际协议，除了 IP 协议外，网络层还包括一些其他网际协议和路由选择协议，一般把网络层就称为 IP 层，由此可知 IP 协议的重要性。

**链路层**
现在我们有应用程序通信的协议，有了给应用程序提供运输的协议，还有了用于约定发送位置的 IP 协议，那么如何才能真正的发送数据呢？ **为了将分组从一个节点（主机或路由器）运输到另一个节点，网络层必须依靠链路层提供服务。** 链路层的例子包括以太网、WiFi 和电缆接入的 DOCSIS 协议，因为数据从源目的地传送通常需要经过几条链路，一个数据包可能被沿途不同的链路层协议处理，我们把链路层的分组称为 帧(frame) 
**物理层**
**虽然链路层的作用是将帧从一个端系统运输到另一个端系统，而物理层的作用是将帧中的一个个 比特 从一个节点运输到另一个节点**，物理层的协议仍然使用链路层协议，这些协议与实际的物理传输介质有关，例如，以太网有很多物理层协议：关于双绞铜线、关于同轴电缆、关于光纤等等。

**转自https://juejin.cn/post/6844904045572800525**
## HTTP 和 HTTPS 的区别
HTTP 是一种 超文本传输协议(Hypertext Transfer Protocol)，HTTP 是一个在计算机世界里专门在两点之间传输文字、图片、音频、视频等超文本数据的约定和规范
![在这里插入图片描述](https://img-blog.csdnimg.cn/9289d895315e4e518c9fbace4f4177d6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiH5LqL6IOc5oSPc3k=,size_20,color_FFFFFF,t_70,g_se,x_16)
HTTP 主要内容分为三部分，超文本（Hypertext）、传输（Transfer）、协议（Protocol）。

> 超文本就是不单单只是本文，它还可以传输图片、音频、视频，甚至点击文字或图片能够进行超链接的跳转。

> 上面这些概念可以统称为数据，传输就是数据需要经过一系列的物理介质从一个端系统传送到另外一个端系统的过程。通常我们把传输数据包的一方称为请求方，把接到二进制数据包的一方称为应答方。

> 而协议指的就是是网络中(包括互联网)传递、管理信息的一些规范。如同人与人之间相互交流是需要遵循一定的规矩一样，计算机之间的相互通信需要共同遵守一定的规则，这些规则就称为协议，只不过是网络协议。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20ca507695d1455cb852a8592beeb98d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiH5LqL6IOc5oSPc3k=,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/fd7c74a0afe14e2daa41c0903795f9f1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiH5LqL6IOc5oSPc3k=,size_20,color_FFFFFF,t_70,g_se,x_16)
## HTTP 和 HTTPS 的主要区别
1.最简单的，HTTP 在地址栏上的协议是以 http:// 开头，而 HTTPS 在地址栏上的协议是以 https:// 开头
2.HTTP 是未经安全加密的协议，它的传输过程容易被攻击者监听、数据容易被窃取、发送方和接收方容易被伪造；而 HTTPS 是安全的协议，它通过 密钥交换算法 - 签名算法 - 对称加密算法 - 摘要算法 能够解决上面这些问题。
3.HTTP 的默认端口是 80，而 HTTPS 的默认端口是 443。
## 什么是无状态协议，HTTP 是无状态协议吗，怎么解决
无状态协议(Stateless Protocol) **就是指浏览器对于事务的处理没有记忆能**力。举个例子来说就是比如客户请求获得网页之后关闭浏览器，然后再次启动浏览器，登录该网站，但是服务器并不知道客户关闭了一次浏览器。
**HTTP 就是一种无状态的协议**，他对用户的操作没有记忆能力。可能大多数用户不相信，他可能觉得每次输入用户名和密码登陆一个网站后，下次登陆就不再重新输入用户名和密码了。这其实不是 HTTP 做的事情，起作用的是一个叫做 小甜饼(Cookie) 的机制。它能够让浏览器具有记忆能力

> 解决办法：
> 1.cookie
> 2.JWT 机制

JWT 的 Cookie 信息存储在客户端，而不是服务端内存中。也就是说，JWT 直接本地进行验证就可以，验证完毕后，这个 Token 就会在 Session 中随请求一起发送到服务器，通过这种方式，可以节省服务器资源，并且 token 可以进行多次验证。
JWT 支持跨域认证，Cookies 只能用在单个节点的域或者它的子域中有效。如果它们尝试通过第三个节点访问，就会被禁止。使用 JWT 可以解决这个问题，使用 JWT 能够通过多个节点进行用户认证，也就是我们常说的跨域认证。

## UDP 和 TCP 的区别
**UDP**
UDP 的全称是 User Datagram Protocol，用户数据报协议。它不需要所谓的握手操作，从而加快了通信速度，允许网络上的其他主机在接收方同意通信之前进行数据传输。


**UDP 的特点主要有**

UDP 能够支持容忍数据包丢失的带宽密集型应用程序
UDP 具有低延迟的特点
UDP 能够发送大量的数据包
UDP 能够允许 DNS 查找，DNS 是建立在 UDP 之上的应用层协议。

**TCP 是什么**
TCP 的全称是Transmission Control Protocol ，传输控制协议。它能够帮助你确定计算机连接到 Internet 以及它们之间的数据传输。通过三次握手来建立 TCP 连接，三次握手就是用来启动和确认 TCP 连接的过程。一旦连接建立后，就可以发送数据了，当数据传输完成后，会通过关闭虚拟电路来断开连接。
**TCP 的主要特点有**

TCP 能够确保连接的建立和数据包的发送
TCP 支持错误重传机制
TCP 支持拥塞控制，能够在网络拥堵的情况下延迟发送
TCP 能够提供错误校验和，甄别有害的数据包。

**TCP的拥塞控制**
主要针对网络中某一资源的需求超过了该资源所能提供的可用部分，网络性能就要变坏
**几种拥塞控制方法**

    慢开始、拥塞避免、快重传和快恢复。

## TCP 和 UDP 的不同
![在这里插入图片描述](https://img-blog.csdnimg.cn/27663104326b41d9af9532d9a87a137c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiH5LqL6IOc5oSPc3k=,size_20,color_FFFFFF,t_70,g_se,x_16)

## TCP 三次握手和四次挥手
**三次握手**
![在这里插入图片描述](https://img-blog.csdnimg.cn/9883a0cf13224dbb824f66ab328aa7ba.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiH5LqL6IOc5oSPc3k=,size_20,color_FFFFFF,t_70,g_se,x_16)

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/08afbb69a310490eab4cd469fb8ef2df.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5oiR6KaB5a2m5YmN56uv44CC,size_20,color_FFFFFF,t_70,g_se,x_16)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/c58c493fa4c74fa38d2ac62d00b2cd3f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5oiR6KaB5a2m5YmN56uv44CC,size_20,color_FFFFFF,t_70,g_se,x_16)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/788eeea0d89246549c213aff48e6daab.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5oiR6KaB5a2m5YmN56uv44CC,size_20,color_FFFFFF,t_70,g_se,x_16)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/9d35904b3db74beb91e92fda5db0e341.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5oiR6KaB5a2m5YmN56uv44CC,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/26c4584d5a894dd1a4cc76d987c8d976.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5oiR6KaB5a2m5YmN56uv44CC,size_20,color_FFFFFF,t_70,g_se,x_16)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/0bdd59d415284ac0af3031ca4ad8677b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5oiR6KaB5a2m5YmN56uv44CC,size_20,color_FFFFFF,t_70,g_se,x_16)
> 但是在三次握手的情况下服务端收不到最后的ACK包不会认为链接建立成功
> 其实三次握手的本质是解决 网络链接不可靠的问题

**四次挥手**
![在这里插入图片描述](https://img-blog.csdnimg.cn/03c5fddd9ae849288ba57a444d00d427.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5oiR6KaB5a2m5YmN56uv44CC,size_20,color_FFFFFF,t_70,g_se,x_16)

为什么 客服端要经过超市等待时间才能关闭链接这是为了保证服务端接收到ACK包
如果客服端立即关闭链接这时ACK 包发生了丢失，服务端会一直进入等待确认状态
如果有超时等待时间，当服务端没有收到ACK包时会重新发生FIN包，客服端会响应这个FIN包然后从新发送ACK包并刷新超时等待时间

摘抄：https://juejin.cn/post/6844904132067885064#heading-20
