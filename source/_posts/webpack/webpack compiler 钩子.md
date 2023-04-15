---
title: webpack compiler 钩子
date: 2023-04-15
categories: webpack
---

## webpack compiler 钩子

### compiler
compiler的hooks属性上面携带了插件执行的生命周期钩子。你可以调用不同的钩子来控制不同时刻的执行。

### compilation
compilation对象记录模块的信息，只要项目文件改动，就会重新生成compilation，compilation上面有很多方法可以供我们使用。
[webpack官网compilation对象](https://webpack.docschina.org/api/compilation-object/)

### compiler 钩子

[官网compiler钩子地址](https://webpack.docschina.org/api/compiler-hooks/)

常用生命周期钩子
1. beforeRun 编译器执行前
2. run 编译器开始读取记录前
3. beforeCompile 开始编译之前
4. compile 新的compilation创建之前
5. compilation compilation 创建之后执行
6. emit 文件提交到dist目录前
7. afterEmit 输出 asset 到 output 目录之后执行
8. make compilation 结束之前执行
9. done 在 compilation 完成时执行

