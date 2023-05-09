---
title: 慢啃Webpack
date: 2023-05-04
categories: webpack
password: webpack test end
---

## 慢啃Webpack

&ensp;&ensp;&ensp;&ensp;很多人在学习webpack的时候都会将Webpack与工程化联系起来，认为webpack就是前端工程化，我当时也是这样认为的，想要学习好Webpack就需要放下偏见，首先要知道webpack的定义。webpack是前端的构建工具，是一个静态模块打包工具。他就是一个打包工具仅此而已。那么webpack是将什么打包成什么呢？webpack官网给出了一张图片。

<div style="background: #2b3a42; padding: 0 20px">
<svg viewBox="0 0 1088 615" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
    <g stroke-width="1" fill="none" fill-rule="evenodd">
        <text font-family="'Source Sans Pro', sans-serif" font-size="18" font-weight="600" fill="#86A5BA">
            <tspan x="933.778" y="459">静态资源</tspan>
        </text>
        <g transform="translate(1002, 326)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-1"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="84" height="84" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="18.891" y="46.7096774">.png</tspan>
            </text>
        </g>
        <g transform="translate(1002, 214)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-2"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="84" height="84" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="22.532" y="46.7096774">.css</tspan>
            </text>
        </g>
        <g transform="translate(894, 326)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-3"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="84" height="84" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="21.817" y="46.7096774">.jpg</tspan>
            </text>
        </g>
        <g transform="translate(894, 214)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-4"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="84" height="84" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="29" y="46.7096774">.js</tspan>
            </text>
        </g>
        <g transform="translate(342, 225)" stroke="#7E8C94" stroke-width="4">
            <path d="M499.558824,86.52 C499.558824,86.52 484.852941,81.02 439.908088,109.436667 C394.963235,137.853333 380.992647,164.436667 380.992647,164.436667" stroke-dasharray="7"></path>
            <path d="M499.558824,86.0616667 C499.558824,86.0616667 484.852941,91.5616667 439.908088,63.145 C394.963235,34.7283333 380.992647,8.145 380.992647,8.145" stroke-dasharray="7"></path>
            <path d="M0.477941176,170.395 C0.477941176,170.395 169.382939,98.895 447.847936,98.895" stroke-dasharray="6"></path>
            <path d="M0.477941176,72.395 C0.477941176,72.395 169.382939,0.895 447.847936,0.895" stroke-dasharray="6" transform="translate(224.162939, 36.645000) scale(1, -1) translate(-224.162939, -36.645000) "></path>
        </g>
        <text font-family="'Source Sans Pro', sans-serif" font-size="18" font-weight="600" fill="#86A5BA">
            <tspan x="24.934" y="562">模块依赖</tspan>
        </text>
        <g transform="translate(228, 335)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-5"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="66" height="66" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="12.817" y="38">.jpg</tspan>
            </text>
        </g>
        <g transform="translate(228, 223)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-6"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="66" height="66" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="9.891" y="38">.png</tspan>
            </text>
        </g>
        <g transform="translate(302, 414.500000) scale(1, -1) translate(-302, -414.500000) translate(182, 404)">
            <rect fill="#BBDBEC" x="0" y="0" width="2" height="6"></rect>
            <rect fill="#BBDBEC" x="76" y="6" width="2" height="12"></rect>
            <rect fill="#BBDBEC" transform="translate(75.304690, 4.704683) rotate(-45) translate(-75.304690, -4.704683) " x="74.3046896" y="1.87968342" width="2" height="5.6500001"></rect>
            <rect fill="#BBDBEC" x="2" y="2" width="72" height="2"></rect>
            <polyline stroke="#BBDBEC" stroke-width="2" points="80 12 77 20.8000002 74 12"></polyline>
        </g>
        <g transform="translate(116, 391)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-7"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="66" height="66" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="8.076" y="38">.sass</tspan>
            </text>
        </g>
        <g transform="translate(116, 279)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-8"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="66" height="66" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="8.076" y="38">.sass</tspan>
            </text>
        </g>
        <g transform="translate(182, 201)">
            <rect fill="#BBDBEC" x="0" y="0" width="2" height="6"></rect>
            <rect fill="#BBDBEC" x="76" y="113" width="2" height="12"></rect>
            <rect fill="#BBDBEC" transform="translate(75.304690, 111.704683) rotate(-45) translate(-75.304690, -111.704683) " x="74.3046896" y="108.879683" width="2" height="5.6500001"></rect>
            <rect fill="#BBDBEC" x="26" y="109" width="48" height="2"></rect>
            <rect fill="#BBDBEC" transform="translate(24.704683, 108.304690) rotate(-45) translate(-24.704683, -108.304690) " x="23.7046835" y="105.47969" width="2" height="5.6500001"></rect>
            <rect fill="#BBDBEC" x="22" y="6" width="2" height="101"></rect>
            <rect fill="#BBDBEC" transform="translate(21.304690, 4.704683) rotate(-45) translate(-21.304690, -4.704683) " x="20.3046896" y="1.87968342" width="2" height="5.6500001"></rect>
            <rect fill="#BBDBEC" x="2" y="2" width="18" height="2"></rect>
            <polyline stroke="#BBDBEC" stroke-width="2" points="80 118 77 126.8 74 118"></polyline>
        </g>
        <g transform="translate(182, 189)">
            <rect fill="#BBDBEC" x="0" y="0" width="2" height="6"></rect>
            <rect fill="#BBDBEC" x="76" y="6" width="2" height="19"></rect>
            <rect fill="#BBDBEC" transform="translate(75.304690, 4.704683) rotate(-45) translate(-75.304690, -4.704683) " x="74.3046896" y="1.87968342" width="2" height="5.6500001"></rect>
            <rect fill="#BBDBEC" x="2" y="2" width="72" height="2"></rect>
            <polyline stroke="#BBDBEC" stroke-width="2" points="80 18 77 26.8000002 74 18"></polyline>
        </g>
        <g transform="translate(116, 167)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-9"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="66" height="66" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="20" y="38">.js</tspan>
            </text>
        </g>
        <g transform="translate(110, 470.500000) scale(1, -1) translate(-190, -470.500000) translate(150, 460)">
            <rect fill="#BBDBEC" x="0" y="0" width="2" height="6"></rect>
            <rect fill="#BBDBEC" x="76" y="6" width="2" height="12"></rect>
            <rect fill="#BBDBEC" transform="translate(75.304690, 4.704683) rotate(-45) translate(-75.304690, -4.704683) " x="74.3046896" y="1.87968342" width="2" height="5.6500001"></rect>
            <rect fill="#BBDBEC" x="2" y="2" width="72" height="2"></rect>
            <polyline stroke="#BBDBEC" stroke-width="2" points="80 12 77 20.8000002 74 12"></polyline>
        </g>
        <g transform="translate(4, 447)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-10"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="66" height="66" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="8.076" y="38">.sass</tspan>
            </text>
        </g>
        <g transform="translate(70, 363)">
            <rect fill="#BBDBEC" x="0" y="0" width="2" height="6"></rect>
            <rect fill="#BBDBEC" x="76" y="6" width="2" height="12"></rect>
            <rect fill="#BBDBEC" transform="translate(75.304690, 4.704683) rotate(-45) translate(-75.304690, -4.704683) " x="74.3046896" y="1.87968342" width="2" height="5.6500001"></rect>
            <rect fill="#BBDBEC" x="2" y="2" width="72" height="2"></rect>
            <polyline stroke="#BBDBEC" stroke-width="2" points="80 12 77 20.8000002 74 12"></polyline>
        </g>
        <g transform="translate(4, 335)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-11"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="66" height="66" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="15.38" y="38">.cjs</tspan>
            </text>
        </g>
        <g transform="translate(38, 307)">
            <rect fill="#BBDBEC" x="0" y="22" width="6" height="2"></rect>
            <rect fill="#BBDBEC" x="2" y="6" width="2" height="16"></rect>
            <rect fill="#BBDBEC" transform="translate(4.704683, 4.704683) rotate(45) translate(-4.704683, -4.704683) " x="3.70468347" y="1.87968342" width="2" height="5.6500001"></rect>
            <rect fill="#BBDBEC" x="6" y="2" width="62" height="2"></rect>
            <polyline stroke="#BBDBEC" stroke-width="2" transform="translate(66.400000, 3) rotate(270) translate(-66.400000, -3) " points="69.4000001 -1.4000001 66.4000001 7.4000001 63.4000001 -1.4000001"></polyline>
        </g>
        <g transform="translate(26, 289)">
            <polyline stroke="#BBDBEC" stroke-width="2" points="6 30 3 38.8000002 0 30"></polyline>
            <rect fill="#BBDBEC" x="2" y="0" width="2" height="39"></rect>
            <rect fill="#BBDBEC" x="0" y="0" width="6" height="2"></rect>
        </g>
        <g transform="translate(110, 246.500000) scale(1, -1) translate(-190, -246.500000) translate(150, 236)">
            <rect fill="#BBDBEC" x="0" y="0" width="2" height="6"></rect>
            <rect fill="#BBDBEC" x="76" y="6" width="2" height="12"></rect>
            <rect fill="#BBDBEC" transform="translate(75.304690, 4.704683) rotate(-45) translate(-75.304690, -4.704683) " x="74.3046896" y="1.87968342" width="2" height="5.6500001"></rect>
            <rect fill="#BBDBEC" x="2" y="2" width="72" height="2"></rect>
            <polyline stroke="#BBDBEC" stroke-width="2" points="80 12 77 20.8000002 74 12"></polyline>
        </g>
        <g transform="translate(4, 223)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-12"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="66" height="66" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="10.947" y="38">.hbs</tspan>
            </text>
        </g>
        <g transform="translate(32, 177)">
            <polyline stroke="#BBDBEC" stroke-width="2" points="6 30 3 38.8000002 0 30"></polyline>
            <rect fill="#BBDBEC" x="2" y="0" width="2" height="39"></rect>
            <rect fill="#BBDBEC" x="0" y="0" width="6" height="2"></rect>
        </g>
        <g transform="translate(4, 111)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-13"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="66" height="66" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="20" y="38">.js</tspan>
            </text>
        </g>
    </g>
  </svg>
