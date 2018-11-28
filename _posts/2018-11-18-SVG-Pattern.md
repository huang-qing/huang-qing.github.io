---
layout:     post
title:      SVG-Pattern
subtitle:   
date:       2018-11-18
author:     huangqing
header-img: img/post-bg-svg.png
catalog: true
categories: [SVG]
tags:
    - SVG
---

#  SVG Pattern

svg有两种填充方式：`pattern`和`gradient`。

`pattern`可以实现以指定的图案来填充一块区域。

`gradient`可以实现用渐变色来填充一块区域，渐变可以是线性（`linearGradient`）的，也可以是放射性（`radialGradient`）的。这块区域可以是`fill`，也可以是`stroke`。

## pattern

用`<pattern>`定义的图案，可以被用在其他元素的`fill`或者`stroke`属性中。

```html
<pattern id="pa">
    //描绘pattern图案的svg元素
</pattern>
<rect x="0" y="0" height="100" width="100" style="fill:url(#pa); stroke:url(#pa)">
```

## patternUnits

`pattern`有两种填充方式：`objectBoundingBox`和`userSpaceOnUse`

#### objectBoundingBox

一种可以理解为在指定区域内，规定沿`x`轴和`y`轴平铺指定数量的图案。这种方式对应的属性为`patternUnits="objectBoundingBox"`。`objectBoundingBox`也是`patternUnits`的默认属性值。

当`patternUnits="objectBoundingBox"`时，我们通过指定`width`和`height`来间接规定图案平铺的数量。因为这时，`width`和`height`被限制在`0~1`，或者`0%~100%`之间，即宽度或高度占填充区域高度或宽度的百分比。可想而知20%放5个，40%放2.5个。

```html
<defs>
    <pattern id="tile" width="0.2" height="0.2" patternUnits="objectBoundingBox">
        <path d="M 0 0 Q 5 20 10 10 T 20 20" stroke="black" fill="none"></path>
        <rect x="0" y="0" width="20" height="20" stroke="grey" fill="none"></rect>
    </pattern>
</defs>
<rect x="20" y="20" width="100" height="100" fill="url(#tile)" stroke="grey"></rect>
<rect x="140" y="20" width="70" height="80" fill="url(#tile)" stroke="grey"></rect>
<rect x="230" y="20" width="150" height="180" fill="url(#tile)" stroke="grey"></rect>
```

![](/images/svg/4069389078-590933ac85ab5_articlex.png)

`patternUnits`并不会对内部的图案做缩放，所以第二张图中当宽度70的20%不足画出20*20的贝塞尔曲线时，曲线超出部分就被裁掉了。而第三张图中当宽度150的20%超过20时，曲线也不会被放大，超出部分由空白来填充。

默认情况下，`pattern`图案会将填充区域的左上角作为起点坐标`(origin point)(0,0)`。当然也可以通过设置x和y属性的值改变这个起点坐标。x和y分别表示相对于起点坐标的偏移量。在`patternUnits="objectBoundingBox"`情况下，x和y的值乘以填充区域相应的宽度和高度，即为实际起点坐标偏移量。

```html
<defs>
    <pattern id="tile" x="0.1" y="0.1" width="0.2" height="0.2" patternUnits="objectBoundingBox">
        <path d="M 0 0 Q 5 20 10 10 T 20 20" stroke="black" fill="none"></path>
        <rect x="0" y="0" width="20" height="20" stroke="grey" fill="none"></rect>
    </pattern>
</defs>
<rect x="20" y="20" width="100" height="100" fill="url(#tile)" stroke="grey"></rect>
<rect x="140" y="20" width="70" height="80" fill="url(#tile)" stroke="grey"></rect>
<rect x="230" y="20" width="150" height="180" fill="url(#tile)" stroke="grey"></rect>
```

![](/images/svg/2106999598-590933d0e9381_articlex.png)


#### userSpaceOnUse

另一种是将`pattern`的宽度和高度固定住，在指定区域内平铺，能铺多少铺多少，超出部分裁掉。对应的属性为p`atternUnits="userSpaceOnUse"`。相应的`width`和`height`即`pattern`的宽度和高度。

<defs>
    <pattern id="tile2" x="0" y="0" width="20" height="20" patternUnits="userSpaceOnUse">
        <path d="M 0 0 Q 5 20 10 10 T 20 20" stroke="grey" fill="none"></path>
        <rect x="0" y="0" width="20" height="20" stroke="grey" fill="none"></rect>
    </pattern>
</defs>
<rect x="0" y="0" width="100" height="100" fill="url(#tile2)" stroke="black"></rect>
<rect x="100" y="0" width="70" height="80" fill="url(#tile2)" stroke="black"></rect>
<rect x="170" y="0" width="150" height="130" fill="url(#tile2)" stroke="black"></rect>

![](/images/svg/3536774748-590945925a1cf_articlex.png)

需要注意的是，`userSpaceOnUse`的`pattern`并不会在每个指定区域内重新以区域左上角开始排序。*`userSpaceOnUse`的`pattern`的起点坐标只有一个，就是其x和y表示的在svg画布坐标系中的位置*。
所以可以看到第二和第三个图相邻的两个图，贝塞尔曲线是连续的。我们可以改变pattern的x和y看下结果。

