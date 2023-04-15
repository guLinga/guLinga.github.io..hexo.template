---
title: webpack loader
date: 2023-04-15
categories: webpack
---

## webpack loader

### 定义
简单来说loader就是把webpack不能直接处理的资源变成可以直接处理。
loader本质上是一个js模块，导出的是一个函数。

### loader函数的结构
```js
/**
 * 
 * @param {string|Buffer} context 
 * @param {object} map 可以被 https://github.com/mozilla/source-map 使用的 SourceMap 数据
 * @param {any} meta meta 数据，可以是任何内容
 * @returns context
 */
// 其中 map 和 meta 是可选参
function Demo(context,map,meta){
  console.log("context",context);
  console.log("map",map);
  console.log("meta",meta);
  return context;
}

module.exports = Demo;
```
```js
// webpack.config.js
const path = require('path');
module.exports = {
  mode: 'none',
  entry: './src/index.js',
  output: {
    filename: 'build.js',
    path: path.join(__dirname,'dist')
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        loader: path.join(__dirname,'./webpack-loader/demo.js')
      }
    ]
  }
}
```

### loader优先级及其执行顺序

loader中有四类：
1. pre 前置loader
2. normal 普通loader
3. inline 内联loader
4. post后置loader

默认的执行优先级为 pre，normal，inline，post
相同优先级的loader执行顺序为 从右往左，从下往上

通过下面的代码输出顺序为`Three`、`Second`、`First`，可以验证相同优先级的loader执行顺序为`从右往左，从下往上`。

```js
// webpack.config.js
const path = require('path');
module.exports = {
  mode: 'none',
  entry: './src/index.js',
  output: {
    filename: 'build.js',
    path: path.join(__dirname,'dist')
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        loader: path.join(__dirname,'./webpack-loader/first.js')
      },
      {
        test: /\.js$/,
        use: [
          {loader: path.join(__dirname,'./webpack-loader/second.js')},
          {loader: path.join(__dirname,'./webpack-loader/three.js')}
        ]
      }
    ]
  }
}
```

给loader加上不同的优先级来改变执行顺序。输出顺序为`First`、`Three`、`Second`。
```js
const path = require('path');
module.exports = {
  mode: 'none',
  entry: './src/index.js',
  output: {
    filename: 'build.js',
    path: path.join(__dirname,'dist')
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        loader: path.join(__dirname,'./webpack-loader/first.js'),
        enforce: 'pre'
      },
      {
        test: /\.js$/,
        use: [
          {loader: path.join(__dirname,'./webpack-loader/second.js')},
          {loader: path.join(__dirname,'./webpack-loader/three.js')}
        ],
        enforce: 'post'
      }
    ]
  }
}
```

### 限制loader在哪些位置查找
可以使用`resolveLoader.modules`来限制loader的查找位置。
```js
// webpack.config.js
module.exports = {
  mode: 'none',
  entry: './src/index.js',
  output: {
    filename: 'build.js',
    path: path.join(__dirname,'dist')
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        loader: path.join(__dirname,'./webpack-loader/first.js'),
        enforce: 'pre'
      },
      {
        test: /\.js$/,
        use: [
          {loader: path.join(__dirname,'./webpack-loader/second.js')},
          {loader: path.join(__dirname,'./webpack-loader/three.js')}
        ],
        enforce: 'post'
      }
    ]
  },
  resolveLoader: {
    modules: [
      path.join(__dirname,'node_modules'),
      path.join(__dirname,'webpack-loader')
    ]
  }
}
```

### loader的使用方式分类
loader在使用方式上分为`同步loader`、`异步loader`、`raw loader`、`pitch loader`四类。

