---
layout:     post
title:      IE 兼容性 
subtitle:   IE Compatibility IE6~IE10
date:       2017-02-04
author:     huangqing
header-img: img/post-bg-ie.jpg
catalog: true
categories: [browser]
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

## CSS hack

```CSS

 /* IE6：的样式... */
_selector{
    property:value;
}

/* IE7：的样式... */
+selector{
    property:value;
}

/* IE8：的样式... */
selector{
    property:value\0;
}

/* IE6 & IE7：的样式... */
*selector{
    property:value;
}

/* IE6 & IE7 & IE8：的样式... */
selector{property:value\9;}

/* IE9：的样式... */
@media all and (min-width:0) {
    .content .test{
        background: #f009;
        }
}

 /* IE10的样式... */
@media screen and (-ms-high-contrast: active), (-ms-high-contrast: none) { 
   /* IE10的样式... */
}
```


## Firefox hack 

```CSS

 /* 去除 input[type="number"] 类型的数字按钮 */
.hqgrid input[type="number"] {
    -moz-appearance: textfield;
}
```


## Chrome hack 

```CSS

/* Chrome浏览器中输入框以及其它表单控件获得焦点时的带颜色边框:设置表单控件的outline属性为none值 */
input {
    outline:none
} 

 /* 去除 input[type="number"] 类型的数字按钮 */
.hqgrid input::-webkit-outer-spin-button,
.hqgrid input::-webkit-inner-spin-button {
    -webkit-appearance: textfield;
}
```


## 指定IE文档版本

~~~html
<meta http-equiv="X-UA-Compatible" content="ie=edge">
<meta http-equiv="X-UA-Compatible" content="ie=9">
~~~


## 取消浏览器文本的选择

```javascript
var clearSlct = "getSelection" in window ?
    function () {
        window.getSelection().removeAllRanges();　　
    } :
    function () {
        document.selection.empty();　　
    };
```

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


## disable text selection while pressing 'shift'

Try a combo of JavaScript and css to prevent the selection in the first place:


```javascript
$('li').attr('unselectable', 'on'); // IE
```

css (for browsers not IE):

```css
li {
            user-select: none; /* CSS3 (little to no support) */
        -ms-user-select: none; /* IE 10+ */
       -moz-user-select: none; /* Gecko (Firefox) */
    -webkit-user-select: none; /* Webkit (Safari, Chrome) */
}
```



Try this after the Shift + click...

```javascript
document.getSelection().removeAllRanges();
```
If that is not effective enough, you might have to also cancel the onselectstart event...

```javascript
window.onload = function() {
  document.onselectstart = function() {
    return false;
  }
}
```