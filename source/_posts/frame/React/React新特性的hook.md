---
title: React新hook
date: 2023-05-05
categories:
- 框架
- react
---

## React18新特性的hook

### useId

&ensp;&ensp;&ensp;&ensp;useId解决了客户端和服务端id不相同的问题。

&ensp;&ensp;&ensp;&ensp;我们看下面这段代码

```tsx
function App() {
  return (
    <>
      <Checkbox />
      <Checkbox />
    </>
  );
}

function Checkbox() {
  return (
    <>
      <label htmlFor="1">Do you like React?</label>
      <input type="checkbox" name="react" id="1" />
    </>
  );
}

export default App;
```

&ensp;&ensp;&ensp;&ensp;上面代码我们会发现Checkbox组件复用，会导致出现两个相同的id。会报一个错误`IDs used in ARIA and labels must be unique: Document has multiple elements referenced with ARIA with the same id attribute: 1`。那我们怎么解决这个问题呢？来看下面这段代码。

```tsx
function App() {
  return (
    <>
      <Checkbox />
      <Checkbox />
    </>
  );
}

function Checkbox() {

  const id = Math.random() + "";

  return (
    <>
      <label htmlFor={id}>Do you like React?</label>
      <input type="checkbox" name="react" id={id} />
    </>
  );
}

export default App;
```

&ensp;&ensp;&ensp;&ensp;使用随机数来代替固定的id使得没有报错，但是这仍然有问题，如果恰好两次随机到了一个数，或者当使用服务端渲染的时候，客户端和服务端的id就不相同了。于是在React18版本就退出了useId这个hook。

```tsx
import { useId } from 'react';
function App() {
  return (
    <>
      <Checkbox />
      <Checkbox />
    </>
  );
}

function Checkbox() {

  const id = useId();

  return (
    <>
      <label htmlFor={id}>Do you like React?</label>
      <input type="checkbox" name="react" id={id} />
    </>
  );
}

export default App;
```

&ensp;&ensp;&ensp;&ensp;上面React渲染成真实DOM后就成了下面的代码。

```html
<div id="root">
    <label for=":r1:">Do you like React?</label><input type="checkbox" name="react" id=":r1:">
    <label for=":r3:">Do you like React?</label><input type="checkbox" name="react" id=":r3:">
</div>
```

&ensp;&ensp;&ensp;&ensp;useId的原理就是每个id代表组件在组件树种的层级结构。

### useTransition

&ensp;&ensp;&ensp;&ensp;在React的状态更新中分为紧急更新和过渡更新。

- 紧急更新，输出、点击、拖拽等事件需要立即响应，不然就会有种很卡的感觉。
- 过渡更新，将UI从一个视图过渡到另一个视图，可以有些延迟。

&ensp;&ensp;&ensp;&ensp;并发模式只是提供了可中断的能力，默认情况下，所有的更新都是紧急更新，也就是更新的优先级相同。那么快速更新还是会被其他大量更新拖慢速度。我们看下面代码的示例。

```tsx
import { useEffect, useState } from "react";

function App() {

  const [value, setValue] = useState('');

  return (
    <>
      <input type="text" value={value} onChange={(e)=>{
        setValue(e.target.value)
      }} />
      <List search={value} />
    </>
  );
}

function List({search}:{search: string}){

  const [list, setList] = useState<string[]>([]);

  useEffect(()=>{
    setTimeout(()=>{
      setList(new Array(5000).fill(""))
    },100)
  },[search])

  return <ul>
    {
      list.map((_,index) => {
        return <Item key={index} value={index+"_"+search} />
      })
    }
  </ul>
}

function Item({value}:{value: string}){
  return (
    <li>{value}</li>
  )
}

export default App;
```

&ensp;&ensp;&ensp;&ensp;因为上面有5000条数据，每次都search改变100毫秒后都会重新渲染。当输入的速度很快的时候就会导致输入框特别的卡。这是因为输入框的更新优先级和列表的更新优先级是一样的，那有没有一种可能，我们将输入框的优先级提高，这样列表就不会立刻渲染，只有当输入框输入事件不再触发的时候才会渲染呢。

```tsx
import { useEffect, useState, startTransition } from "react";

function App() {

  const [value, setValue] = useState('');
  const [search, setSearch] = useState('');

  return (
    <>
      <input type="text" value={value} onChange={(e)=>{
        setValue(e.target.value)// 紧急任务
        startTransition(()=>{
          setSearch(e.target.value)// 非紧急任务
        })
      }} />
      <List search={search} />
    </>
  );
}

function List({search}:{search: string}){

  const [list, setList] = useState<string[]>([]);

  useEffect(()=>{
    setTimeout(()=>{
      setList(new Array(5000).fill(""))
    },100)
  },[search])

  return <ul>
    {
      list.map((_,index) => {
        return <Item key={index} value={index+"_"+search} />
      })
    }
  </ul>
}

function Item({value}:{value: string}){
  return (
    <li>{value}</li>
  )
}

export default App;
```

&ensp;&ensp;&ensp;&ensp;使用startTransition将search状态更新变成非优先级任务，还会出现节流的效果。我们使用useTransition来替代startTransition试一试。

```tsx
import { useEffect, useState, useTransition } from 'react';

function App() {

  const [value, setValue] = useState('');
  const [search, setSearch] = useState('');
  const [isPending, startTransition] = useTransition();

  return (
    <>
      <input type="text" value={value} onChange={(e)=>{
        setValue(e.target.value)// 紧急任务
        startTransition(()=>{
          setSearch(e.target.value)// 非紧急任务
        })
      }} />
      {isPending && 'loading……'}{/* 非紧急任务的状态 */}
      <List search={search} />
    </>
  );
}

function List({search}:{search: string}){

  const [list, setList] = useState<string[]>([]);

  useEffect(()=>{
    setTimeout(()=>{
      setList(new Array(5000).fill(""))
    },100)
  },[search])

  return <ul>
    {
      list.map((_,index) => {
        return <Item key={index} value={index+"_"+search} />
      })
    }
  </ul>
}

function Item({value}:{value: string}){
  return (
    <li>{value}</li>
  )
}

export default App;
```

### useDeferredValue

&ensp;&ensp;&ensp;&ensp;除了useTransition我们还可以使用useDeferredValue将任务变成非紧急状态

```tsx
import { useEffect, useState, useDeferredValue } from 'react';

function App() {

  const [value, setValue] = useState('');
  const [search, setSearch] = useState('');

  return (
    <>
      <input type="text" value={value} onChange={(e)=>{
        setValue(e.target.value)// 紧急任务
        setSearch(e.target.value)
      }} />
      <List search={useDeferredValue(search)} />{/* 标记非紧急状态 */}
    </>
  );
}

function List({search}:{search: string}){

  const [list, setList] = useState<string[]>([]);

  useEffect(()=>{
    setTimeout(()=>{
      setList(new Array(5000).fill(""))
    },100)
  },[search])

  return <ul>
    {
      list.map((_,index) => {
        return <Item key={index} value={index+"_"+search} />
      })
    }
  </ul>
}

function Item({value}:{value: string}){
  return (
    <li>{value}</li>
  )
}

export default App;
```

### useDeferredValue与useTransition的区别

- 不同点：useDeferredValue是把更新任务变成了`延迟更新任务`，而useTransition是把一个状态变成了`延迟状态`。
