---
layout:     post
title:      2018 现代前端开发路线图
subtitle:   
date:       2020-07-28
author:     huangqing
header-img: img/post-bg-technology-stack.jpg
catalog: true
categories: [Frontend Engineering]
tags:
    - roadmap
---

![](/images/front-end-engineering/2018-kamrana-frontend-developer-roadmap.jpg)


## CSS预处理器

+ Sass、Less、Stylus
+ PostCSS

### CSS组织

随着你的应用不断膨胀，CSS也开始变得混乱难以维系。有多种手段可以对你的CSS进行组织，让它更好地应对伸缩性，比如OOCSS、SMACSS、SUITCSS、Atomic以及`BEM`。你应该了解它们之间的不同，但是我更偏好BEM。

BEM代表块（Block），元素（Element），修饰符（Modifier）

![Block](/images/front-end-engineering/BEM-block.jpg)
![Element](/images/front-end-engineering/BEM-element.jpg)
![Modifier](/images/front-end-engineering/BEM-modifier.jpg)

[bem](https://en.bem.info/methodology/quick-start/)

[getbem](http://getbem.com/introduction/)

### 构建工具

webpack 、Rollup。

### 衡量和改进性能

Interactivity Time、Page Speed Index以及Lighthouse Score等。

### 渐进式Web应用

学习一下service worker以及如何制作渐进式web应用（PWA）。

### 测试

Jest、Mocha、 Karma及Enzyme

### 服务器渲染

+ React，最值得关注的选项是Next.js 和 After.js
+ Angular，你可以选Universal
+ Vue.js，我们有 Nuxt.js