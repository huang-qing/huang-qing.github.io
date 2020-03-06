---
layout: post
title: 优化打包资源
subtitle: 
date: 2020-03-02
author: huangqing
header-img: img/post-bg-javascript.gif
catalog: true
categories: [Frontend Engineering]
tags:
  - 文件打包
  - 前端工程化
---

## 原则

打包会有两方面的意思，第一在于提高打包的速度，第二在于对打包后的静态资源的优化。而对于静态资源的优化又不仅仅是打包提及的缩减。

对于打包资源优化的总体原则，在于尽可能的减少或者延迟模块的引用。主要遵循以下三点:

1. 减小打包的整体体积
2. `Code Splitting`: 按需加载，优化页面首次加载体积。如根据路由按需加载，根据是否可见按需加载
3. `Bundle Splitting`：分包，根据模块更改频率分层次打包，充分利用缓存

## 01 减小打包的整体体积

1. 代码压缩：最典型的两种方法就是空白符替换以及缩短变量名
2. 移除不必要的模块：`eslint`
3. 选择可替代的体积较小的模块：`moment.js` -> `day.js`
4. 按需引入模块:`lodash`、`antd`、`echarts`

## 02 Code Splitting:按需加载，优化页面首次加载体积

通过 `Code Splitting` 可以只加载当前所需要的核心资源：

1. 如果你处在首页，并且首页中有占用资源过重的图表，需要对图表懒加载，否则它会大幅拖垮应用的首次渲染，加大白屏时间
2. 如果你处在首页，你无需加载当前不可见屏幕下方的复杂组件
3. 如果你处在页面 A，你没有必要加载页面 B 的资源

1. 使用 `import()` 动态加载模块
2. 使用 `React.lazy()` 动态加载组件
3. 使用 `lodable-component` 动态加载路由，组件或者模块

## 03 Bundle Splitting

除了资源体积上的优化，另一个大的优化就是缓存。单页应用有一个最好的方面，就是所有资源都是带有指纹信息的，这意味着所有的资源都是能够设置永久缓存的。

分包，根据模块更改频率分层次打包，充分利用缓存。对资源进行分层次缓存的打包方案，这是一个建议方案.

1. `webpack-runtime`: 应用中的 `webpack` 的版本比较稳定，分离出来，保证长久的永久缓存
2. `react-runtime`: `react` 的版本更新频次也较低
3. `vundor`: 常用的第三方模块打包在一起，如 `lodash`，`classnames` 基本上每个页面都会引用到，但是它们的更新频率会更高一些


## 参考
[前端高级进阶：如何更好地优化打包资源](https://blog.csdn.net/qiwoo_weekly/article/details/104528900)
[山月](https://shanyue.tech)