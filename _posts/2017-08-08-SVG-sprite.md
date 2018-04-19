---
layout:     post
title:      SVG 雪碧图
subtitle:   svg sprite
date:       2017-08-08
author:     huangqing
header-img: img/post-bg-svg.png
catalog: true
categories: [SVG]
tags:
    - SVG
---

# svg sprite

## 概述

SVG（Scalable Vector Graphics）是一种矢量图格式。Adobe Illustrator是专门编辑、制作矢量图的软件。

随着高清视网膜屏幕的出现，Web设计需要根据屏幕输出不同分辨率的图片。在开发中需要准备两套不同的图片应对，一套在普通屏幕上显示；一套在高清屏幕上显示。

现在流行的icon font字体图标，其实质上是SVG的封装。

## SVG的优势

+  SVG是矢量图形文件，无限放大不失真。
+  可以用CSS样式来自由定义图标颜色，比如颜色/尺寸等效果。
+  所有的SVG可以全部在一个文件中，节省HTTP请求 。
+  使用SMIL、CSS或者是javascript可以制作充满灵性的交互动画和滤镜效果。
+  由于SVG也是一种XML节点的文件，所以可以使用gzip的方式把文件压缩到很小。

## Adobe Illustrator

Adobe Illustrator能直接把文件保存成SVG格式：

![1-1保存-选择SVG(svg)](/images/svg/947566-19212c25c0627c6a.png)


![1-2格式类型选择 SVG 1.1](/images/svg/947566-331a0642992c1fed.png)

## 浏览器支持
![浏览器支持统计](/images/svg/947566-24baafa5a4c2fadd.jpg)


