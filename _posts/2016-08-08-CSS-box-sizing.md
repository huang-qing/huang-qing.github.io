---
layout:     post
title:      盒模型 box-sizing
subtitle:   CSS box-sizing
date:       2016-08-08
author:     huangqing
header-img: img/post-bg-css.jpg
catalog: true
categories: [CSS]
tags:
    - CSS
---

# box-sizing

`box-sizing`是CSS3的box属性之一。

`box-sizing ： content-box || border-box || inherit`

初始值	`content-box`

关键字:

```css
box-sizing: content-box;
box-sizing: border-box;
```

全局值:

```css
box-sizing: inherit;
box-sizing: initial;
box-sizing: unset;
```

## CSS的盒模型（Box model）

CSS中Box model是分为两种，第一种是W3C的标准模型，另一种是IE的传统模型.

### W3C的标准Box Model:

*外盒尺寸计算（元素空间尺寸）*:

Element Height = content height + padding + border + margin

Element Width = content width + padding + border + margin

*内盒尺寸计算（元素大小）*:

Element Height = content height + padding + border （Height为内容高度）

Element Width = content width + padding + border （Width为内容宽度）
 

### IE传统下Box Model（IE6以下，不含IE6版本或“QuirksMode下IE5.5+”）:

*外盒尺寸计算（元素空间尺寸）*:

Element Height = content Height + margin (Height包含了元素内容宽度，边框宽度，内距宽度)

Element Height = content Width + margin (Width包含了元素内容宽度、边框宽度、内距宽度)

*内盒尺寸计算（元素大小）*:

Element Height = content Height(Height包含了元素内容宽度，边框宽度，内距宽度)

Element Width = content Width(Width包含了元素内容宽度、边框宽度、内距宽度)


## content-box

content-box:此值为其默认值，其让元素维持W3C的标准Box Model(标准盒子模型)。

width 与 height *只包括内容的宽和高*， 不包括边框（border），内边距（padding），外边距（margin）。注意: 内边距, 边框 & 外边距 都在这个盒子的外部。 

元素的宽度/高度（width/height）=元素边框宽度（border）+ 元素内边距（padding）+元素内容宽度/高度（content width/height）

即：Element Width/Height = border+padding+content width/height。



比如:

如果 `.box {width: 350px};` 而且 `{border: 10px solid black;}` 那么在浏览器中的渲染的实际宽度将是`370px`;


## border-box

border-box:此值让元素维持IE传统的Box Model（IE6以下版本），或文档处于 Quirks模式 时Internet Explorer使用的盒模型。也就是说*元素的宽度/高度 等于 元素内容的宽度/高度 *。填充和边框将在盒子内。

width 和 height 属性包括内容，内边距和边框，但不包括外边距。

这里的content width/height包含了元素的border,padding内容的width/height

content width/height=width/height-border-padding

例如：

`.box {width: 350px; border: 10px solid black;}` 导致在浏览器中呈现的宽度为350px的盒子。

这里的维度计算为：

Element width = content width,

Element height = content height

即：Element Width/Height = width/height。
