---
title: react hooks性能优化
date: 2022-11-21
categories: 
- 面试准备
- React
tag: 
- React
- 性能优化
---

## react hooks性能优化

### memo
使用memo包裹组件，使组件的props进行浅比较。可以避免父组件重新渲染，而子组件中不需要重新渲染的问题。
```js
import { memo, useState } from 'react';
function App() {
  const [val,setVal] = useState(true);
  const onClick = () => {
    setVal(!val);
    console.log('点击了按钮');
  }
  return (
    <div>
      <Child />
      <MemoChilds />
      <button onClick={onClick}>刷新App</button>
    </div>
  )
}

export default App;

function Child() {
  console.log('child 渲染了');
  return (
    <div>
      child
    </div>
  )
}

function MemoChild(){
  console.log('memochild 渲染了');
  return (
    <div>
      memochild
    </div>
  )
}

const MemoChilds = memo(MemoChild)
```

### useMemo
使用useMemo包裹状态值的计算，避免重复的计算。
它接收两个参数，第一项是回调函数，useMemo正是返回的该回调函数的返回值，第二项是一个数组，数组里面是依赖项，当依赖项改变的时候，useMemo中的回调函数才会重新计算值。
```js
import { useMemo, useState } from 'react';
function App() {
  const [list] = useState([1,2,3]);
  const [val,setVal] = useState(true);
  //普通计算list的和
  console.log('普通计算list的和');
  const sum = list.reduce((prev,curr)=>prev+curr);
  //缓存计算list的和
  const memoSum = useMemo(()=>{
    console.log('缓存计算list的和');
    return list.reduce((prev,curr)=>prev+curr);
  },[list])

  const onClick = () => {
    setVal(!val);
  }
  return (
    <div>
      <div>sun: {sum}</div>
      <div>memoSun: {memoSum}</div>
      <button onClick={onClick}>重新渲染</button>
    </div>
  )
}

export default App;
```

### useCallback
我们刚说了使用memo来包裹组件，使组件进行浅比较。但是如果我们给组件传递的是一个函数，当函数触发state值更新后，函数就会重新改变，所以子组件接收的函数会改变，这就使得子组件重复渲染。我们可以给函数加上useCallback，让函数只更新一次。所以，新手一般都会给每个函数都加上useCallback，这是没必要的。如果每个函数都加上useCallback，那么每个函数都要进行依赖项的判断，造成了额外的性能消耗。
使用useCallback有以下两种情况
1. 组件内部定义的函数，需要作为其他hooks的依赖项。
2. 组件内部定义的函数，需要传递给子组件，并且子组件被memo包裹。

组件内部定义的函数，需要作为其他hooks的依赖项。
```js
import React, { useEffect, useState } from 'react';

function App() {
  const [count, setCount] = useState(1);
  const add = () => {
    setCount((count) => count + 1);
  };
  useEffect(() => {
    add();
  }, [add]);
  return <div className="App">count: {count}</div>;
}

export default App;
```

组件内部定义的函数，需要传递给子组件，并且子组件被memo包裹。

```js
import { memo, useState } from 'react';
function App() {
  const [val,setVal] = useState(true);
  const onClick = () => {
    setVal(!val);
    console.log('点击了按钮');
  }
  return (
    <div>
      <MemoChilds onClick={onClick} />
      <button onClick={onClick}>刷新App</button>
    </div>
  )
}
export default App;

function MemoChild({onClick}){
  console.log('memochild 渲染了');
  return (
    <div>
      memochild
    </div>
  )
}

const MemoChilds = memo(MemoChild)
```

```js
import { memo, useCallback, useState } from 'react';
function App() {
  const [val,setVal] = useState(true);
  const onClick = useCallback(() => {
    setVal(!val);
    console.log('点击了按钮');
  },[])
  return (
    <div>
      <MemoChilds onClick={onClick} />
      <button onClick={onClick}>刷新App</button>
    </div>
  )
}
export default App;

function MemoChild({onClick}){
  console.log('memochild 渲染了');
  return (
    <div>
      memochild
    </div>
  )
}

const MemoChilds = memo(MemoChild)
```

### 使用memo，props进行浅比较的时候是怎么比较的呢？
1. 如果都是基本类型，直接比较结果，结果正确则返回true。
2. 比较是否都是对象，如果有一个不是则返回false。
3. 比较两个对象的属性的数量是否一样，不一样则返回false。
4. 比较两者的属性是否相同，属性值是否相同。
```js
function shallowEqual (objA: mixed, objB: mixed): boolean {
  // 下面的 is 相当于 === 的功能，只是对 + 0 和 - 0，以及 NaN 和 NaN 的情况进行了特殊处理
  // 第一关：基础数据类型直接比较出结果
  if (is (objA, objB)) {
    return true;
  }
  // 第二关：只要有一个不是对象数据类型就返回 false
  if (
    typeof objA !== 'object' ||
    objA === null ||
    typeof objB !== 'object' ||
    objB === null
  ) {
    return false;
  }

  // 第三关：在这里已经可以保证两个都是对象数据类型，比较两者的属性数量
  const keysA = Object.keys (objA);
  const keysB = Object.keys (objB);

  if (keysA.length !== keysB.length) {
    return false;
  }

  // 第四关：比较两者的属性是否相等，值是否相等
  for (let i = 0; i < keysA.length; i++) {
    if (
      !hasOwnProperty.call (objB, keysA [i]) ||
      !is (objA [keysA [i]], objB [keysA [i]])
    ) {
      return false;
    }
  }

  return true;
}
```

## 参考资料
[1][React Hooks 性能优化](https://juejin.cn/post/7076123870063198216)
[2][关于React Hooks和Immutable性能优化的实践，我写了一本掘金小册](https://juejin.cn/post/6844904023808540680)