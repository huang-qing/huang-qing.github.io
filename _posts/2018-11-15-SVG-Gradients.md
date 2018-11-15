---
layout:     post
title:      SVG-Gradients
subtitle:   
date:       2018-11-15
author:     huangqing
header-img: img/post-bg-svg.png
catalog: true
categories: [SVG]
tags:
    - SVG
---

#  SVG渐变(SVG Gradients)

SVG渐变是填充SVG图形的一种方法。通过填充渐变色，可以使SVG图形的填充色或描边色由一种颜色过渡到另一种颜色。

## 线性渐变

线性渐变是指从一种颜色到另一种颜色的线性变化。

线性渐变的方向可以是水平方向或垂直方向，也可以是你指定的一个角度的方向。你也可以只为SVG图形的某一部分填充渐变色，而不是整个SVG图形。

下面是一些使用线性渐变填充SVG矩形的例子：

<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="450" height="120">
<defs>
  <linearGradient id="myLinearGradient1" x1="0%" y1="0%" x2="0%" y2="100%" spreadMethod="pad">
    <stop offset="0%" stop-color="#00cc00" stop-opacity="1"></stop>
    <stop offset="100%" stop-color="#006600" stop-opacity="1"></stop>
  </linearGradient>
    <linearGradient id="myLinearGradient2" x1="0%" y1="0%" x2="100%" y2="0%" spreadMethod="pad">
      <stop offset="0%" stop-color="#00cc00" stop-opacity="1"></stop>
      <stop offset="100%" stop-color="#006600" stop-opacity="1"></stop>
    </linearGradient>
    <linearGradient id="myLinearGradient3" x1="0%" y1="0%" x2="100%" y2="100%" spreadMethod="pad">
      <stop offset="0%" stop-color="#00cc00" stop-opacity="1"></stop>
      <stop offset="100%" stop-color="#006600" stop-opacity="1"></stop>
    </linearGradient>
    <linearGradient id="myLinearGradient4" x1="0%" y1="0%" x2="100%" y2="0%" spreadMethod="pad">
      <stop offset="50%" stop-color="#00cc00" stop-opacity="1"></stop>
      <stop offset="100%" stop-color="#006600" stop-opacity="1"></stop>
    </linearGradient>
  <linearGradient id="myLinearStrokeGradient" x1="0%" y1="0%" x2="0%" y2="100%" spreadMethod="pad">
    <stop offset="0%" stop-color="#009000" stop-opacity="1"></stop>
    <stop offset="100%" stop-color="#004000" stop-opacity="1"></stop>
  </linearGradient>
</defs>
<rect x="10" y="10" width="75" height="100" rx="10" ry="10" style="fill:url(#myLinearGradient1); stroke: #005000; stroke-width: 3;"></rect>
<rect x="100" y="10" width="75" height="100" rx="10" ry="10" style="fill:url(#myLinearGradient2); stroke: #005500; stroke-width: 3;"></rect>
<rect x="185" y="10" width="75" height="100" rx="10" ry="10" style="fill:url(#myLinearGradient3); stroke: #005500; stroke-width: 3;"></rect>
<rect x="270" y="10" width="150" height="100" rx="10" ry="10" style="fill:url(#myLinearGradient4); stroke: #005500; stroke-width: 3;"></rect>
</svg>


+ 第一个矩形使用的是垂直渐变
+ 第二个矩形使用的是水平渐变
+ 第三个矩形使用的是对角渐变（渐变色从左上角到右下角）
+ 第四个矩形仅仅在右侧使用渐变色来填充。

我们可以使用`<linearGradient>`元素来定义线性渐变。

```html
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink">
  <defs>
    <linearGradient id="myLinearGradient1"
                    x1="0%" y1="0%"
                    x2="0%" y2="100%"
                    spreadMethod="pad">
      <stop offset="0%"   stop-color="#00cc00" stop-opacity="1"/>
      <stop offset="100%" stop-color="#006600" stop-opacity="1"/>
    </linearGradient>
  </defs>
 
  <rect x="10" y="10" width="75" height="100" rx="10" ry="10"
     style="fill:url(#myLinearGradient1);
            stroke: #005000;
            stroke-width: 3;" />
</svg>  
```
                        