</div>

&ensp;&ensp;&ensp;&ensp;如图所示，在前端的代码中，我们会写一下ts、esnext、less、sass、vue、jsx、tsx等代码，这些代码是不能直接交给浏览器运行的，所以这就需要我们将这些代码转换成浏览器可以运行的代码，这就是webpack的功能，但webpack不仅限于此。接下来就让我们慢啃webpack，真正的明白webpack是什么，可以做什么。

### 配置

&ensp;&ensp;&ensp;&ensp;webpack中最重要的就是webpack的配置了，webpack会按照我们的配置来进行不同的打包。我们来具体了解一下webpack都有哪些配置。

&ensp;&ensp;&ensp;&ensp;初始化webpack项目

```yaml
npm i webpack -g # 全局安装webpack

#创建目录 进入目录

npm init -y # 初始化npm

# 创建index.js和webpack.config.js文件

# 文件结构
|-- index.js
|-- package.json
|-- webpack.config.js
```

#### Entry

&ensp;&ensp;&ensp;&ensp;**entry**

&ensp;&ensp;&ensp;&ensp;`entry`是配置模块的入口，webpack打包的时候肯定需要一个入口文件，不然webpack需要从哪里开始呢？`entry`就是配置打包的入口。webpack会从入口开始搜寻递归解析入口文件所有依赖的模块[^入口文件引入的依赖模块]，`entry`是必填的，如果没有填会导致webpack报错退出。

