---
title: TCP流量控制和死锁
date: 2022-11-23
categories: 
- 面试准备
- 网络
tag:
- 网络
- TCP
- 死锁
---

## TCP流量控制和死锁

1. TCP中采用滑动窗口来进行流量控制。
2. 发送方和接收方都有一个缓存区。
3. 如果接收方读取的速度与数据到达的速度一样快，那么接收方就会向发送方发送一个正的窗口通知。
4. 如果接收方读取的速度慢于数据到达的速度，接收的数据会填满缓存区，这时接收方会发送一个零的窗口通知，将发送方的窗口设置成零，这是发送方的窗口将不能传输数据。
5. 发送方等待接收方发送一个正的窗口通知后再次传输数据。
6. 如果在接收方向发送方发送正的窗口通知的时候，通知丢失了。这是就会产生发送方等待接收方的正的窗口通知，而接收方等待发送方的数据，这就形成了死锁。
7. 在发送方接收到接收方发送的零窗口的通知的时候，就会启用持续计时器，如果计时器超时就会发送一个探测报文，只有1字节，去探测接收方的窗口是否是零。如果是零，那么就重新启用计时器，如果不是零就接触了死锁的局面。