可以看到，`<linearGradient>`元素是嵌套在`<defs>`元素中的。渐变的定义必须嵌套在`<defs>`元素中，之后你可以在SVG图像中引用它们。在上面的例子中，线性渐变被`<rect>`元素引用，使用的方法是在`style`属性中使用`fill:url(#myLinearGradient1)`。
`<linearGradient>`元素中有两个嵌套的`<stop>`元素。

`<linearGradient>`元素控制渐变的方向，而`<stop>`元素控制渐变颜色的开始和结束位置，以及颜色的透明度。

下面列出了`<linearGradient>`元素的一些属性及其描述:

+ `id`	渐变的唯一ID号，用于在图形中引用该渐变
+ `x1`, `y1`	`x1`, `y1`定义渐变的起点。使用的是百分比数值
+ `x2`, `y2`	`x2`, `y2`定义渐变的终点。使用的是百分比数值
+ `spreadMethod`	这个参数定义渐变的传播方式。可取值有3个：`pad`，`repeat`和`reflect`。
    + `pad`是指开始和结束颜色平铺填充整个渐变。
    + `repeat`是指渐变在整个图形中不断重复。
    + `reflect`是指渐变在图形中会镜像显示。这个参数只有在渐变没有填充满整个图形时才有效果。（可以参看`<stop>`元素的offset属性）
+ `gradientTransform`	可以使用该参数在应用一个渐变之前对其进行转换（如旋转）
+ `gradientUnits`	设置计算 x1, y1 和 x2,y2的方式
+ `xlink:href`	设置这个渐变继承自另一个渐变，取值为另一个渐变的ID号。换句话说，如果这个渐变没有设置其它属性值，它将使用ID指向的那个渐变作为默认的渐变


下面列出了`<stop>`元素的一些属性和含义


+ `offset`	设置渐变的开始和结束颜色距离渐变两端的距离。使用渐变的百分比值来设置。例如，10%表示渐变进入图形内部10%的距离
+ `stop-color`	渐变停止点的颜色
+ `stop-opacity`	该渐变停止点的颜色透明度。

关于这些属性通过图像来说明比较清晰，来看下面的图像：

<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="600" height="160">
<defs>
  <linearGradient id="myLinearGradient1-2" x1="0%" y1="0%" x2="100%" y2="0%" spreadMethod="pad">
    <stop offset="10%" stop-color="#00cc00" stop-opacity="1"></stop>
    <stop offset="30%" stop-color="#006600" stop-opacity="1"></stop>
    <stop offset="70%" stop-color="#cc0000" stop-opacity="1"></stop>
    <stop offset="90%" stop-color="#000099" stop-opacity="1"></stop>
  </linearGradient>
</defs>
<rect x="10" y="10" width="500" height="50" rx="10" ry="10" style="fill:url(#myLinearGradient1-2); stroke: #005000; stroke-width: 3;"></rect>

<line x1="60" y1="0" x2="60" y2="75" style="stroke: #000000; stroke-width: 3;"></line>
<line x1="160" y1="0" x2="160" y2="75" style="stroke: #000000; stroke-width: 3;"></line>
<line x1="360" y1="0" x2="360" y2="75" style="stroke: #000000; stroke-width: 3;"></line>
<line x1="460" y1="0" x2="460" y2="75" style="stroke: #000000; stroke-width: 3;"></line>

<text x="30" y="95" style="stroke: #000000">offset 10%</text>
<text x="130" y="95" style="stroke: #000000">offset 30%</text>
<text x="330" y="95" style="stroke: #000000">offset 70%</text>
<text x="424" y="95" style="stroke: #000000">offset 90%</text>

<line x1="25" y1="40" x2="25" y2="130" style="stroke: #000000; stroke-width: 2;"></line>
<text x="20" y="150" style="/* stroke: #000000; */">Padded with first color</text>

<line x1="495" y1="40" x2="495" y2="130" style="stroke: #000000; stroke-width: 2;"></line>
<text x="360" y="150" style="stroke: #000000">Padded with last color</text>
</svg>

下面的代码是上图中渐变定义的代码：

