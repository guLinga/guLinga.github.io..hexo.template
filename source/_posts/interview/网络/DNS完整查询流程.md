---
title: DNS完整查询流程
date: 2022-11-23
categories: 
- 面试准备
- 网络
tag:
- 网络
- DNS
---

## DNS完整查询流程

1. 浏览器查询自己的缓存，如果有对应的ip地址则直接返回，没有则向本地DNS服务器请求。
2. 本地DNS服务器查询自己的缓存，如果有对应的ip地址则直接返回，没有则向继续下一步。
3. 本地DNS服务器向根域名服务器发送请求，根域名服务器返回对应的顶级域名服务器地址。
4. 本地DNS服务器向定义域名服务器发送请求，顶级域名服务器返回对应的权威域名服务器地址。
5. 本地DNS服务器向权威域名服务器发送请求，权威域名服务器返回对应的结果。
6. 本地DNS服务器缓存该结果。
7. 本地DNS将结果返回给浏览器。

其中DNS服务器在向各个服务器发送请求的时候，各个服务器都会先查询缓存。