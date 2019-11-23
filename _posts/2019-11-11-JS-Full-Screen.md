---
layout: post
title: 浏览器全屏显示
subtitle: IE、现代浏览器
date: 2019-11-11
author: huangqing
header-img: img/post-bg-javascript.jpg
catalog: true
categories: [JavaScript]
tags:
  - HTML
  - javascript
---

1. 现代浏览器包括IE11，可以直接用h5的`全屏api`实现 
2. 低版本的IE需要通过`ActiveX`插件实现

## 现代浏览器

```js
//全屏
function fullScreen() {
    var el = document.documentElement;
    var rfs = el.requestFullScreen || el.webkitRequestFullScreen || el.mozRequestFullScreen || el.msRequestFullscreen;
    if (typeof rfs != "undefined" && rfs) {
        //可以指定需要全屏显示的DOM元素
        rfs.call(el);
    };
    return;
}
//退出全屏
function exitScreen() {
    if (document.exitFullscreen) {
        document.exitFullscreen();
    }
    else if (document.mozCancelFullScreen) {
        document.mozCancelFullScreen();
    }
    else if (document.webkitCancelFullScreen) {
        document.webkitCancelFullScreen();
    }
    else if (document.msExitFullscreen) {
        document.msExitFullscreen();
    }
    if (typeof cfs != "undefined" && cfs) {
        cfs.call(el);
    }
}
```

## IE低版本

```js
function ieFullScreen() {
    var el = document.documentElement;
    var rfs = el.msRequestFullScreen;
    if (typeof window.ActiveXObject != "undefined") {
        //这的方法 模拟f11键，使浏览器全屏
        var wscript = new ActiveXObject("WScript.Shell");
        if (wscript != null) {
            wscript.SendKeys("{F11}");
        }
    }
}
//注：ie调用ActiveX控件，需要在ie浏览器安全设置里面把 ‘未标记为可安全执行脚本的ActiveX控件初始化并执行脚本’ 设置为启用
```
