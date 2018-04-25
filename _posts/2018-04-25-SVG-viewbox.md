---
layout:     post
title:      SVG-viewbox
subtitle:   
date:       2018-04-19
author:     huangqing
header-img: img/post-bg-svg.png
catalog: true
categories: [SVG]
tags:
    - SVG
---

# SVG VIEWBOX

## viewport

首先来了解下`SVG`中的`viewport`这个概念，简单来说`viewbox`就是值指`SVG`图片本身可视区域的大小，除了`SVG`本身，其它一些元素也有可视区域的限制，比如`symbol`、`image`、`pattern`等。

在`SVG`中`viewport`主要是通过`width`和`height`属性来定义的。比如，我们定义一个width="600"和height="600"就表示我们定义了一个600X600的可视区域，这与html和css中还有一点不同，SVG本身定义这些属性并没有单位，不过基本上都是以像素px为单位的。

## viewbox

`viewbox`确实是`SVG`中比较难懂抽象的一个属性，第一次接触它确实不知道它干嘛用的，就算理解了在使用中，有可能还是不知道如何去设置它。

`viewbox`简单的理解就是用来定义`SVG`的`可视范围`，那跟上面提到的`viewbox`有什么关系呢？这样来说吧，当没有设置`viewbox`的时候，`viewbox`就是整个`viewport`的大小，而当我们设定了`viewbox`，等于就是告诉`SVG`，我指定的这个区域是我要显示的，`SVG`就会把这个区域放大到`viewport`的大小，比如电视机，电视机就是那么大（viewport），而电视机里的画面，可以特写也可以全景，这就是viewbox。

![](/images/svg/440842781-57fdd9d1c1ba6.jpg)


## preserveAspectRatio

`preserveAspectRatio`和`viewbox`是一个绝配的搭档，特别是当`viewbox`的值和`viewport`的值（也就是宽和高）不一样的时候，`preserveAspectRatio`属性就直接决定浏览器怎么来显示图片。


#### <align> [<meetOrSlice>]

`align`:

| 值 |	含义|
| ---- | ---- |
|xMin	|viewport和viewBox左边对齐|
|xMid	|viewport和viewBox x轴中心对齐|
|xMax	|viewport和viewBox右边对齐|
|YMin	|viewport和viewBox上边缘对齐。注意Y是大写。|
|YMid	|viewport和viewBox y轴中心点对齐。注意Y是大写。|
|YMax	|viewport和viewBox下边缘对齐。注意Y是大写。|

`meetOrSlice`:

| 值 |	含义 |
| ---- | ---- |
|meet	|保持纵横比缩放viewBox适应viewport|
|slice	|保持纵横比同时比例小的方向放大填满viewport|
|none	|扭曲纵横比以充分适应viewport|




#### 实例

首先来看看`viewbox`的值和`viewport`的值一样的表现，代码如下所示：

```html
<div class="contain-demo">
  <svg width="115" height="190" viewBox="0 0 115 190">
    <desc>Green pear to show effects of matching viewport and viewBox.</desc>
    <g>
      <path fill="#BBC42A" d="M4.976,126.451c-0.571,1.76-1.067,3.551-1.475,5.377c-6.62,29.681,7.465,54.363,43.244,56.718 c56.994,3.751,77.653-25.65,60.462-67.25C90.017,79.697,82.063,89.688,80.366,57.764c-0.764-14.367-11.098-27.167-26.203-24.576
      c-17.378,2.982-19.794,19.916-22.192,34.44C28.36,89.508,11.623,105.957,4.976,126.451z"/>
      <path fill="none" stroke="#59351C" stroke-width="7" stroke-linecap="round" d="M56.427,40.406
     c0,0-7.375-15.376,8.06-35.837"/>
      <path fill="#7AA20D" stroke="#7AA20D" stroke-width="3" stroke-linecap="round" stroke-linejoin="round" d="
     M54.247,35.412c0.787-3.843,3.55-7.335,8.368-9.848c10.053-5.244,18.075-4.042,28.061,0.2c0.004,0.002,12.83,5.449,20.517-4.672
      c-4.778,6.291-9.415,12.478-14.878,18.237c-8.878,9.359-25.348,22.181-37.176,9.661C55.019,44.629,53.349,39.793,54.247,35.412z"/>
    </g>
  </svg>
</div>
```

从上面代码可以看出我们把`viewbox`的值设置和`viewport`的宽和高一样，运行结果如下图所示：

```html
<svg width="115" height="190" viewBox="0 0 115 190">
```

![](/images/svg/826621263-57fdd9d1a8b15.jpg)

减小`viewbox`:

如果我们把`viewbox`的值各减少50px，我们就定义了`SVG`要显示的区域，结果就是`SVG`会把整个图像的左部区域拉大填满整个画布显示，如下图所示：

```html
<svg width="115px" height="190px" viewBox="0 0 65 140">
```

![](/images/svg/4178945155-57fdd9d17f29d.jpg)

增加`viewport`:

上面由于我们减少了`viewbox`，由于`SVG`的可视区域也就是`viewport`不足以容纳显示整个内容，所以才会出现上图所示的只显示了图片的部分内容。

下面我们把`SVG`的可视区域也就是`viewport`的宽高各增加200px，`viewbox`保持`115`和`190`不变，则`SVG`内容会直接铺满整个可视区域，如下图所示：

```html
 <svg width="315" height="390" viewBox="0 0 115 190">
```

![](/images/svg/3727517752-57fdd9d18c859.jpg)

