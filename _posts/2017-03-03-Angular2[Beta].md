---
layout:     post
title:      Angular2 (Beta) -> Angular4
subtitle:   
date:       2017-03-03
author:     huangqing
header-img: img/post-bg-angular.png
catalog: true
categories: [Angular]
tags:
    - Angular
---


# Angular2 (Beta) -> Angular4

+  开发语言：**ECMAScript6** 的标准已经完成。浏览器可以支持模块、类、*lambda* 表达式、*generator* 等新特性。

+  开发模式：**Web组件**将很快实现。

+  移动化：针对移动优化,如：缓存预编译、触控支持。

## ES6工具链

Angular2是面向未来的技术，浏览器需要支持ES6+，由于目前浏览器尚未实现ES6，需要使用垫片。

![ES6工具链](/images/angular/947566-0dea83e669291eff.png)

+ `angular2 polyfills` ： 为ES5浏览器提供ES6特性支持，比如Promise

+  `es6-module-loader` ： ES6模块加载器，systemjs会自动加载这个模块

+  `traceur` ： ES6转码器，将ES6代码转换为当前浏览器支持的ES5代码。systemjs会自动加载这个模块。如：TypeScript转码器。

+  `reactive extension` ： javascript版本的*反应式编程/Reactive Programming*实现库，被打包为systemjs的包格式，以便systemjs动态加载

+  `systemjs` ： 通用模块加载器，支持AMD、CommonJS、ES6等各种格式的JS模块加载

+  `angular2` ： Angular2框架，被打包为systemjs的包格式，以便systemjs动态加载模块


# Angular4

[Angular 中文文档](https://angular.cn/docs/ts/latest/)

[angular tutorial](https://angular.cn/docs/ts/latest/tutorial/toh-pt6.html)

[ANGULAR模块 (NGMODULE)](https://angular.cn/docs/ts/latest/guide/ngmodule.html)

[HTTP 客户端](https://angular.cn/docs/ts/latest/guide/server-communication.html)

