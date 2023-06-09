---
title: HTTP1.1和HTTP2的区别
date: 2022-11-24
categories:
- 面试准备
- 网络
tag:
- 网络
- HTTP
---

## HTTP1.1和HTTP2的区别

1. 二进制协议
在http1.1中，头部信息是文本，数据可以是文本也可以是二进制，而http2中都必须是二进制，这个概念称为"帧"，分为头部信息帧和数据帧，帧也是http2中多路复用的基础。

2. 多路复用
在http2中，同样复用了tcp连接，但是http2中可以同时接收和发送多个请求和响应，不用按照顺序一一发送。
对头阻塞：在http1.1中，报文是一发一收的，在http1.1中没有优先级，只有入队的先后顺序，先入队的先执行，如果对首的请求处理的时间过长，后面的请求需要等待对首的执行完毕才能继续执行，这就是对头阻塞。

3. 数据流
在http2中引入了数据流的概念，并不是按顺序发送的，所以每个数据流都有一个独一无二的编号，用来区分是哪个数据流。

4. 头部信息压缩
在http1.1每次都要附带上所有的信息发送请求，例如cookien，这就使得附带的信息可能有重复的，所以，在http2中会使用gzip或compress进行头部信息的压缩，而且服务器和客户端会共同维护一个头部信息表，所有的字段存入这个表中，生成一个索引值，当下次携带相同的信息的时候只携带索引值。

5. 服务器推送
在http2中，服务器在没有收到请求的情况下，主动向客户端推送资源，减少一些延迟时间，推送的资源只能是静态资源。websocket向服务端请求的及时数据是不推送的。