---
layout:     post
title:      前后端开发架构分离
subtitle:   分离前后端开发，独立部署
date:       2017-08-16
author:     huangqing
header-img: img/post-bg-technology-stack.jpg
catalog: true
categories: [technology-stack]
tags:
    - 前端
    - framework
---

# 前后端分离

前后端分离：

架构上分离解耦，摆脱前后端在架构上的依赖。前后端各司其职，分开部署在不同的服务器上，通过RESTful接口传递数据。

### H5游戏：

H5游戏的开发采用HTML5的canvas等制作，使用webgl来做3D的H5游戏

### 移动APP：

1. React为语法基础的React Native

2. Vue为语法基础的Weex框架

### 桌面应用：

Nodejs和Chromium为基础的框架Electron

## 架构

前端应用部署在Nodejs、Nginx或者Nodejs和Nginx组合的服务器上，通过反向代理转发页面请求到后端服务器，相当于在传统的流程中加了Nodejs这一层。当然，也可以用Nodejs服务器来承担一部分负载均衡的工作，业务逻辑也可以放在Nodejs这一层来处理，例如：通过判断请求是来自PC还是APP，将请求发到不同的后端服务器。
 
 ![前后端交互](/images/front-end-framework/front-back-end.jpg)

## SPA

SPA是单页Web应用（single page web application，SPA）的简写

像Angular、React或Vue就是为了SPA而设计的，结合前端路由库（react-router、vue-router）和状态热存储（redux、vuex）等，可以开发出一个媲美Native APP的Web APP，用户体验得到了很大的提升。
当然，SPA也不是完美的，也不是适合所有的web应用，需要结合项目和场景来选择。

SPA有如下缺点：

+ 初次加载耗时增加。可以通过代码拆分、懒加载来提升性能，减少初次加载耗时。
+ SEO不友好，现在可以通过Prerender或Server render来解决一部分。
+ 页面的前进和后端需要开发者自己写，不过现在一些路由库已经帮助我们基本解决了。
+ 对开发者要求高，由于做SPA需要了解一整套技术栈，所以，要考虑后期是否有合适的人选进行维护。

## 语言知识

+ ES5 & ES6 & ES7      // ES语言基础
+ HTML5 API & CSS3        // HTML5和CSS特效
+ Less & Sass         // CSS预编译语言
+ SVG & Canvas & D3.js    // 图形数据可视化
+ WebGL & Three.js            // 3D场景
+ CMD & AMD & CommonJS        // 语言标准
+ RequireJS & SeaJS           // ES模块化库
+ CoffeeScript & TypeScript       // ES语言风格库
+ NodeJS & Express & Koa      // Node的WEB服务器
+ TCP & HTTP & WebSocket    // 网络协议
+ ……

## 框架、库

+ jQuery
+ Backbone
+ Ember
+ Angular & Angular2 & Angular4
+ React
+ Vue & Vue2
+ Ionic & Ionic2
+ React Native
+ Weex
+ Electron
+ ……

## 工具

+ Sublime Text & Atom & Webstorm & VS code     //编辑器、IDE
+ SVN & Git         //代码管理、版本控制
+ Chrome Dev Tools & FireFox Developer Edition // 浏览器开发者工具
+ ESLint & JSLint      // JavaScript代码语法检查
+ React DevTools      // react调试工具
+ Redux DevTools     // redux调试工具
+ Vue DevTools           // vue调试工具
+ Grunt & Gulp & browserify & Webpack // 代码打包工具
+ Babel    // ES6、react等语法转换工具，将代码转换成ES5
+ forever * pm2     // nodejs项目部署工具
+ karma & mocha & PhantomJS      //自动化测试框架
+ ……

