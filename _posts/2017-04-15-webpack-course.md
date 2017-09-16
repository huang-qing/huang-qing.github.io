---
layout:     post
title:      Webpack 简明教程
subtitle:   
date:       2017-04-15
author:     huangqing
header-img: img/post-bg-webpack.jpeg
catalog: true
categories: [web]
tags:
    - web
    - deploy
---


# Webpack 简明使用教程


## 使用webpak

+ [webpak官网教程](http://webpack.github.io/docs/tutorials/getting-started/)

+ [the front-end tooling book](http://tooling.github.io/book-of-modern-frontend-tooling/dependency-management/webpack/getting-started.html)

#### 通过 npm init 创建 package.json

```
npm init 
```

#### 安装全局webpake

```
$ npm install webpack -g
```


#### 保存至项目文件夹饼写入配置文件

```
$ npm install webpack --save-dev
```

#### 手动合并文件命令

```
$ webpack ./entry.js bundle.js
```

#### 加载css、style解析模块

```
$ cnpm install style-loader --save-dev
$ cnpm install css-loader --save-dev
```


## 安装运行 `webpack-dev-server`

浏览器打开 http://localhost:8080/ 或 http://localhost:8080/webpack-dev-server/

#### 安装

```
$ npm install webpack-dev-server -g
$ npm install webpack-dev-server --save-dev
```

#### 执行服务

```
$ webpack-dev-server --progress --colors
```

#### 执行服务:指定执行目录

```
$ webpack-dev-server --content-base dist/
```

## 简单的例子

### 文件目录结构

~~~html

根目录

|  –  dist  输出文件目录

       |  – images      图片文件目录

       |  – bundle.js   输出js文件

       |  – index.html  html文件       

|  –  src   源文件目录

       |  – css         样式文件目录
       
            |  – style.css              样式文件

       |  – js          js文件目录

            |  – echarts-bar.js         js文件

            |  – echarts-pie.js         js文件

       |  – app.js      webapck 文件加载配置

|  –  .gitignore   

|  –  package.json   

|  –  webpack.config.js     webapck配置文件

~~~

### webpack.config.js

~~~javascript
var webpack = require('webpack')

module.exports = {
    entry: './src/app.js',
    //context: __dirname + "/dist",
    output: {
        path: __dirname + '/dist',
        //path:"./dist",
        filename: 'bundle.js'
    },
    module: {
        loaders: [{
            test: /\.css$/,
            loader: 'style!css'
        }]
    },
    plugins: [
        new webpack.BannerPlugin('This file is create by huangqing!')
    ]
}
~~~

### app.js

~~~javascript

document.write("It works.")

//完整的写法
//require("!style!css!./css/style.css")

//通过配置后的简化写法
require("./css/style.css")
require("./js/echarts-bar.js")
require("./js/echarts-pie.js")

~~~



