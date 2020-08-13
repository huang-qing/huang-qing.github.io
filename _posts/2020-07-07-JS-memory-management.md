---
layout: post
title: JavaScript 中的内存泄漏
subtitle: RegExp
date: 2020-07-07
author: huangqing
header-img: img/post-bg-javascript.gif
catalog: true
categories: [JavaScript]
tags:
  - Regexp
---

## 什么是内存泄漏

内存泄漏是应用程序使用过的内存片段，在不再需要时，不能返回到操作系统或可用内存池中的情况。

## 四种常见的JavaScript内存泄漏

1：全局变量

JavaScript以一种有趣的方式来处理未声明的变量：当引用未声明的变量时，会在全局对象中创建一个新变量。

2：被遗忘的定时器或回调

3：闭包

JavaScript开发的一个关键方面是闭包。闭包是一个内部函数，可以访问外部（封闭）函数的变量

4：超出DOM引用

如果你在代码中保留对表格单元格（标签）的引用，并决定从DOM中删除该表格，还需要保留对该特定单元格的引用，则可能会出现严重的内存泄漏。你可能会认为垃圾收集器会释放除了那个单元之外的所有东西，但情况并非如此。由于单元格是表格的一个子节点，并且子节点保留着对父节点的引用，所以对表格单元格的这种引用，会将整个表格保存在内存中。