&ensp;&ensp;&ensp;&ensp;我们来修改一下`index.js`和`webpack.config.js`文件。

```js
// index.js
console.log('webpack!!!');
```

```js
// webpack.config.js
module.exports = {
  mode: 'none',
  entry: './index.js'
}
```

&ensp;&ensp;&ensp;&ensp;`webpack.config.js`中的`mode`属性是打包使用的模式，我们使用`none`。当我们运行在控制台输入`webpack`后会在根目录生产一个`dist`目录，就是打包好的目录。

&ensp;&ensp;&ensp;&ensp;**context**

&ensp;&ensp;&ensp;&ensp;`context`是`webpack`在寻找相对路径的文件时会以context为根目录，如果没有context属性，默认是`webpack`启动时所在的目录。我们修改目录文件与文件中的代码。

```yaml
|--src
|--index.js
|--package.json
|--webpack.config.js
```

```js
// webpack.config.js
const path = require('path');

module.exports = {
  mode: 'none',
  entry: './index.js',
  context: path.resolve(__dirname)
}
```

&ensp;&ensp;&ensp;&ensp;修改完代码后再次运行`webpack`会生成新的`dist/main.js`文件，如果你用的是vscode，你可以安装`Code Runner`插件，来运行`dist/main.js`文件，会有下面的输出。