```html
<svg xmlns="http://www.w3.org/2000/svg"
         xmlns:xlink="http://www.w3.org/1999/xlink">
<defs>
  <linearGradient id="myLinearGradient1"
                  x1="0%" y1="0%"
                  x2="100%" y2="0%"
                  spreadMethod="pad">
    <stop offset="10%" stop-color="#00cc00" stop-opacity="1"/>
    <stop offset="30%" stop-color="#006600" stop-opacity="1"/>
 
    <stop offset="70%" stop-color="#cc0000" stop-opacity="1"/>
    <stop offset="90%" stop-color="#000099" stop-opacity="1"/>
 
  </linearGradient>
 
</defs>
 
<rect x="10" y="10" width="500" height="50" rx="10" ry="10"
   style="fill:url(#myLinearGradient1); stroke: #005000;
          stroke-width: 3;" />
</svg>
```

第一个颜色停止点是`#00cc00`，它位于矩形边部`10%`的地方。因为`spreadMethod`属性被设置为`pad`，在矩形`0-10%`距离的地方仍然使用第一种颜色`#00cc00`来填充。

在第一个颜色停止点之后是第二个颜色停止点，它的颜色是`#006600`，位于矩形边部`30%`距离的地方。

第三个颜色停止点的颜色是`#cc0000`，位于矩形边部`70%`距离的地方。

第四个颜色停止点的颜色是`#000099`，位于矩形边部`90%`距离的地方。在这之后的矩形颜色使用第四个颜色停止点的颜色来填充，因为`spreadMethod`属性被设置为`pad`。

## 径向渐变

径向渐变是一种圆形的颜色渐变方式。下面是几个例子：

<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="512" height="120">
<defs>
  <radialGradient id="myRadialGradient1" fx="50%" fy="50%" r="10%" spreadMethod="reflect">
    <stop offset="0%" stop-color="red" stop-opacity="1"></stop>
    <stop offset="50%" stop-color="blue" stop-opacity="0.5"></stop>
    <stop offset="100%" stop-color="green" stop-opacity="0"></stop>
  </radialGradient>

    <radialGradient id="myRadialGradient2" fx="50%" fy="50%" r="65%" spreadMethod="pad">
      <stop offset="0%" stop-color="#00ee00" stop-opacity="1"></stop>
      <stop offset="100%" stop-color="#006600" stop-opacity="1"></stop>
    </radialGradient>

    <radialGradient id="myRadialGradient3" fx="50%" fy="0%" r="65%" spreadMethod="pad">
      <stop offset="0%" stop-color="#00ee00" stop-opacity="1"></stop>
      <stop offset="100%" stop-color="#006600" stop-opacity="1"></stop>
    </radialGradient>

    <radialGradient id="myRadialGradient4" fx="5%" fy="5%" r="65%" spreadMethod="pad">
      <stop offset="0%" stop-color="#00ee00" stop-opacity="1"></stop>
      <stop offset="100%" stop-color="#006600" stop-opacity="1"></stop>
    </radialGradient>
</defs>

<rect x="10" y="10" width="100" height="100" rx="10" ry="10" style="fill:url(#myRadialGradient1); stroke: #005000; stroke-width: 3;"></rect>

<rect x="120" y="10" width="100" height="100" rx="10" ry="10" style="fill:url(#myRadialGradient2); stroke: #005000; stroke-width: 3;"></rect>

<rect x="230" y="10" width="100" height="100" rx="10" ry="10" style="fill:url(#myRadialGradient3); stroke: #005000; stroke-width: 3;"></rect>

<rect x="340" y="10" width="100" height="100" rx="10" ry="10" style="fill:url(#myRadialGradient4); stroke: #005000; stroke-width: 3;"></rect>
</svg>

在上面的例子中,最后三个绿色的径向渐变分别将渐变的中心设置在不同的位置上，其它都相同，得到的效果却有所不同。第一个绿色径向将被的中心位于矩形的中心位置，第二个绿色径向渐变的中心位于矩形的上边部中心位置，第三个绿色径向渐变的中心位于军训的左上角位置。

我们可以使用`<radialGradient>`元素来定义颜色径向渐变。下面是一个例子：

