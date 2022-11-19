---
title: Promise通过什么消灭了回调地狱？
date: 2022-11-19
categories: 
- 面试准备
- 异步
tag:
- 异步
- Promise
---

## Promise通过什么消灭了回调地狱？

1. 回调函数的延时绑定。
2. 返回值的穿透。
3. 错误冒泡。

如果我们在Promise中写延时器，延时一秒后再调用resolve，then也会等待一秒才执行。这就是回调函数的延时调用。

Promise返回的仍然是一个Promise，这就使得Promise穿透到外层，这就是返回值的穿透，这也使得Promise可以链式调用。

我们不用给每个Promise.then都加上错误的回调，我们只需要在链式调用的最后加上.catch就可以捕获到上层的错误，这就是Promise的错误冒泡。