```yaml
webpack!!!
```

&ensp;&ensp;&ensp;&ensp;如果我们修改`context`为`path.resolve(__dirname,'src')`，运行`webpack`会出现报错。找不到`index.js`，这是因为我们设置的`entry:'./index.js'`，但是在src中找不到`index.js`，如果我们在将`entry`修改成`'../index.js'`就不会报错了。

```js
// webpack.config.js
const path = require('path');

module.exports = {
  mode: 'none',
  entry: './index.js',
  context: path.resolve(__dirname,'src')
}
```

&ensp;&ensp;&ensp;&ensp;**entry类型**

| 类型   | 例子                               | 含义                                                         |
| ------ | ---------------------------------- | ------------------------------------------------------------ |
| string | './index.js'                       | 入口模块的文件路径[^Chunk名称是main.js]                      |
| array  | ['./index.js','./index2.js']       | 入口模块的文件路径，需要搭配`output.library`使用             |
| object | {a: './index.js', b: './index.js'} | 多个入口，每个入口生成一个Chunk[^打包出来的js文件,每个Chunk的名称是object的键值] |

&ensp;&ensp;&ensp;&ensp;**动态entry**

```js
// 同步函数
entry: () => {
  return {
    a:'./pages/a',
    b:'./pages/b',
  }
};
// 异步函数
entry: () => {
  return new Promise((resolve)=>{
    resolve({
       a:'./pages/a',
       b:'./pages/b',
    });
  });
};
```

&ensp;&ensp;&ensp;&ensp;你甚至可以使用node来遍历根目录的出webpack.config.js之外的所有js文件，并为每个js生成Chunk。

```js
// webpack.config.js
const path = require('path');
const fs = require('fs');

module.exports = {
  mode: 'none',
  entry: () => {
    return new Promise((resolve) => {
      let data = fs.readdirSync(__dirname)
      let obj = {};
      data.map((item)=>{
        const temp = path.parse(item);
        if(temp.ext === '.js' && item !== 'webpack.config.js'){
          obj[temp.name] = `./${item}`
        }
      })
      resolve(obj)
    })
  },
  context: path.resolve(__dirname)
}
```

#### Output

#### Module

&ensp;&ensp;&ensp;&ensp;`module.rules`是用来配置`loader`的，在webpack中，默认只能解析js和json文件，配置loader就是让webpack可以解析其他类型的文件。

&ensp;&ensp;&ensp;&ensp;**配置Loader**

&ensp;&ensp;&ensp;&ensp;webpack通过test、include、exclude三个配置项来命中loader匹配的文件。使用use来配置应用loader。

