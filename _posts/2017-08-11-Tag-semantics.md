---
layout:     post
title:      标签语义化
subtitle:   
date:       2017-08-11
author:     huangqing
header-img: img/post-bg-html.jpg
catalog: true
categories: [HTML]
tags:
    - HTML
---

# 标签语义化

语义化的主要目的就是让大家直观的认识标签(markup)和属性(attribute)的用途和作用。优化搜索引擎，让爬虫能更好地理解网页结构。

## 为什么要使用语义化标签？

+  为了在没有css的情况下，页面也能呈现出良好的文档结构和代码结构
+  提高用户体验，例如title、alt可进行详细说明
+  有利于SEO，爬虫只看得懂代码
+  方便特殊设备的解析，如屏幕阅读器、盲人阅读器等
+  便于团队的开发和维护，语义化标签使代码更具可读性

## HTML标签

HTML标签可以分为有语义的标签，和无语义的标签。

+  `table`表示表格，`form`表示表单，`a`标签表示超链接，`strong`标签表强调。
+  无语义标签典型的有 `<div> <span>`等。

## HTML5中，语义化标签

`<header>、 <nav>、 <section>、 <article> 、<figure> 、<dialog>、 <aside> 、<footer>、 <time>、<mark>、<hgroup>`

![HTML5网页的典型结构](/images/html5/947566-69e8ae79c9e3cd0a.gif)

**`<header>`标签**

`<header>`标签是一个网页或者一个区域内的标题。定义`section`或`document`的页眉，包含一些内容介绍等信息。

**`<nav>`标签**

`<nav>`表示导航栏。

**`<section>`标签**

`<section>`元素定义文档中的节（区段）。通常有一个标题和页脚。

**`<article>`标签**

`<article>`用于表示内容与当前文档或网页关联不大的外部资讯，或任何其他内容的独立项目。如杂志、报纸或博客等的外部内容。

**`<figure>`标签**

` <figure>`表示一个自包含内容单元（含可选的标题），重点是即使将该内容移除也不会影响文档整体的含义。包含`<figcaption>`标签表示figure元素的标题。

**`<dialog>`标签**

`<dialog>`定义对话框或窗口。

**`<aside>`标签**

`<aside>`表示与正文内容关系不大的内容。如侧栏内容、注解或页脚等内容。

**`<footer>`标签**

`<footer>`用于定义 `section` 或 `document` 的页脚。页脚通常包含有关的区域如他写的，有关文件，链接资料的版权等

**`<time>`标签**

`<time>`表示内容日期/时间，或者通过`datetime`特性标识出内容关联的日期/时间。格式如下：
        
        YYYY-MM-DDThh:mm:ssTZD
        具体为：
        YYYY - 年 (例如 2011)
        MM - 月 (例如 01 表示 January)
        DD - 天 (例如 08)
        T - 必需的分隔符，若规定时间的话
        hh - 时 (例如 22 表示 10.00pm)
        mm - 分 (例如 55)
        ss - 秒 (例如 03)
        TZD - 时区标识符 (Z 表示祖鲁，也称为格林威治时间)

**`<mark>`标签**

 `<mark>`突出与特定主题（上下文）关联的内容

**`<hgroup>`标签**

`<hgroup>`对h1-h6标签进行分组。内含一个或多个h1-h6标签。例如：文章主标题和副标题。

**`<progress>`标签**

`<progress>`表示某项任务的执行进度，通过max特性设置任务完成时的值，通过value特性设置任务当前的执行进度。样式效果为进度条。

**`<output>`标签**

`<output>`定义内容为计算结果，可在form元素提交时向服务端发送其内容。for特性用于设置与计算结果相关的表单元素id，多个id时使用空格分隔。

**`<details>`标签**

`<details>`用于描述文档或文档某个部分的细节。默认不显示详细信息，通过open特性可修改为显示详细信息。通过点击标题可实现展开/收缩详细信息的效果。结合`<summary>`元素可自定义标题的内容。

**`<ruby>`标签**

`<ruby>`元素：显示的是东亚字符的发音。需要结合`<rt>`元素和可选的`<rp>`元素使用。

~~~html
<ruby>漢 <rt>ㄏㄢˋ</rt><rp>(ㄏㄢˋ)</rp></ruby>
~~~

<ruby>漢 <rt>ㄏㄢˋ</rt><rp>(ㄏㄢˋ)</rp></ruby>

**`<main>`标签**

`<main>`表示文档的主要内容，一个文档仅能出现一个<main>元素，并且不能作为以下元素的后代：`<article>、<aside>、<footer>、<header> 、<nav>`。

## IE9旧版本浏览器支持

IE9以下旧版本浏览器不支持新的语义化标签：

### 解决方案1:使用js将HTML5增加的标签创建出来：

~~~javascript

  var html5Tags = ['header' ,'footer','article','nav' ,'section','aside'];
  for( var i = 0;i < html5Tags.length ; i++ ){
    document.createElement(html5Tags[i]);
  }
~~~

### 解决方案2:使用[html5shiv][]，将以下声明放在head部分：
[html5shiv]:https://github.com/aFarkas/html5shiv

~~~html
<!--[if lte IE 9]> 
    <script src="http://html5shiv.googlecode.com/svn/trunk/html5.js"></script>
<![endif]-->
~~~

## 参考资料
[w3cschool-html5-tags](http://www.w3school.com.cn/tags/)
[w3c-html5](https://www.w3.org/TR/html5/semantics.html#semantics)