---
layout:     post
title:      SVG-Marker
subtitle:   
date:       2018-11-15
author:     huangqing
header-img: img/post-bg-svg.png
catalog: true
categories: [SVG]
tags:
    - SVG
---

#  SVG Marker

实际应用中，可能需要在线段的开始或者结尾添加一个箭头或者其他类型标记。

SVG提供了`<markers>`元素可以很好的实现上述功能，可以标记一条线或路径开始、中间或结束位置。

`marker`是一种可以连结一个或多个`path`、`line`、`polyline`、或`polygon`的顶点的标志类型。最常见的用例是绘制箭头或在输出结果的线上的标记一个（polymarker）图形。

使用`<marker>`元素创建一个marker，以及其相关属性。通常我们把marker放在`<defs>`元素中，然后在其它地方对其进行引用。


<svg width="600px" height="100px"> 
  <defs> 
    <marker viewBox="0 0 6 6" markerWidth="3" markerHeight="3" orient="auto" refX="6" refY="3" id="arrow">
      <path d="M0,0 L0,6 L9,3 z" fill="#1e1e1e" style=""></path>
    </marker>
  </defs> 
  <line x1="50" y1="50" x2="250" y2="50" stroke="#1e1e1e" stroke-width="2" marker-end="url(#arrow)" />
</svg>

```html
<svg width="600px" height="100px"> 
  <defs> 
    <marker viewBox="0 0 6 6" markerWidth="3" markerHeight="3" orient="auto" refX="6" refY="3" id="arrow">
      <path d="M0,0 L0,6 L9,3 z" fill="#1e1e1e" style=""></path>
    </marker>
  </defs> 
  <line x1="50" y1="50" x2="250" y2="50" stroke="#1e1e1e" stroke-width="2" marker-end="url(#arrow)" />
</svg>
```

## marker 属性

（1）`id`属性:

其他元素会利用`<markers>`元素的`id`属性进行引用。

```css
marker-end="url(#arrow)"
```

`<line>`元素的`marker-end`属性值为`"url(#arrow)"`，`"arrow"`是`<markers>`元素的`id`属性值。

（2）`markerWidth`和`markerHeight`属性：

`<markers>`元素创建一个`viewport`视窗，上述两个属性用来规定视窗的尺寸。

`markerWidth`和`markerHeight`属性定义了`marker`视窗的宽度和高度。`marker`在自己的视窗中展示，所以这个尺寸必须至少大于`marker`标签内定义的图形,内部的图形大于这个的尺寸将会导致图形被裁剪。

`viewBox`的值设为(0 0 6 6)，是当前`marker`视窗（宽度为3，高度为3）两倍的大小，所以它会把三角形变成原来的一半大（注意，是一半！因为视窗变大啦）

（3）`refX`和`refY`属性：

规定`<markers>`元素内创建的图形元素哪个位置与指定图形元素相连接。`refX="0"` `refY="3"`规定三角形的`（0，3）`位置与直线的末端相连接。所属坐标系是`<markers>`元素创建视窗内的当前用户坐标系。

（4）`orient`属性:

`marker`的特殊之处就在于它们可以根据应用的对象指向的方向来调整方向。这个属性就是为什么在转换`line`的方向时，不需要调整`marker`的原因。

它接受一个`auto`值，或者一个角度值，这个值决定了`marker`在与其它内容连接的时候是否要旋转。

`auto`这个值表示`marker`会随着应用的元素一起旋转。`45deg`这个值则表示`marker`的方向一直保持`45deg`，不会随着连接的元素一起旋转。


（5）`markerUnits`属性：

用来规定`<marker>`以及其中绘制的图形的尺寸是以什么为单位的。属性值可以是`"strokeWidth"（默认）`或者`"userSpaceOnUse"`。

（6）`viewBox`属性：

`<markers>`元素会创建一个`viewport`视窗，所以它也有`viewBox`。

## 图形元素引用`<markers>`：

图形元素可以使用以下三个属性来引用`<markers>`元素：

1. `marker-start`：表示连接路径的开始位置。
2. `marker-mid`：表示连接路径的中间位置(路径的连接之处)。
3. `marker-end`：表示连接路径的结束位置。

属性值都是如下格式：

```css
marker-start : url(#markerId);
```



