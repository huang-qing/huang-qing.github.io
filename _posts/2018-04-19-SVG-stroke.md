---
layout:     post
title:      SVG-stroke
subtitle:   
date:       2018-04-19
author:     huangqing
header-img: img/post-bg-svg.png
catalog: true
categories: [SVG]
tags:
    - SVG
---

# SVG STROKE

## stroke

定义一条线，文本或元素轮廓颜色

## stroke-width

定义一条线，文本或元素轮廓厚度

```html
<svg viewBox="0 0 300 10">
    <line x1="0" y1="0" x2="300" y2="0" 
    stroke="black" stroke-width="10" />
</svg>
```
上面就指定来这条直线边框的颜色是黑色以及它的宽度为10像素。

## stroke-linecap

描边端点表现形式

```html
<svg>
  <g fill='none' stroke='black' stroke-width='10'>
    <path stroke-linecap='butt' d='M5 20 l215 0' />
    <path stroke-linecap='round' d='M5 40 l215 0' stroke='red'/>
    <path stroke-linecap='square' d='M5 60 l215 0' />
  </g>
</svg>
```

![](/images/svg/stroke-linecap.jpg)

## stroke-linejoin

描边转角的表现方式

```
stroke-linejoin = miter
stroke-linejoin = round
stroke-linejoin = bevel
```

![](/images/svg/stroke-linejoin.jpg)


## stroke-opacity

描边透明度

## stroke-dasharray 虚线

在`SVG`中也可以像`CSS`中那样指定边框为虚线要用到属性`stroke-dasharray`。

`troke-dasharray`属性的参数，是一组用逗号分割的数字组成的序列。需要注意的是，这里的数字必须用逗号分割，虽然也可以插入空格，但是数字之间必须用逗号分开。

每一组数字，第一个用来表示实线，第二个用来表示空白。

```html
<svg viewBox="0 0 300 10">
    <line x1="0" y1="0" x2="300" y2="0" 
    stroke="black" stroke-width="10"  stroke-dasharray="5" />
</svg>
```

![](/images/svg/2393968288-5812dc72e4c35.png)

上面只有一个数字5，则表示会先画5px实线，紧接着是5px空白，然后又是5px实线，从而形成虚线。


如果你想要更复杂的虚线模式，你可以定义更多的数字，比如：

```html
<svg viewBox="0 0 300 10">
<defs>    
    <style>
        line#es {
            stroke: black;
            stroke-width: 5;
            stroke-dasharray: 5,20;
        }
    </style>
</defs>    
    <line id="es" x1="0" y1="0" x2="300" y2="0" />
</svg>
```

上面代码表示会先画5px实线，紧接着是20px空白，然后又是5px实线，从而形成虚线。

![](/images/svg/2509747845-5812dc72e023e.png)

如果是三个数字呢：

```html
<svg viewBox="0 0 300 10">
<defs>    
    <style>
        line#es {
            stroke: black;
            stroke-width: 5;
            stroke-dasharray: 5,20,5;
        }
    </style>
</defs>    
    <line id="es" x1="0" y1="0" x2="300" y2="0" />
</svg>
```

这种情况下，数字会循环两次，形成一个偶数的虚线模式。所以该路径首先是5px实线，然后是20px空白，然后是5px实线，接下来循环这组数字，形成5px空白、20px实线、5px空白。然后这种模式会继续循环。

![](/images/svg/2356996218-5812dc72e1376.png)

## storke-dashoffset

定义虚线开始的位置

```css
line#dashstart {
    stroke: black;
    stroke-width: 5;
    stroke-dasharray: 5, 20, 5;
    stroke-dashoffset: 25;
}
```

![](/images/svg/storke-dashoffset.jpg)

上面代码就定义里虚线是从25px像素的位置开始的。

## 让线动起来

利用`storke-dashoffset`和`stroke-dasharray`再配合`CSS3`的`animation`就可以让`SVG`线条产生像手画出来一样的动画效果。

当我们有一段长度为`300`的线条时，我们指定它的`storke-dashoffset`的值跟它的`stroke-dasharray`的值一样，这时候线条时从`300`像素的开始绘制，而我们整个的线条也只有`300`像素，所以这个时候线条时不可见的。而当我们指定`storke-dashoffset`的值从`300`慢慢的变化到`0`的时候，则线条也会从`0`的位置从新开始绘制，所以就像手画出来一样。配合CSS3就可以模拟实现手绘的动画效果。

```css
@keyframes strokeanim {
    to { stroke-dashoffset: 0; }
}
line#dashstart {
    stroke: hsl(260, 50%, 30%);
    stroke-width: 15;
    stroke-dasharray: 300;
    stroke-dashoffset: 300;
    animation: strokeanim 2s alternate infinite;
}
```

如下图所示：

![](/images/svg/4025459768-5812dc72e6135.gif)