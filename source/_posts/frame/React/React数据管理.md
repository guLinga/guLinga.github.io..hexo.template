---
title: React数据管理
date: 2023-03-06
categories:
- 框架
- react
---

## React数据管理

[代码仓库](https://github.com/guLinga/React-data-administration)

### React批量更新
React中的批量更新就是将多次更新合并处理，最终只渲染一次，来获得更好的性能。

#### React18版本之前的批量更新

```js
// react 17 react-dom 17 react-scripts 4.0.3
import * as ReactDOM from "react-dom";
import {useState} from 'react'
function App() {
  const [num1, setNum1] = useState(0);
  const [num2, setNum2] = useState(0);
  function handleClick() {
    setNum1(c => c + 1);
    setNum2(c => c + 1);
  }
  console.log("render");
  return (
    <div>
      num1:{num1}-num2:{num2}
      <button onClick={handleClick}>Next</button>
    </div>
  );
}
const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```
点击一次按钮触发setNum1和setNum2，但是render只输出一次，React将多次更新合并处理，最终只渲染一次。

然而React18版本之前，批量更新并不是所有场景都会生效。
```js
import * as ReactDOM from "react-dom";
import {useState} from 'react'
function App() {
  const [num1, setNum1] = useState(0);
  const [num2, setNum2] = useState(0);
  function handleClick() {  
    setTimeout(()=>{
      setNum1(c => c + 1);
      setNum2(c => c + 1);
    })
  }
  console.log("render");
  return (
    <div>
      num1:{num1}-num2:{num2}
      <button onClick={handleClick}>Next</button>
    </div>
  );
}
const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```
点击一次按钮触发setNum1和setNum2，然后会输出两次render，这就是在React18版本之前，React的批量更新失效了。

总结：在React18之前，我们只能在React事件处理函数中执行过程中进行批量更新。对于promise、setTimeout、原生事件处理函数或其他任何事件中的状态更新都不会进行批量更新。

#### React18自动批量更新
从React18的createRoot开始，无论在哪里，所有更新都将自动进行批量更新。
这意味着 setTimeout、promises、原生事件处理函数或其他任何事件的批量更新都将与 React 事件一样，以相同的方式进行批量更新。我们希望这样可以减少渲染工作量，从而提高应用程序的性能:

```js
// react 18.2.0 react-dom 18.2.0 react-scripts 5.0.1
import ReactDOM from 'react-dom/client';
import {useState} from 'react'
function App() {
  const [num1, setNum1] = useState(0);
  const [num2, setNum2] = useState(0);
  function handleClick() {  
    setTimeout(()=>{
      setNum1(c => c + 1);
      setNum2(c => c + 1);
    })
  }
  console.log("render");
  return (
    <div>
      num1:{num1}-num2:{num2}
      <button onClick={handleClick}>Next</button>
    </div>
  );
}
const root = ReactDOM.createRoot(
  document.getElementById('root')
);
root.render(
    <App />
);
```
点击一次按钮触发setNum1和setNum2，然后会输出一次render，这就是在React18版本，自动批量更新，所有更新都将进行批量更新。

#### 禁止批量更新
使用flushSync来包裹更新，做到禁止批量更新
```js
import ReactDOM from 'react-dom/client';
import {flushSync} from 'react-dom';
import {useState} from 'react';
function App() {
  const [num1, setNum1] = useState(0);
  const [num2, setNum2] = useState(0);
  function handleClick() {  
    setTimeout(()=>{
      flushSync(()=>{
        setNum1(c => c + 1);
      })
      flushSync(()=>{
        setNum2(c => c + 1);
      })
    })
  }
  console.log("render");
  return (
    <div>
      num1:{num1}-num2:{num2}
      <button onClick={handleClick}>Next</button>
    </div>
  );
}
const root = ReactDOM.createRoot(
  document.getElementById('root')
);
root.render(
    <App />
);
```

#### React18的自动批量更新对Hooks的影响
如果你正在使用 Hooks，在绝大多数情况下批量更新都能“正常工作”。

#### React18的自动批量更新对Classes的影响

如果在React17版本之前，没有在React事件处理函数中执行的代码不会批量执行，同时它也是同步执行，而在React18版本中，都是批量更新和异步执行的。所以会导致React18版本和React18版本之前的输出不一样。

在React18版本之前
```js
import React, { Component } from 'react'
import ReactDOM from 'react-dom'
export default class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      num1: 1,
      num2: 1
    }
  }
  handleClick(){
    setTimeout(()=>{
      this.setState(({num1})=>({
        num1: num1+1
      }))
      console.log(this.state);
      this.setState(({num2})=>({
        num2: num2+1
      }))
    })
  }
  render() {
    console.log('render');
    return (
      <div>
        num1:{this.state.num1}-num2:{this.state.num2}
        <button onClick={()=>{
          this.handleClick()
        }}>Next</button>
      </div>
    )
  }
}
const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
/*
  上述代码点击的时候会输出一下内容：
  render
  {num1: 2, num2: 1}
  render
*/
```

在React18版本
```js
import React, { Component } from 'react'
import ReactDOM from 'react-dom/client';
export default class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      num1: 1,
      num2: 1
    }
  }
  handleClick(){
    setTimeout(()=>{
      this.setState(({num1})=>({
        num1: num1+1
      }))
      console.log(this.state);
      this.setState(({num2})=>({
        num2: num2+1
      }))
    })
  }
  render() {
    console.log('render');
    return (
      <div>
        num1:{this.state.num1}-num2:{this.state.num2}
        <button onClick={()=>{
          this.handleClick()
        }}>Next</button>
      </div>
    )
  }
}
const root = ReactDOM.createRoot(
  document.getElementById('root')
);
root.render(
    <App />
);
/*
  上述代码点击的时候会输出一下内容：
  {num1: 1, num2: 1}
  render
*/
```

如果想要在React18中按照React18版本之前的输出需要使用flushSync来进行修改
```js
import React, { Component } from 'react'
import { flushSync } from 'react-dom';
import ReactDOM from 'react-dom/client';
export default class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      num1: 1,
      num2: 1
    }
  }
  handleClick(){
    setTimeout(()=>{
      flushSync(()=>{
        this.setState(({num1})=>({
          num1: num1+1
        }))
      })
      console.log(this.state);
      this.setState(({num2})=>({
        num2: num2+1
      }))
    })
  }
  render() {
    console.log('render');
    return (
      <div>
        num1:{this.state.num1}-num2:{this.state.num2}
        <button onClick={()=>{
          this.handleClick()
        }}>Next</button>
      </div>
    )
  }
}
const root = ReactDOM.createRoot(
  document.getElementById('root')
);
root.render(
    <App />
);
/*
  上述代码点击的时候会输出一下内容：
  {num1: 1, num2: 1}
  render
*/
```


### useState是同步还是异步

先上结论：
1. 在React18版本之前，useState在React合成事件和hooks中是异步的，其他情况都是同步的，例如，原生事件、setTimeout、promise等
2. 在React18版本，useState在React中都是异步的

#### React18版本之前的this.setState
```js
import * as ReactDOM from "react-dom";
import React,{useState,useEffect} from 'react'
class App extends React.Component {
  state = {
    count: 0
  }
  componentDidMount() {
    this.setState({count: this.state.count + 1})
    console.log(this.state.count);
    this.setState({count: this.state.count + 1})
    console.log(this.state.count);
    setTimeout(() => {
      this.setState({count: this.state.count + 1})
      console.log(this.state.count);
      this.setState({count: this.state.count + 1})
      console.log(this.state.count);
    });
  }
  render() {
    return <h1>Count: {this.state.count}</h1>
  }
}
const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```
上述代码输出`0 0 2 3`，我们来分析一下为什么这样输出。
1. 首先进入`componentDidMount`，因为this.setState是异步执行的，所以先执行同步代码，执行两次`console.log(this.state.count);`，输出两次0
2. 在`componentDidMount`中，this.setState导致的更新会进行合并，批量更新，只会更新最后一个，所以当还没执行`setTimeout`之前，更新`count`，这时的`count`变成了1
3. 进入`setTimeout`执行，在`setTimeout`中，this.setState是同步执行的，并且没有批量更新
4. 执行`this.setState({count: this.state.count + 1})`，这时`count`同步更新为2，执行`console.log(this.state.count);`输出2
5. 执行`this.setState({count: this.state.count + 1})`，这时`count`同步更新为3，执行`console.log(this.state.count);`输出4

所以最终输出`0 0 2 3`。

#### React18版本之前的this.setState怎么在setTimeout实现异步批量更新(unstable_batchedUpdates)
this.setState怎么在setTimeout实现异步批量更新，React提供了一种解决方案，就是从`react-dom`中暴露一个API:`unstable_batchedUpdates`，我们看一下具体的用法。

```js
import * as ReactDOM from "react-dom";
import React,{useState,useEffect} from 'react'
class App extends React.Component {
  state = {
      count: 0
  }
  componentDidMount() {
      this.setState({count: this.state.count + 1})
      console.log(this.state.count);
      this.setState({count: this.state.count + 1})
      console.log(this.state.count)
      setTimeout(() => {
          ReactDOM.unstable_batchedUpdates(() => {
              this.setState({count: this.state.count + 1})
              console.log(this.state.count)
              this.setState({count: this.state.count + 1})
              console.log(this.state.count)
          }) 
      })
  }
  render() {
      return <h1>Count: {this.state.count}</h1>
  }
}
const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```
上述代码输出`0 0 1 1`，我们来分析一下为什么这样输出。
1. 首先进入`componentDidMount`，因为this.setState是异步执行的，所以先执行同步代码，执行两次`console.log(this.state.count);`，输出两次0
2. 在`componentDidMount`中，this.setState导致的更新会进行合并，批量更新，只会更新最后一个，所以当还没执行`setTimeout`之前，更新`count`，这时的`count`变成了1
3. 进入`setTimeout`执行，因为`setTimeout`中的代码用了`unstable_batchedUpdates`嵌套，所以也是异步批量更新。
4. 先执行同步代码，执行两次`console.log(this.state.count)`，输出两次1。
5. 执行`this.setState({count: this.state.count + 1})`，此时`count`更新为2。
5. 执行`this.setState({count: this.state.count + 1})`，此时`count`更新为3。

#### React18的解决方案
如果在React17版本中，想要在`setTimeout`中执行异步批量更新，需要在每个`setTimeout`中都嵌套`unstable_batchedUpdates`，React18中解决了这个痛点，无论在哪里的更新都是异步批量更新。
```js
import ReactDOM from 'react-dom/client';
import {flushSync} from 'react-dom';
import React from 'react';
class App extends React.Component {
  state = {
    count: 0
  }
  componentDidMount() {
    this.setState({count: this.state.count + 1})
    console.log(this.state.count);
    this.setState({count: this.state.count + 1})
    console.log(this.state.count);
    setTimeout(() => {
      this.setState({count: this.state.count + 1})
      console.log(this.state.count);
      this.setState({count: this.state.count + 1})
      console.log(this.state.count);
    });
  }
  render() {
    return <h1>React18 Count: {this.state.count}</h1>
  }
}
const root = ReactDOM.createRoot(
  document.getElementById('root')
);
root.render(
    <App />
);
```
上述代码输出`0 0 1 1`，我们来分析一下为什么这样输出。
1. 首先进入`componentDidMount`，因为this.setState是异步执行的，所以先执行同步代码，执行两次`console.log(this.state.count);`，输出两次0
2. 在`componentDidMount`中，this.setState导致的更新会进行合并，批量更新，只会更新最后一个，所以当还没执行`setTimeout`之前，更新`count`，这时的`count`变成了1
3. 进入`setTimeout`执行，在React18中，`setTimeout`中是异步批量更新。
4. 先执行同步代码，执行两次`console.log(this.state.count)`，输出两次1。
5. 执行`this.setState({count: this.state.count + 1})`，此时`count`更新为2。
5. 执行`this.setState({count: this.state.count + 1})`，此时`count`更新为3。