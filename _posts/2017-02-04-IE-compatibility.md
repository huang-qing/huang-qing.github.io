---
layout:     post
title:      IE 兼容性
subtitle:   IE Compatibility
date:       2017-02-04
author:     huangqing
header-img: img/post-bg-ie.jpg
catalog: true
categories: [IE]
tags:
    - IE
---



## HTML Poolyfills

[HTML5 Cross Browser Polyfills](https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-browser-Polyfills?utm_source=tuicool&utm_medium=referral)

## html 版本判断标签

~~~html
<!--[if !IE]><!--> 除IE外都可识别 <!--<![endif]-->
<!--[if IE]> 所有的IE可识别 <![endif]-->
<!--[if IE 6]> 仅IE6可识别 <![endif]-->
<!--[if lt IE 6]> IE6以及IE6以下版本可识别 <![endif]-->
<!--[if gte IE 6]> IE6以及IE6以上版本可识别 <![endif]-->
<!--[if IE 7]> 仅IE7可识别 <![endif]-->
<!--[if lt IE 7]> IE7以及IE7以下版本可识别 <![endif]-->
<!--[if gte IE 7]> IE7以及IE7以上版本可识别 <![endif]-->
<!--[if IE 8]> 仅IE8可识别 <![endif]-->
<!--[if IE 9]> 仅IE9可识别 <![endif]-->
<!--[if (gt IE 9)|!(IE)]><!--> 非IE浏览器或者IE版本大于IE9 <!--<![endif]-->
~~~

## 指定IE文档版本

~~~html
<meta http-equiv="X-UA-Compatible" content="ie=edge">
<meta http-equiv="X-UA-Compatible" content="ie=9">
~~~

## [html5shiv](https://github.com/aFarkas/html5shiv)

This script is the defacto way to enable use of HTML5 sectioning elements in legacy Internet Explorer.
~~~html
<!--[if lte IE 9]> 
    <script src="http://html5shiv.googlecode.com/svn/trunk/html5.js"></script>
<![endif]-->
~~~

## [Respond](https://github.com/scottjehl/Respond/)

A fast & lightweight polyfill for min/max-width CSS3 Media Queries (for IE 6-8, and more)

~~~html
<!--[if lt IE 9]>
    <script src="https://oss.maxcdn.com/html5shiv/3.7.2/html5shiv.min.js"></script>
    <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
<![endif]-->
~~~

## [es6-promise](https://github.com/stefanpenner/es6-promise)

Usage in IE<9