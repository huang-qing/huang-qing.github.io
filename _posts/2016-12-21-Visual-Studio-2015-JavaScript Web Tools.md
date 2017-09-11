---
layout:     post
title:      Visual Studio 2015
subtitle:   Visual Studio 2015和ASP.NET 5前端开发工具集（Visual Studio 2015-JavaScript Web Tools ）
date:       2016-12-21
author:     huangqing
header-img: img/post-bg-vscode.jpg
catalog: true
categories: [visual-studio]
tags:
    - visual-studio
---




# Visual Studio 2015和ASP.NET 5前端开发工具集

最近微软发布了一本[白皮书-JavaScript Web Tools ](http://www.microsoft.com/en-us/download/details.aspx?id=46417)，谈到了一些可以和`Visual Studio 2015`和`ASP.NET 5`配合使用的JS/前端Web开发工具（比如：函数库、任务执行器、框架等）。

由于现在前端开发的生态系统在快速增长，也变得越来越复杂和庞大。所以，微软特意发布了这么一个白皮书来讲解一些可以集成到VS 2015用于ASP.NET 5开发的前端工具库。

这些前端工具库，都能很好的被VS2015所支持，比如提供智能提示等内置特性。

每个涉及的工具库都给出了入门介绍、基本概念，以及在VS和ASP.NET中的用法。这个白皮书完全就是一个非常难得的前端开发入门手册。

具体涉及到的工具库有：

## 流行的JS任务执行器：`Grunt`和`Gulp`

两者都可以自动对脚本进行压缩、对`TypeScript`编译、对代码质量进行分析、对CSS进行预处理等。

两者的区别在于，`Grunt`出现的较早，使用相对广泛；而`Gulp`出现较晚，但是相对轻量级性能也更好。`VS2015`默认使用`Grunt`，当然`Gulp`也可以很容易使用。

## 包管理器：`NPM`和`Bower`

虽然两者都是包管理器，不过`NPM`更多是安装开发环境的包，`Bower`是安装运行环境的前端包。

所以白皮书着重介绍的`Bower`。同时VS2015也直接通过`Bower`来加载前端库。

另外，对于`node.js`，前不久微软刚刚发布了`node.js Tools for Visual Studio`，可以让大家很方便的在VS中开发node.js应用。

## 自适应Web框架： `Bootstrap`

大名鼎鼎的Bootstrap我想就不用过多介绍了。之前要使用Bootstrap只能通过NuGet来安装，现在也可以使用Bower、npm来安装。

## 美化应用程序：  `Less`、`Sass`和`Font Awesome`

Less和Sass都CSS预处理工具库。而Font Awesome提供大量的矢量图标可以免费使用。

## 企业级JavaScript开发：`TypeScript`

此白皮书对TS给出了一个非常好的入门向导。同时讲到现在一些流行的js库（比如jQuery、angularjs、Boostrap、d3、requirejs、knockoutjs、node.js）都提供了TS的定义接口文件。

## MVVM函数库：`KnockoutJS`

一个很好支持Model-View-ViewModel模式的前端函数库。当然Knockout并非一个完整的SPA（单页应用）库，需要配以Durandal和Requirejs才能更好的开发大型js应用。

## MVC函数库：`Backbone`

顾名思义，一个可以让你以MVC模式来实现前端开发的函数库。不过，你可以只使用其中的一部分功能，这样方便迁移和入门。

## SPA框架：`AngularJS`

不仅介绍了1.x的入门和关键组件的使用。还简要介绍了Angular 2.0。

## 可重用的UI组件框架：`ReactJS`

ReactJS主要是用来构建可重用的UI组件的，可以和MVC或MVVM框架配合，来更方便的开发视图部分。

通过阅读这个白皮书的内容，基本可以了解如何在VS2015中使用这些流行前端工具库。强大的前端开发IDE。