---
title: 手写instanceof
date: 2022-09-29
categories: write
---

[手写instanceof](#1)

<p id=1></p>

## 手写instanceof

```js
function _instanceof(A, B) {
  let temp = B.prototype;
  let __proto__ = A.__proto__;
  while (__proto__) {
    if (__proto__ === temp) return true;
    __proto__ = __proto__.__proto__;
  }
  return false;
}
```