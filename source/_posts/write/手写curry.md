---
title: 手写curry
date: 2022-10-31
categories: write
---

## 手写curry

```js
function curry(fn,args){
  //获取函数的参数长度
  let length = fn.length;
  args = args || [];
  return function(){
    //获取上层函数调用的参数数组
    let subArgs = [].slice.call(args);
    //将本层的参数添加和上层参数合并到一起
    for(let i = 0, len = arguments.length; i < len; i ++){
      subArgs.push(arguments[i]);
    }
    //判断如果参数的长度是否满足函数所有参数的长度
    if(subArgs.length >= length){
      return fn.apply(this, subArgs);
    } else {
      return curry.call(this, fn, subArgs);
    }
  }
}
```