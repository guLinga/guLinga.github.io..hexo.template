---
title: async、await的运行机制
date: 2022-11-19
categories: 
- 面试准备
- 异步
tag:
- 异步
- async
- await
---

## async、await的运行机制

### async
async是通过异步执行并隐式返回Promise的函数。
```js
async function fn(){
  return 1
}
console.log(fn());//Promise {<fulfilled>: 1}
```

### await
```js
async function test() {
  console.log(2)
  let x = await 4
  console.log(x)
  console.log(5)
}
console.log(1)
test()
console.log(3)
```
上面的代码输出1 2 3 4 5。

我们先来看一下上面的代码是怎么执行的？
js先执行同步任务，先输出1，然后执行test函数，输出2。
这里就要注意了`let x = await 4`，js引擎将`await 4`转换成了一个Promise。
```js
let promise = new Promise((resolve, reject) => {
  resolve(4)
})
```
将其压入微任务队列，然后继续执行同步任务输出3，
同步任务执行完之后，执行微任务队列。
输出4，最后输出5。

### 测试题目
```js
async function foo() {
  console.log('3')
}

async function fn() {
  console.log('6')
  return 8
}

async function bar() {
  console.log('2')
  await foo()
  let result = await fn()
  console.log(result);
  console.log('9')
}

console.log('1')

setTimeout(function () {
  console.log('10')
}, 0)

bar()

new Promise(function (resolve) {
  console.log('4')
  resolve()
}).then(function () {
  console.log('7')
})

console.log('5')
```
输出1 2 3 4 5 6 7 8 9 10