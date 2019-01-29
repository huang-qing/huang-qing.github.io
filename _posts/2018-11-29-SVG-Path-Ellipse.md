---
layout:     post
title:      SVG-Path Ellipse
subtitle:   绘制椭圆弧 
date:       2018-11-29
author:     huangqing
header-img: img/post-bg-svg.png
catalog: true
categories: [SVG]
tags:
    - SVG
---

#  SVG Ellipse 圆弧(A)指令

`SVG`中的路径数据，即`path`元素的 `d` 属性，有一系列的路径绘制指令，其中椭圆弧指令(A)最复杂，不算椭圆弧起始点的`x,y`坐标的话，依然有 `7` 个参数。

SVG椭圆弧指令的参数，与Canvas等圆弧指令的参数有很大差别，Canvas中使用圆心、半径、起始角度、结束角度等为参数，而**SVG使用起始点坐标、半径、方向、结束点坐标等为参数**。

SVG之所以实现为这样的参数形式，是因为SVG中的路径包含的每段子路径具有依次首尾相连的特性，这样每一段路径绘制的起点终点就比较明确直观。

SVG椭圆弧路径指令说明：


|指令	|A (绝对) a (相对)|
|----|----|
|**名称**	|elliptical arc 椭圆弧|
|**指令格式**	|`A(a) rx ry x-axis-rotation large-arc-flag sweep-flag x y`|
|**解释**|画一段到`(x,y)`的椭圆弧.<br/> 椭圆弧的 `x, y` 轴半径分别为 `rx,ry`.<br/> 椭圆相对于 `x 轴旋转 x-axis-rotation 度`.<br/>通过`large-arc`选取大/小弧线<br/>通过`sweep`选取顺/逆时针方向上的弧线|
|**参数解释**|	1. `rx` `ry` 是椭圆的两个半轴的长度。<br/> 2. `x-axis-rotation` 是椭圆相对于坐标系的旋转角度，`角度数`而非弧度数。<br/>3. `large-arc-flag` 是标记绘制`大弧(1)还是小弧(0)`部分：<br/> `large-arc=0`表明`弧线小于180度`<br/>  `large-arc=1`表示`弧线大于180度` <br/> 4. `sweep-flag` 是标记向`顺时针(1)还是逆时针(0)`方向绘制。<br/>`sweep=0`表明弧线`逆时针旋转`<br/>`sweep=1`表明弧线`顺时针旋转`.<br/> 5. `x` `y` 是圆弧终点的坐标。|
|**描述**	|从当前点绘制一段椭圆弧到点 `(x, y)`，<br/>椭圆的大小和方向由 `(rx, ry)` 和 `x-axis-rotation` 参数决定， <br/>`x-axis-rotation` 参数表示椭圆整体相对于当前坐标系统的旋转角度。<br/>椭圆的中心坐标 `(cx, cy)` 会自动进行计算从而满足其它参数约束。<br/>`large-arc-flag` 和 `sweep-flag `也被用于圆弧的计算与绘制。|

实例：

<svg xmlns="http://www.w3.org/2000/svg" width="600" height="300" viewbox="0 0 1200 600">
    <path d="M300,200 h-150 a150,150 0 1,0 150,-150 z"
        fill="red" stroke="blue" stroke-width="5" />
    <path d="M275,175 v-150 a150,150 0 0,0 -150,150 z"
        fill="yellow" stroke="blue" stroke-width="5" />

    <path d="M600,350 l 50,-25 
            a25,25 -30 0,1 50,-25 l 50,-25 
            a25,50 -30 0,1 50,-25 l 50,-25 
            a25,75 -30 0,1 50,-25 l 50,-25 
            a25,100 -30 0,1 50,-25 l 50,-25"
        fill="none" stroke="red" stroke-width="5"  />
</svg>


```html
<path d="M300,200 h-150 a150,150 0 1,0 150,-150 z"
    fill="red" stroke="blue" stroke-width="5" />
<path d="M275,175 v-150 a150,150 0 0,0 -150,150 z"
    fill="yellow" stroke="blue" stroke-width="5" />

<path d="M600,350 l 50,-25 
        a25,25 -30 0,1 50,-25 l 50,-25 
        a25,50 -30 0,1 50,-25 l 50,-25 
        a25,75 -30 0,1 50,-25 l 50,-25 
        a25,100 -30 0,1 50,-25 l 50,-25"
    fill="none" stroke="red" stroke-width="5"  />
```


在其它参数确定的情况下，`large-arc-flag` 和 `sweep-flag` 的不同组合，共有 `4` 种情况，分别对应 `4` 段不同的椭圆弧。


```html
<path d="M 125,75 a100,50 0 ?,? 100,50"
      style="fill:none; stroke:red; stroke-width:6"/>
```

![](/images/svg/4160144800-56acef9582989_articlex.png)

1. `large-arc-flag=0` 表示`小圆`, `sweep-flag=0` 表示`逆时针`.
2. `large-arc-flag=0` 表示`小圆`, `sweep-flag=1` 表示`顺时针`.
3. `large-arc-flag=1` 表示`大圆`, `sweep-flag=0` 表示`逆时针`.
4. `large-arc-flag=1` 表示`大圆`, `sweep-flag=1` 表示`顺时针`.

`x-axis-rotation` 又是什么意思呢? b, c, d, e 产生的前提是 椭圆的 x 轴与用户坐标系的 x 轴是平行的.
f 图表示椭圆 x 轴相对于用户坐标系的` x 轴旋转30度`所产生的椭圆弧. 灰色的部分表示原来产生的椭圆弧.