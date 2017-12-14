---
layout:     post
title:      纯CSS绘制三角形（各种角度）
subtitle:   
date:       2017-12-09
author:     huangqing
header-img: img/post-bg-css.jpg
catalog: true
categories: [CSS]
tags:
    - CSS
---

## 上三角

<p/>
<div id="triangle-up"></div>
<style>
#triangle-up {
    width: 0;
    height: 0;
    border-left: 50px solid transparent;
    border-right: 50px solid transparent;
    border-bottom: 100px solid #D94E37;
}
</style>
<p/>

```CSS
#triangle-up {
    width: 0;
    height: 0;
    border-left: 50px solid transparent;
    border-right: 50px solid transparent;
    border-bottom: 100px solid #D94E37;
}
```

## 下三角

<p/>
<div id="triangle-down"></div>
<style>
#triangle-down {
    width: 0;
    height: 0;
    border-left: 50px solid transparent;
    border-right: 50px solid transparent;
    border-top: 100px solid #D94E37;
}
</style>
<p/>

```CSS
#triangle-down {
    width: 0;
    height: 0;
    border-left: 50px solid transparent;
    border-right: 50px solid transparent;
    border-top: 100px solid #D94E37;
}
```

## 左三角

<p/>
<div id="triangle-left"></div>
<style>
#triangle-left {
    width: 0;
    height: 0;
    border-top: 50px solid transparent;
    border-right: 100px solid #D94E37;
    border-bottom: 50px solid transparent;
}
</style>
<p/>

```CSS
#triangle-left {
    width: 0;
    height: 0;
    border-top: 50px solid transparent;
    border-right: 100px solid #D94E37;
    border-bottom: 50px solid transparent;
}
```


## 右三角

<p/>
<div id="triangle-right"></div>
<style>
#triangle-right {
    width: 0;
    height: 0;
    border-top: 50px solid transparent;
    border-left: 100px solid #D94E37;
    border-bottom: 50px solid transparent;
}
</style>
<p/>

```CSS
#triangle-right {
    width: 0;
    height: 0;
    border-top: 50px solid transparent;
    border-left: 100px solid #D94E37;
    border-bottom: 50px solid transparent;
}
```


## 左上三角

<p/>
<div id="triangle-topleft"></div>
<style>
#triangle-topleft {
    width: 0;
    height: 0;
    border-top: 100px solid #D94E37;
    border-right: 100px solid transparent;
}
</style>
<p/>

```CSS
#triangle-topleft {
    width: 0;
    height: 0;
    border-top: 100px solid #D94E37;
    border-right: 100px solid transparent;
}
```

## 右上三角

<p/>
<div id="triangle-topright"></div>
<style>
#triangle-topright {
    width: 0;
    height: 0;
    border-top: 100px solid #D94E37;
    border-left: 100px solid transparent; 
}
</style>
<p/>

```CSS
#triangle-topright {
    width: 0;
    height: 0;
    border-top: 100px solid #D94E37;
    border-left: 100px solid transparent; 
}
```

## 左下三角

<p/>
<div id="triangle-bottomleft"></div>
<style>
#triangle-bottomleft {
    width: 0;
    height: 0;
    border-bottom: 100px solid #D94E37;
    border-right: 100px solid transparent;
}
</style>
<p/>

```CSS
#triangle-bottomleft {
    width: 0;
    height: 0;
    border-bottom: 100px solid #D94E37;
    border-right: 100px solid transparent;
}
```

## 右下三角

<p/>
<div id="triangle-bottomright"></div>
<style>
#triangle-bottomright {
    width: 0;
    height: 0;
    border-bottom: 100px solid #D94E37;
    border-left: 100px solid transparent;
}
</style>
<p/>

```CSS
#triangle-bottomright {
    width: 0;
    height: 0;
    border-bottom: 100px solid #D94E37;
    border-left: 100px solid transparent;
}
```