1. 同步loader
```js
function Sync(content,map,meta){
  console.log('同步loader');
  this.callback(null,content,map,meta);
}

module.exports = Sync;
```
2. 异步loader
异步loader并不是让出当前loader的执行权力让下一个的loader先执行，而是卡住当前执行进程，方便你再异步的时间里进行一些额外的操作。等到操作完成后任务进程交给下一个loader。
```js
function Async(content,map,mate){ 
  console.log('异步 loader 外');
  // this.async 告诉 loader-runner 这个 loader 将会异步地回调。返回 this.callback。
  const callback = this.async();
  setTimeout(()=>{
    console.log('异步 loader 内');
    // 调用 callback 后，才会执行下一个 loader
    callback(null,content,map,mate);
  },500)
}

module.exports = Async;
```
下面代码输出`Second`、`异步 loader 外`、`异步 loader 内`、`First`，`Second`、`异步 loader 外`输出后等到0.5s后再输出后面的，说明异步的loader类似于async中的await机制，等到结果返回后再继续执行后面的代码。
```js
const path = require('path');
module.exports = {
  mode: 'none',
  entry: './src/index.js',
  output: {
    filename: 'build.js',
    path: path.join(__dirname,'dist')
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        loader: path.join(__dirname,'./webpack-loader/first.js')
      },
      {
        test: /\.js$/,
        use: path.join(__dirname,'./webpack-loader/async.js')
      },
      {
        test: /\.js$/,
        use: [
          {loader: path.join(__dirname,'./webpack-loader/second.js')}
        ]
      }
    ]
  },
  resolveLoader: {
    modules: [
      path.join(__dirname,'node_modules'),
      path.join(__dirname,'webpack-loader')
    ]
  }
}
```
3. raw loader
raw loader一半用于处理 Buffer 数据流文件，在处理图片，字体图标经常用到。
```js
function Raw(content){
  console.log(content);
  return content;
}
Raw.raw = true;
module.exports = Raw;
```
```js
const path = require('path');
module.exports = {
  mode: 'none',
  entry: './src/index.js',
  output: {
    filename: 'build.js',
    path: path.join(__dirname,'dist')
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        loader: 'raw'
      }
    ]
  },
  resolveLoader: {
    modules: [
      path.join(__dirname,'node_modules'),
      path.join(__dirname,'webpack-loader')
    ]
  }
}
```
上面代码输出内容为：
```text
<Buffer 2f 2f 20 69 6d 70 6f 72 74 20 42 75 74 74 6f 6e 20 66 72 6f 6d 20 27 2e 2f 63 6f 6d 70 6f 6e 65 6e 74 73 2f 42 75 74 74 6f 6e 27 0d 0a 0d 0a 2f 2f 20 ... 113 more bytes>
```
4. pitch loader
```js
// loader1.js
function loader1(content,map,mate){
  console.log('同步 loader1');
  this.callback(null,content,map,mate);
}

function pitch1(remainingRequest, precedingRequest, data){
  console.log('pitch loader1');
}

module.exports = loader1;
module.exports.pitch = pitch1;
```
```js
// loader2.js
function loader2(content, map, meta) {
  console.log('异步 loader2 外')
  const callback = this.async()
  setTimeout(() => {
    console.log('异步 loader2 内')
    callback(null, content, map, meta)
  },500)
}

function pitch2() {
  console.log('pitch loader2')
}

module.exports = loader2
module.exports.pitch = pitch2
```
```js
// loader3.js
function loader3(content, map, meta) {
  console.log('loader3')
  this.callback(null, content, map, meta)
}

function pitch3(remainingRequest, precedingRequest, data) {
  console.log('pitch loader3')
}
module.exports = loader3
module.exports.pitch = pitch3
```
```js
// webpack.config.js
const path = require('path');
module.exports = {
  mode: 'none',
  entry: './src/index.js',
  output: {
    filename: 'build.js',
    path: path.join(__dirname,'dist')
  },
  module:{
    rules:[{
      test: /\.js$/,
      loader: 'loader1',
    },{
      test: /\.js$/,
      loader: 'loader2',
    },{
      test: /\.js$/,
      loader: 'loader3',
    }]
  },
  resolveLoader: {
    modules: [
      path.join(__dirname,'node_modules'),
      path.join(__dirname,'webpack-loader')
    ]
  }
}
```
上面代码的执行顺序是`pitch loader1`、`pitch loader2`、`pitch loader3`、`loader3`、`异步 loader2 外`、`异步 loader2 内`、`同步 loader1`。
由此可见，pitch的执行顺序是`从上到下，从左到右`。

