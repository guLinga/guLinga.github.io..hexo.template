---
title: 手写new
date: 2022-09-29
categories: write
---

## 手写new

```js
function create(){
  let obj = {};
  let Con = [].shift.call(arguments);
  console.log([].shift);
  obj.__proto__ = Con.prototype;
  let result = Con.apply(obj,arguments);
  return result instanceof Object ? result : obj;
}
```