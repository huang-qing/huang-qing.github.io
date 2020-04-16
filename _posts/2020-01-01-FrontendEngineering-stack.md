---
layout: post
title: 前端工程技术栈（索引）
subtitle:
date: 2020-01-01
author: huangqing
header-img: img/post-bg-javascript.gif
catalog: true
categories: [Frontend Engineering]
tags:
  - 索引
---

+ 编程方式
  + 命令式编程
  + 面向对象编程
  + 函数式编程
  + 响应式编程
+ 语言
  + C#
  + JavaScript
    + ES5
    + ES6
  + TypeScript
  + HTML5
    + 拖放（drag 和 drop）
    + SVG
      + 贝塞尔曲线[cubic-bezier](https://cubic-bezier.com/#.17,.67,.83,.67) 
  + CSS3
    +  编译
       + Less
       + Sass
    +  CSS 媒体查询（Media Query）
    +  flex 弹性盒模型
  + WebGL
  + MarkDown
  + Canvas
  + Node.js
+ 编程工具
  + Visual Studio
  + Visual Studio Code
+ 版本控制
  + VSS
  + TFS
  + Git
+ 调试工具
  + Chrome
  + IE
  + Fiddler
  + VMware
  + Genymotion
  + ngrok
+ 开发框架
  + .NET
    + ASP.NET
    + ASP.NET MVC
  + APP
    + IONIC
    + Cordova
  + Web
    + Vue
    + BootStrap
    + jekyll:博客系统
  + 小程序
+ 工具库
  + 基础库
    + jQuery
    + jQueryUI
    + Babel:JavaScript 编译器
  + 函数库
    + RxJs
    + Moment.js
    + underscore
    + Lodash.js
  + SVG
    + Raphaël
    + Snap.svg
    + D3.js
  + 图表
    + ECharts.js
    + Charts.js
  + 3D
    + three.js
+ 前端工程化
  + 包管理工具
    + NPM
    + RubyGems
  + 自动化构建工具
    + babel ES编译CommonJS规范
    + browserify
    + webpack
    + gulp
    + grunt
  + 自动化测试【开发&测试】
  + 自动化打包【开发环境】
    + source map
    + 模块热替换(hot module replacement) HMR
    + 开发服务器(dev server),自动编译代码,实时重新加载(live reloading)
    + tree shaking
  + 自动化打包【发布环境】
    + tree shaking
    + 压缩文件
    + 分离切片（CSS、JS）
    + 编译代码ES6->ES5
    + 懒加载
    + 缓存、版本控制
    + 打包资源
      + 减小打包整体体积
        + 代码压缩
        + 移除不必要的模块`eslint`
        + 按需引入模块：`loadash`、`antd`
        + 选择可代替的较小模块：`moment.js` -> `day.js`
      + 按需加载（Code Splitting）
        + 首屏资源优化
        + 动态加载模块
        + 动态加载组件
        + 动态加载路由
      + 分包（Bundle Splitting）
  + MOCK数据
  + 网站部署
    + nginx
      + 解决跨域
      + 请求过滤
      + 配置gzip
      + 负载均衡
      + 静态资源服务器