上面的pitch loader都是无返回值的情况，如果有返回值执行顺序又会发生改变。
```js
function pitch2() {
  console.log('pitch loader2')
  return 'console.log("pitch2 return")'
}
```
讲pitch2改成上面的代码，运行程序后输出`pitch loader1`、`pitch loader2`、`同步 loader1`，如果pitch有返回值会阻断后面的执行，直接执行前一个loader的normal loader（没有enforce属性默认是normal loader）。

**熔断时要注意，如果pitch中return一个不可执行字符串，例如"pitch loader"，在normal loader中return需要做处理，处理成可执行字符串，例如console.log("pitch loader")，否则会报错`You may need an additional loader to handle the result of these loaders.`，还要注意的是如果pitch中return的是一个连贯的字符串，也不会报错，例如return "123"。**

**我费劲心思的想在pitch中获取对应loader的content参数，还尝试在loader中获取原本的content，但是都失败了，所以如果想pitch想要想loader传参还是推荐使用data的方式。**

5. pitch传参
```js
// loader1.js
function loader1(content,map,meta){
  console.log('同步 loader1',content,map,meta);
  this.callback(null,content,map,meta);
}

function pitch1(remainingRequest, precedingRequest, data){
  console.log('pitch loader1', data);
}

module.exports = loader1;
module.exports.pitch = pitch1;
```
```js
// loader2.js
function loader2(content, map, meta) {
  console.log('异步 loader2 外',content, map, meta)
  const callback = this.async()
  setTimeout(() => {
    console.log('异步 loader2 内')
    callback(null, content, map, this.data.value)
  },500)
}

function pitch2(remainingRequest, precedingRequest, data) {
  data.value = 1;
  console.log('pitch loader2', data)
}

module.exports = loader2
module.exports.pitch = pitch2
```
```js
// loader3.js
function loader3(content, map, meta) {
  console.log('loader3',content, map, meta)
  this.callback(null, content, map, meta)
}

function pitch3(remainingRequest, precedingRequest, data) {
  console.log('pitch loader3', data)
}
module.exports = loader3
module.exports.pitch = pitch3
```
运行webpack进行打包，输出为:
```js
pitch loader1 {}
pitch loader2 { value: 1 }
pitch loader3 {}
loader3 console.log('index入口'); undefined undefined
异步 loader2 外 console.log('index入口'); undefined undefined
异步 loader2 内
同步 loader1 console.log('index入口'); undefined 1
```
我们可以看到，我们在pitch2中给data添加了value属性，在loader2中可以通过this.data.value来获取。在loader2中callback让后面的loader可以通过meta获取到该参数。

### 手写loader练习
1. clean-log-loader 清除所有的console.log语句
```js
function CleanLogLoader(content){
  return content.replace(/console\.log\(.*\);?/g,"");
}

module.exports = CleanLogLoader;
```
2. banner-loader 用于给代码添加注释信息
```json
// banner-loader.json
{
  "type":"object",
  "properties":{
    "author":{
      "type": "string"
    }
  },
  "additionalProperties" : false
}
```
```js
// banner-loader.js
const schema = require('./banner-loader.json');

function BannerLoader(content){
  const options = this.getOptions(schema);
  const prefix = `
    /**
     * author: ${options.author}
     */ 
  `
  return prefix + content;
}

module.exports = BannerLoader;
```
```js
// webpack.config.js
const path = require('path');
module.exports = {
  mode: 'none',
  entry: './src/index.js',
  output: {
    filename: 'build.js',
    path: path.join(__dirname,'dist')
  },
  module: {
    rules: [
      {test:/\.js$/,use: [
        {loader: 'banner-loader',options:{
          author: 'test'
        }}
      ]}
    ]
  },
  resolveLoader: {
    modules: [
      path.join(__dirname,'node_module'),
      path.join(__dirname,'./webpack-loader/write')
    ]
  }
}
```
### loader执行原理
小结： normal loader 从下往上，从右往左；pitch loader 与之相反。优先级权重为 pre，normal，inline，post。熔断时 pitch loader 返回上一个loader的 normal loader。