|         | 作用                                                         | 举例                                                         |
| ------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| test    | 使用正则表达式匹配文件                                       | ```test: /.\js$/```                                          |
| include | 需要处理的文件是include指定的文件<br />或include指定的目录中符合test的文件 | `test: /\.js$/`<br />`include: path.resolve(__dirname, 'src')` |
| exclude | exclude排除哪个文件，exclude和include同时存在时必须时include的子集，否则无效。 | `exclude: [path.resolve(__dirname, './node_modules/normalize.css')]`<br />`include: /node_modules/`<br />// 指定除了normalize.css之外的所有/node_modules/ |

&ensp;&ensp;&ensp;&ensp;我们安装一下loader并修改一下文件的结构，来写一下解析css的loader配置

```yaml
npm i css-loader style-loader
# css-loader 用来解析css
# style-loader 用来将解析的css插入html中
```

```yaml
|--dist
	|--main.js
|--node_modules
|--index.css
|--index.html
|--index.js
|--package-lock.json
|--package.json
|--wepack.config.js
```

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <script src="./dist//main.js"></script>
</body>
</html>
```

```css
/* index.css */
.div{
  color: red;
}
```

```js
// index.js
import './index.css'

const DIV = document.createElement('div');
DIV.className = 'div';
DIV.innerHTML = 'DIV';

document.body.appendChild(DIV);
```

```js
// webpack.config.js
const path = require('path');
const fs = require('fs');

module.exports = {
  mode: 'none',
  entry: './index.js',
  context: path.resolve(__dirname),
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader','css-loader']
      }
    ]
  }
}
```

&ensp;&ensp;&ensp;&ensp;再次运行`webpack`，然后打开index.html文件，会看到红色的DIV字体。

&ensp;&ensp;&ensp;&ensp;use可以是一个字符串也可以是一个数组，默认执行loader的顺序是从右到左，从下到上，我们可以通过设置来改变loader的顺序，可以参考我的这篇文章[webpack loader]([webpack loader | (1514.work)](https://1514.work/2023/04/15/webpack/loader/))。

&ensp;&ensp;&ensp;&ensp;**noParse**

&ensp;&ensp;&ensp;&ensp;noParse配置项可以让webpack忽略对部分文件的处理。noParse是可选的，类型可以是RegExp、[RegExp]、function中的一个。

```js
// 使用正则表达式
noParse: /jquery|chartjs/
noParse: (content)=> {
  // content 代表一个模块的文件路径
  // 返回 true or false
  return /jquery|chartjs/.test(content);
}
```

> 注意被忽略掉的文件里不应该包含 `import` 、 `require` 、 `define` 等模块化语句，不然会导致构建出的代码中包含无法在浏览器环境下执行的模块化语句。

&ensp;&ensp;&ensp;&ensp;**parser**

&ensp;&ensp;&ensp;&ensp;parser也可以配置哪些模块不解析，但是parser比noParse更加的细粒。parser可以精确到语法方面。

```JS
module.exports = {
  //...
  module: {
    rules: [
      {
        //...
        parser: {
          amd: false, // 禁用 AMD
          commonjs: false, // 禁用 CommonJS
          system: false, // 禁用 SystemJS
          harmony: false, // 禁用 ES2015 Harmony import/export
          requireInclude: false, // 禁用 require.include
          requireEnsure: false, // 禁用 require.ensure
          requireContext: false, // 禁用 require.context
          browserify: false, // 禁用特殊处理的 browserify bundle
          requireJs: false, // 禁用 requirejs.*
          node: false, // 禁用 __dirname, __filename, module, require.extensions, require.main, 等。
          node: {...}, // 在模块级别(module level)上重新配置 [node](/configuration/node) 层(layer)
          worker: ["default from web-worker", "..."] // 自定义 WebWorker 对 JavaScript 的处理，其中 "..." 为默认值。
        }
      }
    ]
  }
}
```

#### Plugins



#### Resolve

#### DevServer
