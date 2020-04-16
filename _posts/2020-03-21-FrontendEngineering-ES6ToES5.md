---
layout: post
title: 将ES6编译为ES5
subtitle: 
date: 2020-03-21
author: huangqing
header-img: img/post-bg-javascript.gif
catalog: true
categories: [Frontend Engineering]
tags:
  - 编译
  - 前端工程化
---


## babel 7

babel的目的就是为了解决浏览器的自身对于es语言的差异性而带来的一款工具，有了babel就不用担心浏览器不支持es语言这件事。

[babel7](https://babel.docschina.org)的各个组成部分：

+ @babel/cli
+ @babel/core
+ @babel/preset-env
+ @babel/polyfill
+ @babel/runtime
+ @babel/plugin-transform-runtime
+ @babel/plugin-transform-xxx


#### @babel/cli

`@babel/cli` 是一个适合安装在本地项目里，而不是全局安装

`@babel/node` 跟`node cli`类似，不适用在产品中，意味着适合全局安装

注意一下这个`@`这个符号，这个是只有`babel7`才特有的

```Shell
npm install --save "@babel/cli" "@babel/core"  "@babel/preset-env" "@babel/polyfill" "@babel/plugin-transform-runtime"
```

#### @babel/plugin-transform-xxx

真正发挥作用执行转换需要使用插件[`@babel/plugin-transform-xxx`](https://babeljs.io/docs/en/plugins)

使用`.babelrc`配置文件：

```
{
  "plugins": [
    "@babel/plugin-transform-arrow-functions"
    "@babel/plugin-transform-block-scoping" 
  ]
}
```

#### @babel/preset-env

babel提供了一个叫做`preset`的概念，说好听点叫预设，直白点就是插件包的意思，意味着babel会预先替我们做好了一系列的插件包，例如下面这些babel认为程序员会用到的常用的插件包：

+ @babel/preset-env
+ @babel/preset-flow
+ @babel/preset-react
+ @babel/preset-typescript


`@babel/preset-env`

>Without any configuration options, babel-preset-env behaves exactly the same as babel-preset-latest (or babel-preset-es2015, babel-preset-es2016, and babel-preset-es2017 together).

```json
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "targets": {
          "node": "4",
          "chrome": "58",
          "ie": "11"
        }
      }
    ]
  ]
}
```

>babel 编译过程处理第一种情况 - 统一语法的形态，通常是高版本语法编译成低版本的，比如 ES6 语法编译成 ES5 或 ES3。而 babel-polyfill 处理第二种情况 - 让目标浏览器支持所有特性，不管它是全局的，还是原型的，或是其它。这样，通过 babel-polyfill，不同浏览器在特性支持上就站到同一起跑线。


#### [@babel/polyfill](https://babeljs.io/docs/en/babel-polyfill/)

As of Babel 7.4.0, this package has been deprecated in favor of directly including `core-js/stable` (to polyfill ECMAScript features) and `regenerator-runtime/runtime` (needed to use transpiled generator functions

>`polyfill`我们又称垫片，见名知意，所谓垫片也就是垫平不同浏览器或者不同环境下的差异，因为有的环境支持这个函数，有的环境不支持这种函数，解决的是有与没有的问题，这个是靠单纯的@babel/preset-env不能解决的，因为@babel/preset-env解决的是将高版本写法转化成低版本写法的问题，因为不同环境下低版本的写法有可能不同而已。

Babel默认只转换新的JavaScript句法（syntax），而不转换新的API，比如`Iterator`、`Generator`、`Set`、`Maps`、`Proxy`、`Reflect`、`Symbol`、`Promise`等全局对象，以及一些定义在全局对象上的方法（比如`Object.assign`）都不会转码。

```js
import "core-js/stable";
import "regenerator-runtime/runtime";
```

```Shell
npm install --save @babel/polyfill
```
>Because this is a polyfill (which will run before your source code), we need it to be a `dependency`, not a `devDependency`


Usage in Node / Browserify / Webpack

```js
require("@babel/polyfill");
```

ES6's `import` syntax

```js
import "@babel/polyfill";
```

Usage in Browser

Available from the `dist/polyfill.js` file within a `@babel/polyfill` npm release. This needs to be included **before** all your compiled Babel code. You can either prepend it to your compiled code or include it in a `<script>` before it.

[ECMAScript6 compatibility](http://kangax.github.io/compat-table/es6/)

>IE中引入`polyfill.js`文件解决API报错的问题

#### @babel/runtime

`@babel/runtime`的作用是提供统一的模块化的helper

```Shell
npm install --save "@babel/runtime" "@babel/plugin-transform-runtime"
```

```json
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "targets": {
          "node": "4"
        }
      }
    ]
  ],
  "plugins": [
    "@babel/plugin-transform-runtime"
  ]
}
```

#### 执行编译

package.json

```json
{
  "name": "babel",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "babel index.js -d lib"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "@babel/cli": "^7.1.5",
    "@babel/core": "^7.1.5",
    "@babel/plugin-transform-runtime": "^7.1.0",
    "@babel/polyfill": "^7.0.0",
    "@babel/preset-env": "^7.1.5"
  }
}

````

babel.config.js

```js
const presets = [
    [
      "@babel/env",
      {
        targets: {
        ie:"11",
        edge: "17",
        firefox: "60",
        chrome: "67",
        safari: "11.1",
        },
      },
    ],
  ];
  
  module.exports = { presets };
  ```

>`babel`是按照`CommonJS`的规范将 `import` 转换成`CommonJS`规范 形式的，但是浏览器本身没有 `module` `export` `require` `global` 这些变量，并没有原生的支持`CommonJS`规范 ，所以需要一些库来做这些支持。

## 打包编译浏览器环境

#### 使用 browserify

安装依赖包：

```Shell
cnpm install -g browserify
```
```Shell
cnpm install --save-dev babelify
```

执行编译
```
browserify src/index.js -t babelify --outfile dist-babelify/index.js
```


#### 在gulp中使用gulp-browserify

安装依赖包：

```Shell
cnpm install --save-dev gulp-browserify
```

gulpfile.js

```js

var gulp = require('gulp');
var browserify = require('gulp-browserify');
var rename = require('gulp-rename');
var babelify = require("babelify");

gulp.task('es5', async function () {
    //需要打包的文件入口
    gulp.src('src/dashboard.js', { read: false })
        .pipe(browserify({
            debug: true,
            transform: ["babelify"],
        }))
        .pipe(rename('dashboard.js'))
        .pipe(gulp.dest('./build/'))
})

```

#### 在gulp中使用browserify

```Shell
npm install --save-dev browserify vinyl-source-stream vinyl-buffer
```

```javascript
var browserify = require('browserify'),  // 插件，实际是node系
    // 转成stream流，gulp系
    stream = require('vinyl-source-stream'),
    // 转成二进制流，gulp系
    buffer = require('vinyl-buffer');
    
gulp.task('browserify', function () {
    // 定义入口文件
    return browserify({
        // 入口必须是转换过的es6文件，且文件不能是es6经过转换的es5文件，否者会报错
        entries: 'src/dashboard.js',
        debug: true
    })
        // 在bundle之前先转换es6，因为readabel stream 流没有transform方法
        //.transform("babelify", {presets: ['es2015']})
        .transform("babelify")
        // 转成stream流（stream流分小片段传输）
        .bundle()
        .on('error', function (error) {
            console.log(error.toString())
        })
        // node系只有content，添加名字转成gulp系可操作的流
        .pipe(stream('dashboard.js'))
        // 转成二进制的流（二进制方式整体传输）
        .pipe(buffer())
        // 输出
        .pipe(gulp.dest('build/'))
```


>`source-map`在谷歌浏览器上无效，可以重置浏览器的设置使其生效