```html
<svg xmlns="http://www.w3.org/2000/svg"
         xmlns:xlink="http://www.w3.org/1999/xlink">
    <defs>
        <radialGradient id="myRadialGradient4"
           fx="5%" fy="5%" r="65%"
           spreadMethod="pad">
          <stop offset="0%"   stop-color="#00ee00" stop-opacity="1"/>
          <stop offset="100%" stop-color="#006600" stop-opacity="1" />
        </radialGradient>
    </defs>
 
    <rect x="340" y="10" width="100" height="100" rx="10" ry="10"
           style="fill:url(#myRadialGradient4);
                  stroke: #005000; stroke-width: 3;" />
</svg>    
```

上面的代码实际上是前面例子中第四个绿色径向渐变的代码。在径向渐变中，颜色停止点`<stop>`的定义和线性渐变中的定义是一样的。

下面列出了`<radialGradient>`元素的一些属性:

+ `id`	用于在图形上引用该渐变的唯一标识符
+ `cx`,`cy`	径向渐变的中心点X和Y坐标。它的值使用用填充的百分比值。如果没有定义则默认值为50%
+ `fx`, `fy`	径向渐变的焦点X和Y值。它的值使用用填充的百分比值。如果没有定义则默认值为50%。注意：在Firefox 3.05中如果值低于5%可能会发生问题。
+ `r`	径向渐变的半径
+ `spreadMethod`	定义径向渐变的传播方式。可取值有3个：`pad`，`repeat`和`reflect`。
 + `pad`是指开始和结束颜色平铺填充整个渐变。
 + `repeat`是指渐变在整个图形中不断重复。
 + `reflect`是指渐变在图形中会镜像显示。这个参数只有在渐变没有填充满整个图形时才有效果。
+ `gradientTransform`	可以使用该参数在应用一个渐变之前对其进行转换（如旋转）
+ `gradientUnits`	设置计算 x1, y1 和 x2,y2的方式
+ `xlink:href`	设置这个渐变继承自另一个渐变，取值为另一个渐变的ID号。换句话说，如果这个渐变没有设置其它属性值，它将使用ID指向的那个渐变作为默认的渐变

径向渐变的聚焦点是颜色辐射的角度。你可以将径向渐变想象为一盏灯，那么聚焦点决定灯光从什么方向“照射”在图形上。50%,50%表示在图形的正上方，5%,5%表示在图形的左上角位置。

为了更好的理解径向渐变的中心点和聚焦点，你最好亲自动手分别设置一些它们不同的值，观察得到的不同效果。

## 渐变的转换

你可以使用标准的`SVG转换函数`来对渐变进行各种转换。可以在`<linearGradient>`和`<radialGradient>`元素中使用`gradientTransform`属性来进行渐变的转换。下面是一个例子：

```html
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink">
  <defs>
    <linearGradient id="myLinearGradient1"
                    x1="0%" y1="0%"
                    x2="0%" y2="100%"
                    spreadMethod="pad"
                    gradientTransform="rotate(45)"
    >
      <stop offset="0%"   stop-color="#00cc00" stop-opacity="1"/>
      <stop offset="100%" stop-color="#006600" stop-opacity="1"/>
    </linearGradient>
  </defs>
 
  <rect x="10" y="10" width="75" height="100" rx="10" ry="10"
     style="fill:url(#myLinearGradient1);
            stroke: #005000;
            stroke-width: 3;" />
</svg>  
```

这个例子定义了一个带`gradientTransform()`属性的线性渐变，`gradientTransform()`属性中将渐变旋转45度。正常情况下这个线性渐变是从上到下的渐变，但是使用了渐变转换之后，渐变变为从右上角到左下角的渐变。

下面是上面代码的返回结果：

<svg width="500" height="120">
    <defs>
        <linearGradient id="transformedGradient1" x1="0%" y1="0%" x2="0%" y2="100%" spreadMethod="pad" gradientTransform="rotate(45)">
            <stop offset="0%" stop-color="#00cc00" stop-opacity="1"></stop>
            <stop offset="100%" stop-color="#006600" stop-opacity="1"></stop>
        </linearGradient>
    </defs>

    <rect x="10" y="10" width="75" height="100" rx="10" ry="10" style="fill:url(#transformedGradient1);
    stroke: #005000;
    stroke-width: 3;"></rect>
</svg>