```html
<defs>
    <pattern id="tile2" x="0" y="0" width="20" height="20" patternUnits="userSpaceOnUse">
        <path d="M 0 0 Q 5 20 10 10 T 20 20" stroke="grey" fill="none"></path>
        <rect x="0" y="0" width="20" height="20" stroke="grey" fill="none"></rect>
    </pattern>
</defs>
<rect x="10" y="10" width="100" height="100" fill="url(#tile2)" stroke="black"></rect>
<rect x="110" y="20" width="70" height="80" fill="url(#tile2)" stroke="black"></rect>
<rect x="180" y="30" width="150" height="130" fill="url(#tile2)" stroke="black"></rect>
```

![](/images/svg/2364695114-590947a670ee4_articlex.png)

## patternContentUnits

`pattern`的缩放和排布由`patternUnits`控制，而`pattern`内部的图案的缩放和排布由`patternContentUnits`控制。

`patternContentUnits`也有两个属性：`objectBoundingBox`和`userSpaceOnUse`（默认属性值）

#### userSpaceOnUse

在`userSpaceOnUse`模式下，`pattern`内部元素的大小不会因为`pattern`的缩放而改变。`userSpaceOnUse`是`patternContentUnits`的默认属性值。

#### objectBoundingBox

在`objectBoundingBox`模式下，`pattern`内部元素所有属性的数值都会根据设置的比例，乘以`pattern`的`width`或`height`计算出实际长度。`pattern`内部元素所有属性的数值如果后面不带百分号%，都乘上100作为百分比数。所以像`stroke-width`默认值是1的这种属性，如果不指定一个数值，就会被当成100%来计算，结果就是撑满整个pattern。

```html
<svg  width="600px" height="200px">
    <defs>
        <pattern id="tile1" patternUnits="objectBoundingBox"  x="0" y="0" width=".2" height=".2" patternContentUnits="objectBoundingBox">
            <path d="M 0 0 Q .05 .2 .1 .1 T .2 .2" style="stroke: black; fill: none; stroke-width: .01;"></path>
            <path d="M 0 0 h .2 v .2 h-.2z" style="stroke: black; fill: none; stroke-width: .01;"></path>
        </pattern>
    </defs>
    <g transform="translate(20,20)">
        <rect x="0" y="0" width="100" height="100" style="fill: url(#tile1); stroke: black;"></rect>
    </g>
    <g transform="translate(135,20)">
        <rect x="0" y="0" width="70" height="80" style="fill: url(#tile1); stroke: black;"></rect>
    </g>
    <g transform="translate(220,20)">
        <rect x="0" y="0" width="150" height="80" style="fill: url(#tile1); stroke: black;"></rect>
    </g>
</svg>
```
<svg  width="600px" height="200px">
    <defs>
        <pattern id="tile1" patternUnits="objectBoundingBox"  x="0" y="0" width=".2" height=".2" patternContentUnits="objectBoundingBox">
            <path d="M 0 0 Q .05 .2 .1 .1 T .2 .2" style="stroke: black; fill: none; stroke-width: .01;"></path>
            <path d="M 0 0 h .2 v .2 h-.2z" style="stroke: black; fill: none; stroke-width: .01;"></path>
        </pattern>
    </defs>
    <g transform="translate(20,20)">
        <rect x="0" y="0" width="100" height="100" style="fill: url(#tile1); stroke: black;"></rect>
    </g>
    <g transform="translate(135,20)">
        <rect x="0" y="0" width="70" height="80" style="fill: url(#tile1); stroke: black;"></rect>
    </g>
    <g transform="translate(220,20)">
        <rect x="0" y="0" width="150" height="80" style="fill: url(#tile1); stroke: black;"></rect>
    </g>
</svg>


## viewBox

可以在`pattern`的属性中使用`viewBox`完全替代`patternContentUnits="objectBoundingBox"`，上例用viewBox的实现如下：

```html
<pattern id="tile2" patternUnits="objectBoundingBox"  x="0" y="0" width=".2" height=".2" viewBox="0 0 20 20" preserveAspectRatio="none">
    <path d="M 0 0 Q 5 20 10 10 T 20 20" style="stroke: black; fill: none; stroke-width: 1;"></path>
    <path d="M 0 0 h 20 v 20 h-20z" style="stroke: black; fill: none; stroke-width: 1;"></path>
</pattern>
```

当然，通过设置不同的`preserveAspectRatio`还可以实现一些`patternContentUnits="objectBoundingBox"`无法实现的功能。如果有`viewBox`属性，`patternContentUnits`属性将被忽略。

## pattern中内嵌pattern

允许在一个`pattern`中的元素内，嵌入另一个`pattern`。

```html
<defs>
    <pattern id="stripe" x="0" y="0" width=".2" height=".3333" patternContentUnits="objectBoundingBox">
        <path d="M0 0h1" stroke="grey" stroke-width=".02"></path>
        <path d="M0 0v1" stroke="grey" stroke-width=".02"></path>
    </pattern>
    <pattern id="circle" x="0" y="0" width=".2" height=".25" viewBox="0 0 40 40">
        <circle cx="20" cy="20" r="15" fill="url(#stripe)" stroke="black"></circle>
    </pattern>
</defs>
<path d="M0 0 h300v300h-300z" fill="url(#circle)" stroke="black"></path>
```

![](/images/svg/3780875450-590975514908a_articlex.png)


