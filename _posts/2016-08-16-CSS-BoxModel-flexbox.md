---
layout:     post
title:      Flexbox 盒模型
subtitle:   CSS box-sizing Flexbox
date:       2016-08-16
author:     huangqing
header-img: img/post-bg-css.jpg
catalog: true
categories: [CSS]
tags:
    - CSS
---

# Flexbox 

[CSS](https://developer.mozilla.org/en-US/docs/Learn/CSS)

[Css3-flexbox](https://www.w3.org/html/ig/zh/wiki/Css3-flexbox/zh-hans)

[Flexbox——快速布局神器](http://www.w3cplus.com/css3/flexbox-basics.html)

[一个完整的Flexbox指南](http://www.w3cplus.com/css3/a-guide-to-flexbox.html)

[CSS3 Flexbox属性可视化指南](http://www.css88.com/archives/5744#more-5744)

[Autoprefixer](https://www.npmjs.com/package/autoprefixer)

## Flexbox属性图

![](/images/css/20180322154438.jpg)

从第一个子节点可以看到`Flexbox`由`Flex容器`和`Flex项目`组成，容器即父元素，项目即子元素。他们之间的一些关系可以这样来表示：

![](/images/css/20180322154615.jpg)


## Flex容器

#### `display:flex`

当我们使用`flexbox`布局时候，需要先给父容器的`display`值定位`flex`（块级）或者`inline-flex`（行内级）。

当使用了这个值以后，伸缩容器会为内容建立新的伸缩格式化上下文（FFC），它的上下文展示效果和BFC根元素相同（BFC特性：浮动不会闯入伸缩容器，且伸缩容器的边界不会与其内容边界叠加）。

`伸缩容器不是块容器`，因此有些设计用来控制块布局的属性，在伸缩布局中不适用，特别是多栏（column)，`float`，`clear`，`vertical-align`这些属性。

#### `flex-direction`

`[flex-direction]`属性用来控制上图中伸缩容器中主轴的方向，同时也决定了伸缩项目的方向。

+ `flex-direction:row`;也是默认值，即主轴的方向和正常的方向一样，从左到右排列。
+ `flex-direction:row-reverse`;和row的方向相反，从右到左排列。
+ `flex-direction:column`;从上到下排列。
+ `flex-direction:column-reverse`;从下到上排列。 以上只针对ltr书写方式，对于rtl正好相反了。

网页展示效果如下：

![](/images/css/20180322154941.jpg)

#### `flex-warp`

`[flex-wrap]`属性控制伸缩容器是单行还是多行，也决定了侧轴方向（新的一行的堆放方向）。

+ `flex-wrap:nowrap`;伸缩容器单行显示，默认值；
+ `flex-wrap:wrap`;伸缩容器多行显示；伸缩项目每一行的排列顺序由上到下依次。
+ `flex-wrap:wrap-reverse`;伸缩容器多行显示，但是伸缩项目每一行的排列顺序由下到上依次排列。

网页效果见图:

![](/images/css/20180322155110.jpg)

#### `flex-flow`

`[flex-flow]`属性为`flex-direction`（主轴方向）和`flex-wrap`（侧轴方向）的缩写，两个属性决定了伸缩容器的主轴与侧轴。

+ `flex-flow:[flex-direction][flex-wrap]`;默认值为`row nowrap`；

举两个栗子：

+ `flex-flow:row`;也是默认值;主轴是行内方向，单行显示，不换行；
+ `flex-flow:row-reverse wrap`;主轴和行内方向相反，从右到左，项目每一行由上到下排列（侧轴）。

网页效果如下：

![](/images/css/20180322155336.jpg)

#### `justify-content`

`[justify-content]`用于定义伸缩项目在`主轴上面的的对齐方式`，当一行上的所有伸缩项目都不能伸缩或可伸缩但是已经达到其最大长度时，这一属性才会对多余的空间进行分配。当项目溢出某一行时，这一属性也会在项目的对齐上施加一些控制。

+ `justify-content:flex-start`;伸缩项目向主轴的起始位置开始对齐，后面的每元素紧挨着前一个元素对齐。

+ `justify-content:flex-end`;伸缩项目向主轴的结束位置对齐，前面的每一个元素紧挨着后一个元素对齐。

+ `justify-content:center`;伸缩项目相互对齐并在主轴上面处于居中，并且第一个元素到主轴起点的距离等于最后一个元素到主轴终点的位置。以上3中都是“捆绑”在一个分别靠左、靠右、居中对齐。

+ `justify-content:space-between`;伸缩项目平均的分配在主轴上面，并且第一个元素和主轴的起点紧挨，最后一个元素和主轴上终点紧挨，中间剩下的伸缩项目在确保两两间隔相等的情况下进行平分。

+ `justify-content:space-around`;伸缩项目平均的分布在主轴上面，并且第一个元素到主轴起点距离和最后一个元素到主轴终点的距离相等，且等于中间元素两两的间距的一半。完美的平均分配，这个布局在阿里系中很常见。


![](/images/css/20180322155516.jpg)

![](/images/css/20180322155542.jpg)


#### `align-items`

`[align-items]`用来定义伸缩项目在`侧轴的对齐方式`，这类似于[justify-content]属性，但是是另一个方向。（flex-directon和flex-wrap是一对，justify-content和align-items是一对，前者分别定义主轴和侧轴的方向，后者分别定义主轴和侧轴中项目的对齐方式）。

+ `align-items:flex-start`;伸缩项目在侧轴起点边的外边距紧靠住该行在侧轴起点的边。
+ `align-items:flex-end`;伸缩项目在侧轴终点边的外边距靠住该行在侧轴终点的边。
+ `align-items:center`;伸缩项目的外边距在侧轴上居中放置。
+ `align-items:baseline`;如果伸缩项目的行内轴与侧轴为同一条，则该值与`[flex-start]`等效。 其它情况下，该值将参与基线对齐。
+ `align-items:stretch`;伸缩项目拉伸填充整个伸缩容器。此值会使项目的外边距盒的尺寸在遵照`「min/max-width/height」`属性的限制下尽可能接近所在行的尺寸。

下面demo只展示`center`和`stretch`的栗子，其他几个可以参考`flex-start`和`flex-end`那样。

![](/images/css/20180322155832.jpg)

#### `align-content`

`[align-content]`属性可以用来调准伸缩行在`伸缩容器里的对齐方式`，这与调准伸缩项目在主轴上对齐方式的`[justify-content]`属性类似。只不过这里`元素是以一行为单位`。请注意本属性在只有一行的伸缩容器上没有效果。`当使用flex-wrap:wrap时候多行效果就出来了`。

`align-content: flex-start || flex-end || center || space-between || space-around || stretch;`

+ `align-content: stretch`;默认值,各行将会伸展以占用剩余的空间。
+ 其他可以参考`[justify-content]`用法。

具体图片来至w3.org官方文档；

![](/images/css/20180322160044.jpg)

## Flex项目

主要是3个，`order`，`flex`（`flex-grow`，`flex-shrink`，`flex-basis`的组合），`align-self`；用来比较多的是前两个。

譬如我们想控制一个`container`中有4个box，想box4为一个显示，box1为最后一个显示。只需要 这样

```html
<style>
.container{
        display: flex;
    }
    .box1{
        order:1;
    }
    .box4{
        order:-1;
    }
</style>
<div class="container">
    <div class="box1">1</div>
    <div class="box2">2</div>
    <div class="box3">3</div>
    <div class="box4">4</div>
</div>
```

显示效果就这样了：

![](/images/css/20180322160237.jpg)

#### flex

`[flex]`属性可以用来指定可伸缩长度的部件，是`flex-grow`（扩展比例）,`flow-shrink`（收缩比例）,`flex-basis`（伸缩基准值）这个三个属性的缩写写法，建议大家采用缩写的方式而不是单独来使用这3个属性。

```css
flex:none | [ <'flex-grow'> ?<'flew-shrink'> || <'flow-basis'>]
// flex-grow是必须得flex-shrink和flow-basis是可选的
```

+ `flex-grow`:;其中number作为扩展比例，没有单位，初始值是`0`，主要用来决定伸缩容器剩余空间按比例应扩展多少空间。
+ `flex-grow`:;其中number作为收缩比例，没有单位，初始值是`1`，也就是剩余空间是负值的时候此伸缩项目相对于伸缩容器里其他伸缩项目能收缩的空间比例，在收缩的时候收缩比率会以`[flex-basis]`伸缩基准值加权。
+ `flex-basis`:|auto;默认是`auto`也就是根据可伸缩比率计算出剩余空间的分布之前，伸缩项目主轴长度的起始数值。若在「flex」缩写省略了此部件，则「flex-basis」的指定值是长度零。

flex-basis用图来表示就是这样：

![](/images/css/20180322160710.jpg)

#### align-self

[align-self]用来在单独的伸缩项目上覆写默认的对齐方式，这个属性是用来覆盖伸缩容器属性align-items对每一行的对齐方式。也就是说在默认的情况下这两个值是相等的。

```css
align-self: auto | flex-start | flex-end | center | baseline | stretch
```

## 兼容性

具体大家可以见这个网站：[caniuse](http://caniuse.com/#search=flexbox)

![](/images/css/20180322160829.jpg)

假如想`兼容多个浏览器`，可以采用`优雅降级`的方式来使用，这里推荐一个scss的`sass-flex-mixin`,这样就可以使用最新的写法，并且兼容大部分浏览器了。