1. loader执行栈

loader执行栈分为`loader分类`和`生成执行栈`，loader分类主要是webpack将获取到的loader按照pre、normal、inline、post分类。

webpack会将匹配到的loader按照post、inline、normal、pre的顺序压入执行栈。这与上面说的按照pre、normal、inline、post顺序执行正好相反，这是因为loader运行的时候顺序是反向的。

2. loader runner的运行
loader runner运行loader，主要包括pitch和normal两个阶段。分别对应loader runner的iteratePitchingLoader和iterateNormalLoader两个方法。
interatePitchingLoader递归执行，同时记录loader的pitch状态，与当前的loaderIndex，当到达最大值时也就是执行栈的长度，所有的loader pitch已经执行完毕。开始处理module，调用processResource方法处理模块，loaderIndex--，并递归调用interateNormalLoader。
```js
// abort after last loader
if(loaderContext.loaderIndex >= loaderContext.loaders.length)
   return processResource(options, loaderContext, callback);
```
```js
// iterate
if(currentLoaderObject.pitchExecuted) {
  loaderContext.loaderIndex++;
  return iteratePitchingLoaders(options, loaderContext, callback);
}
```
```js
loaderContext.loaderIndex--;
iterateNormalLoaders(options, loaderContext, args, callback);
```
3. 熔断原理
在 loader runner 中，当 loaderIndex 达不到 loader 本身的长度时（有 pitch loader 提前 return 发生了熔断）时， processResource 这个方法是不会触发的，这就导致 addDependency 这个方法也不会触发，因此不会将该模块资源添加进依赖，无法读取模块的内容。继而熔断后续操作。
4. loader异步处理
在loader中调用 this.async 时，实际是将 loaderContext 上的 async 属性赋值为一个函数。isSync 变量默认为 true，当 loader 中使用 this.async 时，它被置为 false，并返回一个 innerCallback 作为异步回调完成的通知。
```js
context.async = function async() {
  if(isDone) {
    if(reportedError) return; // ignore
    throw new Error("async(): The callback was already called.");
  }
  isSync = false;
  return innerCallback;
}
```
当 isSync 为 true 时，会在 loader function 执行完毕后同步回调 callback 继续 loader runner 的执行流程。
```js
if(isSync) {
  isDone = true;
  if(result === undefined)
    return callback();
  if(result && typeof result === "object" && typeof result.then === "function") {
    return result.then(function(r) {
      callback(null, r);
    }, callback);
  }
  return callback(null, result);
}
```
5. loader中的this
webpack官方对于loader的定义中有这样一段，函数中的 this 作为上下文会被 webpack 填充。那这个this到底是什么呢？是webpack的实例吗，其实不是，这个this是 loader runner 中的 loaderContext，我们熟悉的 async，callback 等都来自于这个对象。
```js
// prepare loader objects
loaders = loaders.map(createLoaderObject);

loaderContext.context = contextDirectory;
loaderContext.loaderIndex = 0;
loaderContext.loaders = loaders;
loaderContext.resourcePath = resourcePath;
loaderContext.resourceQuery = resourceQuery;
loaderContext.resourceFragment = resourceFragment;
loaderContext.async = null;
loaderContext.callback = null;
// ...
```
你可以在这上面查看this的参数[官网Loader Context](https://webpack.docschina.org/api/loaders/#the-loader-context)

### 小结
1. loader的优先级顺序为pre、normal、inline、post。
2. loader在normal中的执行顺序为`从下到上，从右到左`。
3. loader中pitch的执行顺序正好和normal相反，为`从上到下，从左到右`。loader的熔断机制可以改变执行顺序。
4. 介绍了loader的四种类型，同步loader、异步loader、raw loader、pitch loader。
5. 手写练习了两个loader。
6. 了解了loader runner运行的机制和熔断机制。