---
title: TCP的重传机制
date: 2022-11-23
categories: 
- 面试准备
- 网络
tag:
- 网络
- TCP
---

## TCP的重传机制

TCP的下层网络可能会出现，重复，丢失，失序等情况，TCP会重传它自己认为丢失的包，TCP的重传是根据两方面来进行的，一个是时间，一个是确认信息。

TCP在发送一个数据之后就会开启一个定时器，如果在这个时间内，没有收到ACK的确认报文，则重传数据，如果多次重传都没有成功则放弃重传，并返回一个复位信号。