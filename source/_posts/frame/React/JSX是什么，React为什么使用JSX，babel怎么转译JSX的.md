---
title: JSX是什么，React为什么使用JSX，babel怎么转译JSX的
date: 2023-02-27
categories:
- 框架
- react
---

## JSX是什么，React为什么使用JSX，babel怎么转译JSX的

在前端的框架中有两种“描述UI”的方案，一种是JSX语法，一种是模板语言。

其中React就是选择的JSX，Vue就是选择的模板语言。

JSX其实就是一个语法糖，在编写React代码的时候你可以不使用JSX来进行编写。在React中，你写的JSX代码最终都会被babel编译。

```jsx
// JSX语法
const element = <h1>Hello,World!</h1>
```

```jsx
// babel编译后
var element = React.createElement("h1",null,"Hello,world!");//React17版本之前
// React17版本之后
var _jsxRuntime = require("react/jsx-runtime");
var element = _jsxRuntime.jsx("h1",{children:"Hello World!"});
```

JSX由babel转换成React.createElement或_jsxRuntime.jsx的形式，函数执行后返回虚拟DOM，所以说你可以不使用JSX，可以直接写React.createElement或_jsxRuntime.jsx的形式。
所以我们写的代码最终都会被构建成虚拟DOM树。JSX就是一种类XML语法的语法糖，让开发者来构建这个虚拟DOM树更加的方便，使代码更加的简洁。

那么babel是怎么样将JSX语法转换成React.createElement或_jsxRuntime.jsx的形式的呢？

babel编译JSX的流程分为三个部分：
1. parse：通过parse将JSX代码转换成AST。
2. transform：在transform阶段使用`@babel/plugin-transform-react-jsx`插件，它的核心就是visitor函数，通过这个函数来遍历AST，根据不同的节点类型来做不同的处理，生成了JSX对应的createElement对应的AST。
3. generate：最后由generate将AST转换为JS。
