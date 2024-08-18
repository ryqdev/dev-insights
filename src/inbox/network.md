# Network

**A B C类网络地址范围**

[https://www.sohu.com/a/370994047\_825275](https://www.sohu.com/a/370994047\_825275)

socket连接以后 数据存储位置 进程如何获得这些数据？

[https://cloud.tencent.com/developer/article/1666211](https://cloud.tencent.com/developer/article/1666211)

#### 基础知识

## Cookies and Sessions

Cookies is to make server identify who you are, and sessions is for security.

### Cookies

* Stored in browser (in client).
* Not secure, since data is stored in a text file.

### Sessions

* Session stores the variables and their values (e.g. username and password) in the server.
* Secure. Since cookies only store session id.

[https://www.zhihu.com/question/21546408](https://www.zhihu.com/question/21546408)

#### web

web页面请求过程是怎样的

[https://www.cnblogs.com/51benpao/p/12984951.html](https://www.cnblogs.com/51benpao/p/12984951.html)

两台计算机之间是如何通信

[https://cloud.tencent.com/developer/article/1793766](https://cloud.tencent.com/developer/article/1793766)

**ISO 七层 以及各自的应用**

* 物理层：

集线器（hub），用网线把计算机连接在一起

用MAC地址用来标识不同的计算机，但是这时候的hub不是很智能，数据包是广播给所有的计算机，每个计算机判断这个数据包是不是给自己的，如果不是就丢弃

* 数据链路层：

为了解决上面提到的问题，交换机（switch）出现了。交换机里面会存储一张 MAC 地址表：

| MAC 地址            | 端口 |
| ----------------- | -- |
| bb-bb-bb-bb-bb-bb | 1  |
| cc-cc-cc-cc-cc-cc | 3  |
| aa-aa-aa-aa-aa-aa | 4  |

通过维护这样的一张表格，就可以把数据包定向传输给目标机器。

但是，随着网络的发展，MAC地址表会变得巨大，交换机的端口数量有限，解决办法是用网线将交换机连接在一起。

* 网络层：

上述交换机的方案在互联网时代是不适用的，原因是从交换机出去的寻找其他交换机的那根网线，还不知道会后面的路径是怎么样的。

这时候出现了路由器，帮助交换机做一次转发，目的是快速找到对应的交换机。

但是与此同时，随着计算机数量的暴增，如何维护MAC地址表成了一个问题，因为之前是通过MAC地址来定位计算机，交换机维护MAC地址表格，为了方便路由器转发，交换机连接的机器的MAC地址最好是有一些统一的特征（比如有一样的前缀）。但是MAC地址本来就是和硬件绑定的并且随机的，这时候就出现IP地址这个工具，它是软件层面的地址，可以通过IP地址来构建网络拓扑结构，方便路由器的转发。

输入域名，如何得到ip地址

[https://blog.51cto.com/leonsh/3860919](https://blog.51cto.com/leonsh/3860919)

* 传输层：

TCP和UDP

* 会话层
* 表示层
* 应用层

Compare with sliding window algorithm

#### 传输层

TCP and UDP

**TCP**

[https://zhuanlan.zhihu.com/p/108822858](https://zhuanlan.zhihu.com/p/108822858)

TCP 协议的特点：

TCP 三次握手 和 四次挥手(讲细节， TCP三次握手目的，过程及状态转换，三次握手过程中传输的都是什么信息？)

讲一下 TCP 的延迟主要是哪些：

* 拥塞避免的慢开始[算法](https://www.nowcoder.com/jump/super-jump/word?word=%E7%AE%97%E6%B3%95)。
* 多次握手挥手。

time-wait和close-wait分别指什么

[https://www.jianshu.com/p/f6a563f3526c](https://www.jianshu.com/p/f6a563f3526c)

2MSL 的目的

TCP SYN 攻击

TCP的back log

[https://tonytancoder.github.io/2019/11/27/TCP%E4%B8%ADbacklog%E5%8F%82%E6%95%B0%E7%9A%84%E4%BD%9C%E7%94%A8/#:\~:text=backlog%E9%80%9A%E5%B8%B8%E8%A2%AB%E6%8F%8F%E8%BF%B0%E4%B8%BA,%E4%B8%8A%E9%9D%A2%E7%9A%84TCP%E7%8A%B6%E6%80%81%E5%9B%BE%EF%BC%89%E3%80%82](https://tonytancoder.github.io/2019/11/27/TCP%E4%B8%ADbacklog%E5%8F%82%E6%95%B0%E7%9A%84%E4%BD%9C%E7%94%A8/)

[https://juejin.cn/post/6844904071367753736](https://juejin.cn/post/6844904071367753736)

TCP 拥塞控制和流量控制（流量控制和拥塞控制要区别开来）：[https://zhuanlan.zhihu.com/p/37379780](https://zhuanlan.zhihu.com/p/37379780)

服务器最多可以建立几个TCP连接

[https://zhuanlan.zhihu.com/p/290651392#:\~:text=%E5%81%87%E8%AE%BE%E4%BD%A0%E5%8F%AA%E4%BF%9D%E6%8C%81%E8%BF%9E%E6%8E%A5,%E6%95%B0%E9%87%8F%E6%98%AF100%E4%B8%87%E5%B7%A6%E5%8F%B3%E3%80%82](https://zhuanlan.zhihu.com/p/290651392)

如何实现可靠的UDP连接

[https://zhuanlan.zhihu.com/p/129218784](https://zhuanlan.zhihu.com/p/129218784)

#### 会话层

cookie和session的区别

session如何识别是来自哪个客户端的请求

[https://www.51cto.com/article/679219.html](https://www.51cto.com/article/679219.html)

#### 表示层

#### 应用层

[https://juejin.cn/post/7108012337264607262](https://juejin.cn/post/7108012337264607262)

http 和 https 的区别

每次请求，https 的公私钥固定不变吗？(第一次会协商和加密，之后同一个浏览器的话会用 session id 做 session key 的提取，可以看做固定不变)

http 状态码介绍一下

301和302 的区别

https 在传输消息前多出了几次 RTT(2-7次)

http 报文介绍

[https://www.cnblogs.com/huansky/p/14007810.html](https://www.cnblogs.com/huansky/p/14007810.html)

分别介绍 http 1.1 2.0 3.0

[https://juejin.cn/post/7128770691247128589](https://juejin.cn/post/7128770691247128589)

[https://zhuanlan.zhihu.com/p/266578819](https://zhuanlan.zhihu.com/p/266578819)

http 2.0:

\1. HTTP2使用的是二进制传送。 \2. HTTP2支持多路复用。 \3. HTTP2头部压缩。 \4. HTTP2支持服务器推送。 \5. HTTP2支持设置请求的优先级。 \6. 还答到了这些区别是为了优化HTTP1.x哪些痛点。队头阻塞、TCP连接之间竞争带宽等等。

讲一下缓存(应用层，http相关的)

[https://cloud.tencent.com/developer/news/588770#:\~:text=%E4%BB%80%E4%B9%88%E6%98%AFHTTP%20%E7%BC%93%E5%AD%98,%E5%8E%BB%E6%BA%90%E6%9C%8D%E5%8A%A1%E5%99%A8%E9%87%8D%E6%96%B0%E4%B8%8B%E8%BD%BD%E3%80%82](https://cloud.tencent.com/developer/news/588770)

#### socket编程

介绍 socket

生成 tcp 和 udp socket有什么区别(在创建声明和 api 使用上均有区别)

socket 如何标识一个协议(sock\_type)

#### 要搞成非阻塞式socket怎么做？

#### 五种IO模型

[https://cloud.tencent.com/developer/article/1071195](https://cloud.tencent.com/developer/article/1071195)

UNIX网络编程 里面有介绍

## 面试中的计网知识

**三次握手过程**

[https://www.youtube.com/watch?v=TEh6t8meORo](https://www.youtube.com/watch?v=TEh6t8meORo)

**三次握手，为什么是3次，而不是2次或4次？（其实是在问原理，但是怎么和第一个问题相区别）**

[https://blog.csdn.net/lengxiao1993/article/details/82771768](https://blog.csdn.net/lengxiao1993/article/details/82771768) 里面的图不错

因为三次握手后，C和S至少可以确认之前的通信情况，但无法确认之后的情况。 所以如果四次还是五次或是更多次都是徒劳的。

**什么是TCP， TCP是如何确保传输安全的？**

Transmission Control Protocol 传输控制协议，是一种**面向连接(**连接导向)的、可靠的、 基于IP的传输层协议

保证安全可以参考：[https://www.cnblogs.com/sunlong88/p/12845035.html](https://www.cnblogs.com/sunlong88/p/12845035.html)

**TCP和UDP的区别**

最主要的区别在于：TCP 是面向连接的，UDP 是面向无连接的（可以理解成TCP是打电话，UDP是发邮件）

[https://blog.csdn.net/zhang6223284/article/details/81414149](https://blog.csdn.net/zhang6223284/article/details/81414149)

**输入一个URL，发生的过程**

[https://blog.csdn.net/wlk2064819994/article/details/79756669](https://blog.csdn.net/wlk2064819994/article/details/79756669)

[https://www.youtube.com/watch?v=104KeTzLcak](https://www.youtube.com/watch?v=104KeTzLcak)

**DNS 域名系统**

[https://blog.csdn.net/codejas/article/details/80086068](https://blog.csdn.net/codejas/article/details/80086068)

![Screen Shot 2021-02-26 at 21.33.21](https://raw.githubusercontent.com/Yukun4119/BlogImg/main/img/Screen%20Shot%202021-02-26%20at%2021.33.21.png)

**HTTP状态码**

[https://www.runoob.com/http/http-status-codes.html](https://www.runoob.com/http/http-status-codes.html)

**Http的301和302的区别**

总的来说，301是永久重定向，302是临时重定向，但是需要从以下几个方面展示：

* 从定义上来看，301是被请求的资源已永久移动到新位置， 而302是请求的资源现在临时从不同的 URI 响应请求。两者都是一个POST请求经过 301/302 后会被浏览器转为GET请求
* 从缓存方面，301有缓存，而302是没有缓存
* 在搜索引擎方面，301的旧地址不可访问，重定向到新的地址。但是302的旧地址仍然可以访问。
* 从安全性角度，301更加安全，302更容易被网址劫持。

**HTTP和HTTPS的区别，为什么HTTPS比较安全**

* HTTP（HyperText Transfer Protocol）：中文叫超文本传输协议， 直接通过明文在浏览器和服务器之间传递信息。
* HTTPS（Hypertext Transfer Protocol Secure）：中文叫超文本传输安全协议，是一种透过计算机网络进行安全通信的传输协议。采用`对称加密`和`非对称加密`结合的方式来保护浏览器和服务端之间的通信安全。
* HTTPS其实是有两部分组成：HTTP + SSL/ TLS，也就是在HTTP上又加了一层处理加密信息的模块
* HTTPS 经由 HTTP 进行通信，但利用 SSL/TLS 来加密数据包。HTTPS 开发的主要目的，是提供对网站服务器的身份认证，保护交换数据的隐私与完整性。

**HTTPS的SSL（TLS）协议**

[https://www.jianshu.com/p/a7292b4db7bd](https://www.jianshu.com/p/a7292b4db7bd)

**HTTPS加密（握手）过程**

[https://www.jianshu.com/p/eo30a8c4fa329](https://www.jianshu.com/p/eo30a8c4fa329)

**抓包的原理**

[https://blog.csdn.net/l61052319940708/article/details/80624900](https://blog.csdn.net/l61052319940708/article/details/80624900)

**断点续传怎么做的**

[https://blog.csdn.net/zhangliangzi/article/details/51348755](https://blog.csdn.net/zhangliangzi/article/details/51348755)

**什么是子网掩码、它的作用是什么**

[https://blog.csdn.net/mao\_hui\_fei/article/details/83005940](https://blog.csdn.net/mao\_hui\_fei/article/details/83005940)

**post和get的异同**

[https://www.cnblogs.com/logsharing/p/8448446.html](https://www.cnblogs.com/logsharing/p/8448446.html)

**流量控制和拥塞控制**

[https://blog.csdn.net/qq\_35396127/article/details/80019516](https://blog.csdn.net/qq\_35396127/article/details/80019516)

### Reference

[计算机网络——TCP协议中的三次握手四次挥手以及11种状态转换](https://blog.csdn.net/a987073381/article/details/52206215)

## HTTPS Explained

HTTPS = HTTP + SSL/TLS

### HTTPS

![https](https://raw.githubusercontent.com/Yukun4119/BlogImg/main/img/https.png)

(image source:[https://www.youtube.com/watch?v=T4Df5\_cojAs](https://www.youtube.com/watch?v=T4Df5\_cojAs) )

The process of **HTTPS**:

1. **Client** send package to **Server** (to request resources)
2. **Sever** show certificate to **Client** (containing **public key** and **private key**), and **Cilent** get the public key.
3. **Client** generates a new secret key encrypted with public key, and sends it to **Server**
4. **Server** decrypt this new secret key with private key(to get this new secret key， **asymmetric-key encryption**)
5. Since both **Client** and **Server** have the new secret key, they can encrypt and decrypt the message. （**symmetric-key encryption**）

### SSL vs TLS

**TLS**(Transport Layer Security) is the successor of **SSL**(Secure Sockets Layer)

Now TLS is used in most browsers.

### Reference

[How does HTTPS work? What's a CA? What's a self-signed Certificate?](https://www.youtube.com/watch?v=T4Df5\_cojAs)

[HTTPS加密过程详解](https://segmentfault.com/a/1190000019976390)

[通俗理解SSL/TLS协议区别与原理](https://xiaoyue26.github.io/2018/09/26/2018-09/%E9%80%9A%E4%BF%97%E7%90%86%E8%A7%A3SSL-TLS%E5%8D%8F%E8%AE%AE%E5%8C%BA%E5%88%AB%E4%B8%8E%E5%8E%9F%E7%90%86/)

[Transport Layer Security](https://en.wikipedia.org/wiki/Transport\_Layer\_Security)
