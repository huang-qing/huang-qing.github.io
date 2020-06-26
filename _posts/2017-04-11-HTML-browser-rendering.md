---
layout:     post
title:      浅析前端页面渲染机制
subtitle:   render
date:       2017-04-11
author:     huangqing
header-img: img/post-bg-html.jpg
catalog: true
categories: [HTML]
tags:
    - HTML
---

## 浏览器

浏览器基础结构主要包括如下7部分：

1. 用户界面（User Interface）:用户所看到及与之交互的功能组件，如地址栏，返回，前进按钮等；
2. 浏览器引擎（Browser engine）:负责控制和管理下一级的渲染引擎；
3. 渲染引擎（Rendering engine）:负责解析用户请求的内容（如HTML或XML，渲染引擎会解析HTML或XML，以及相关CSS，然后返回解析后的内容）；
4. 网络（Networking）:负责处理网络相关的事务，如HTTP请求等；
5. UI后端（UI backend）:负责绘制提示框等浏览器组件，其底层使用的是操作系统的用户接口；
6. JavaScript解释器（JavaScript interpreter）:负责解析和执行JavaScript代码；
7. 数据存储（Data storage）:负责持久存储诸如cookie和缓存等应用数据。

浏览器内核：

+ Trident内核： IE
+ Webkit内核：Chrome,Safari
+ Gecko内核：FireFox

## 网络

+ DNS预解析（DNS prefetch）
+ 多进程：览器网络部分通常是有几个平行进程同时开启，但是也会有限制，一般为2-6个。

## 渲染引擎及关键渲染路径（Critical Rendering Path）

1. 构建DOM树(DOM tree)：从上到下解析HTML文档生成DOM节点树（DOM tree），也叫内容树（content tree）；
2. 构建CSSOM(CSS Object Model)树：加载解析样式生成CSSOM树；
3. 执行JavaScript：加载并执行JavaScript代码（包括内联代码或外联JavaScript文件）；
4. 构建渲染树(render tree)：根据DOM树和CSSOM树,生成渲染树(render tree)；渲染树：按顺序展示在屏幕上的一系列矩形，这些矩形带有字体，颜色和尺寸等视觉属性。
5. 布局（layout）：根据渲染树将节点树的每一个节点布局在屏幕上的正确位置；
6. 绘制（painting）：遍历渲染树绘制所有节点，为每一个节点适用对应的样式，这一过程是通过UI后端模块完成；

![critical-rendering-path](/images/html5/critical-rendering-path.png)

流程图

Webkit渲染引擎流程如下图：

![](/images/html5/webkit-render-flow.png)

Gecko渲染引擎流程如下图：

![](/images/html5/gecko-render-flow.jpg)

渲染引擎是单线程工作的，意味着渲染流程是一步一步渐进完成的.

1. 解析样式和脚本
2. 构建DOM树
3. 构建CSSOM树
4. 执行JavaScript
5. 构建渲染树(render tree)
6. 布局（Layout）或回流（reflow，relayout）
7. 绘制（painting）

![dom-tree](/images/html5/dom-tree.png)
![cssom-tree](/images/html5/cssom-tree.png)
![render-tree](/images/html5/render-tree.png)