在所有浏览器中支持，可以采用以下方式：[David Bushell](http://dbushell.com/2013/02/04/a-primer-to-front-end-svg-hacking/)


## 兼容处理

+ ### 使用 JavaScript if there’s an error：

~~~html
<img src="image.svg" onerror="this.onerror=null; this.src='image.png'">
~~~

+ ### 使用Modernizr检测,JQuery做降级处理：

~~~javascript
if (!Modernizr.svg) { 
    $('img[src$=".svg"]').each(function() { 
        $(this).attr('src', $(this).attr('src').replace('.svg', '.png'));
    });
}
~~~

+ ### 使用Modernizr检测,CSS降级处理：Modernizr在检测到不支持SVG时，会在HTML上加了`no-svg`CSS类

~~~css

.icon-logo
{ 
    background: url(logo.svg) no-repeat top left;
    background-size: contain;
}

.no-svg .main-header 
{ 
    background-image: url(logo.png);
}
~~~

+ ### 使用[SVGeezy](http://benhowdle.im/svgeezy/)插件



## SVG的使用

+  ### 使用`img`和`object`标签直接引用svg。这种方法的缺点主要在于要求每个图标都单独保存成一个SVG文件，使用时也是单独请求的，增加了HTTP请求。

最简单的用法：

~~~html
<img src="image.svg">
~~~

使用object标签：

~~~html
<object type="image/svg+xml" data="image.svg">
    <img src="fallback.png">
</object>
~~~

使用object标签，*Data URL* 方式

~~~html
<object type="image/svg+xml" data="data:image/svg+xml;base64,[data]"> 
    <img src="fallback.png">
</object>
~~~

`object`+`css`：不能加载svg时，会渲染内部 `div`的样式

~~~html

<object id="logo" type="image/svg+xml" data="logo.svg"> 
    <div>logo description</div>
</object>
~~~

~~~css
#logo div 
{ 
    width: 300px; 
    height: 50px; 
    background-image: url("logo.png");
}
~~~

+  ### Inline SVG。

直接把SVG写入 HTML 中，这种方法简单直接，而且具有非常好的可调性。Inline SVG 作为HTML文档的一部分，不需要单独请求。临时需要修改某个图标的形状也比较方便。但是Inline SVG使用上比较繁琐，需要在页面中插入一大块SVG代码不适合手写，图标复用起来也比较麻烦。

~~~html
<!--[if (gt IE 8)]><!--><svg></svg><!--<![endif]-->
~~~


+  ### SVG Sprite。

这里所说的Sprite技术，没错，类似于CSS中的Sprite技术。图标图形整合在一起，实际呈现的时候准确显示特定图标。其实基础的SVG Sprite也只是将原来的位图改成了SVG而已。

使用svg中的`<symbol>`元素来制作icon。SVG本身的定义是允许你可以使用<use>的方式直接引用SVG中的某一部分。

+ 首先使用`<symbol>`方式，将SVG文件组装起来。注意每个`<symbol>`都必须有唯一的id。

~~~html
        <svg xmlns="http://www.w3.org/2000/svg" style="display: none;"> 
            <symbol id="circle-cross" viewBox="0 0 32 32">       
                <title>circle-cross icon</title>
                <path d="M16 1.333q2.99 0 5.703 1.161t4.677 3.125 3.125 4.677 1.161 5.703-1.161 5.703-3.125 4.677-4.677 3.125-5.703 1.161-5.703-1.161-4.677-3.125-3.125-4.677-1.161-5.703 1.161-5.703 3.125-4.677 4.677-3.125 5.703-1.161zm0 2.667q-2.438 0-4.661.953t-3.828 2.557-2.557 3.828-.953 4.661.953 4.661 2.557 3.828 3.828 2.557 4.661.953 4.661-.953 3.828-2.557 2.557-3.828.953-4.661-.953-4.661-2.557-3.828-3.828-2.557-4.661-.953zm3.771 6.885q.552 0 .948.391t.396.943-.396.948l-2.833 2.833 2.833 2.823q.396.396.396.938 0 .552-.396.943t-.948.391-.938-.385l-2.833-2.823-2.823 2.823q-.385.385-.948.385-.552 0-.943-.385t-.391-.938q0-.563.385-.948l2.833-2.823-2.833-2.833q-.385-.385-.385-.938t.391-.948.943-.396.948.396l2.823 2.833 2.833-2.833q.396-.396.938-.396z"/> 
            </symbol> 
            <symbol id="circle-check" viewBox="0 0 32 32">
                <title>circle-check icon</title> 
                    <path d="M16 1.333q2.99 0 5.703 1.161t4.677 3.125 3.125 4.677 1.161 5.703-1.161 5.703-3.125 4.677-4.677 3.125-5.703 1.161-5.703-1.161-4.677-3.125-3.125-4.677-1.161-5.703 1.161-5.703 3.125-4.677 4.677-3.125 5.703-1.161zm0 2.667q-2.438 0-4.661.953t-3.828 2.557-2.557 3.828-.953 4.661.953 4.661 2.557 3.828 3.828 2.557 4.661.953 4.661-.953 3.828-2.557 2.557-3.828.953-4.661-.953-4.661-2.557-3.828-3.828-2.557-4.661-.953zm4.49 7.99q.552 0 .943.391t.391.943-.396.948l-5.656 5.656q-.385.385-.938.385-.563 0-.948-.385l-2.833-2.823q-.385-.385-.385-.948 0-.552.391-.943t.943-.391.948.396l1.885 1.885 4.708-4.719q.396-.396.948-.396z"/> 
            </symbol> 
            <!-- .... -->
        </svg>
~~~

1.  将SVG的XML文档插入到HTML中，直接用id引用icon：

~~~html
<svg class="icon">
    <use xlink:href="#circle-cross"></use>
</svg>
~~~

2.  使用外部链接文件的形式引用icon：

~~~html
<svg class="icon">
    <use xlink:href:"/asssets/svg-symbols.svg#circle-cross"></use>
</svg>
~~~

3.  支持fallback：

~~~html
<svg class="icon" viewBox="0 0 50 41"> 
    <switch> 
        <use xlink:href="#twitter-icon"></use>     
        <foreignObject> 
            <img class="icon" src="img/twitter-icon.png" alt="Twitter"> 
        </foreignObject> 
    </switch> 
</svg>
~~~

## gulp

自动合并生成工具[gulp-svg-symbols](https://github.com/Hiswe/gulp-svg-symbols)

## 参考文章

+  [W3C-SVG ProfilesSVG的规范说明](http://www.w3.org/TR/SVGMobile/)
+  [Gulp as a Development Web Server](http://code.tutsplus.com/tutorials/gulp-as-a-development-web-server--cms-20903)
+  [SVG symbol a Good Choice for Icons](http://css-tricks.com/svg-symbol-good-choice-icons/)
+  [Inline SVG vs Icon Fonts](http://css-tricks.com/icon-fonts-vs-svg/)
+  [replace-icon-fonts-with-svg](http://io-meter.com/2014/07/20/replace-icon-fonts-with-svg/)
+  [SVG浏览器支持统计图](http://caniuse.com/#feat=svg-img)
+  [https://css-tricks.com/svg-sprites-use-better-icon-fonts/](https://css-tricks.com/svg-sprites-use-better-icon-fonts/)