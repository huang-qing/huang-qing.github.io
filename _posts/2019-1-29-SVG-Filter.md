---
layout:     post
title:      SVG-Filter 
subtitle:   Inset Drop-shadow
date:       2019-01-29
author:     huangqing
header-img: img/post-bg-svg.png
catalog: true
categories: [SVG]
tags:
    - SVG
---

# Filter

[SVG教程](https://cloud.tencent.com/developer/doc/1231)

## SVG过滤器的输入和输出

SVG过滤器在应用过滤效果的时候需要一个输入源。这个输入源可以是一个图形，或图形的alpha通道，或另一个过滤器的输出值。

SVG过滤器可以从输入源中产生一个输出图像。一个过滤器的输出可以是另一个过滤器的输入，这样，过滤器可以被链接起来使用。

下面是一张SVG过滤器输入和输出的说明图片：

![SVG过滤器的输入和输出](/images/svg/filters-1.png)

SVG过滤器的输入通常在SVG滤镜的`in`属性中指定，例如：
```HTML
<feGaussianBlur stdDeviation="3" in="SourceGraphic" /> 
```                             
如果你需要将一个SVG过滤器的输出作为另一个过滤器的输入，需要在输出元素上添加一个`result`属性：
```HTML
<feGaussianBlur stdDeviation="3" in="SourceGraphic" result="blur"/>     
```    
这样，在另一个过滤器中，可以通过在`in`属性中设置值为`blur`来使用它作为输入源。

## 过滤器的尺寸

一个SVG过滤器的尺寸由`x`、`y`、`width`和`height`属性来决定。

`x`和`y`属性是相对于输入源图形的x和y属性来设定。由于过滤器的输出图形通常会比输入图形大（例如对图形添加模糊效果），因此，我们通常需要将x和y属性设置为负值来剪切掉多出的部分。

`width`和`height`属性指定过滤器的宽度和高度，大多数时候你需要指定宽度和高度大于输出图像的尺寸，以便于在剪切后尺寸和原来的图形基本相等。



# Filter 类型标签

`<filter>`标签用来定义滤镜，滤镜必须含有id属性来标识滤镜。图形元素通过`filter=url(#id)`来引用滤镜。

+ `feBlend`:类似于CSS blend modes，描述了图像通过混合模式进行交互
+ `feComponentTransfer`: 改变个人对RGBA通道的总称（如feFuncG）
+ `feComposite`:一个原始的过滤器，定义像素图像交互方式
+ `feConvolveMatrix` ：这个过滤器规定像素与他近邻将关闭交互（如：模糊、锐化）
+ `feDiffuseLighting` ：定义了一个光源
+ `feDisplacementMap` : 使用另一个输入值(in2)取代一个图像的像素值(in)
+ `feFlood` : 完成过滤器的填充区域指定的颜色和alpha等级
+ `feGaussianBlur` :输入的模糊值和标准值的偏差
+ `feImage` ：使用其他的过滤器(像feBlend或feComposite)
+ `feMerge` ： 允许异步过滤效果应用，而不是分层
+ `feMorphology` : 削弱或扩张源图像
+ `feOffset` ：用来创建阴影
+ `feSpecularLighting` : 通过alpha创建凹凸贴图，又将其称之为"镜面"(Phong Reflection Model)
+ `feTile` : 指图像如何重复填补空间
+ `feTurbulence` : 允许创建纹理


## feMerge(多重过滤器)

可以通过`<feMerge>`元素来同时使用多个SVG过滤器。看下面的例子：

```HTML
<defs>
    <filter id="blurFilter2" y="-10" height="40" x="-10" width="150">
        <feOffset in="SourceAlpha" dx="3" dy="3" result="offset2" />
        <feGaussianBlur in="offset2" stdDeviation="3"  result="blur2"/>
 
        <feMerge>
            <feMergeNode in="blur2" />
            <feMergeNode in="SourceGraphic" />
        </feMerge>
    </filter>
</defs>
 
<ellipse cx="55" cy="60" rx="25" ry="15"
         style="stroke: none; fill: #0000ff; filter: url(#blurFilter2);" />  
```

这个例子中创建了一个SVG过滤器，它包括两个滤镜元素：`<feOffset>`和`<feGaussianBlur>`。偏移滤镜的输入源是椭圆图形的alpha通道，高斯模糊滤镜的输入源是偏移滤镜的输出。

`<feMerge>`元素将原始图像和高斯模糊滤镜的输出相结合。在<`feMerge>`元素中的结合顺序决定了它们的显示顺序，后输入的元素会显示在先输入元素的上面。


## feblend（混合）

[MDN feBlend](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/feBlend)

SVG滤镜原语`<feBlend>`通过一个确定的混合模式将两个对象组合到一起。它有点像我们平常使用的图片编辑器中的合并两个图层。混合模式由`mode`属性定义。

`in`、`in2` 标识为给定的滤镜原始输入：`SourceGraphic` /` SourceAlpha` / `BackgroundImage` / `BackgroundAlpha` / `FillPaint` / `StrokePaint` / `<filter-primitive-reference>`*（注意：in在上层，in2在下层）*

`mode` 混合模式： `normal` / `multiply` / `screen` / `darken` / `lighten`


```html
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
	<filter id="feBlend1">
		<feFlood result="floodFill" x="0" y="0" width="100%" height="100%" flood-color="green" flood-opacity="0.9"/>
		<feBlend in="SourceGraphic" in2="floodFill" mode="normal"/>
	</filter>
	<filter id="feBlend2">
		<feFlood result="floodFill" x="0" y="0" width="100%" height="100%" flood-color="green" flood-opacity="0.9"/>
		<feBlend in="floodFill" in2="SourceGraphic" mode="normal"/>
	</filter>
	<circle cx="50" cy="50" r="40" fill="red" filter="url(#feBlend1)" />
	<circle cx="150" cy="50" r="40" fill="red" filter="url(#feBlend2)"/>
</svg>
```

<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
	<filter id="feBlend1">
		<feFlood result="floodFill" x="0" y="0" width="100%" height="100%" flood-color="green" flood-opacity="0.9"/>
		<feBlend in="SourceGraphic" in2="floodFill" mode="normal"/>
	</filter>
	<filter id="feBlend2">
		<feFlood result="floodFill" x="0" y="0" width="100%" height="100%" flood-color="green" flood-opacity="0.9"/>
		<feBlend in="floodFill" in2="SourceGraphic" mode="normal"/>
	</filter>
	<circle cx="50" cy="50" r="40" fill="red" filter="url(#feBlend1)" />
	<circle cx="150" cy="50" r="40" fill="red" filter="url(#feBlend2)"/>
</svg>


## feFlood（探照灯）

[参考MDN](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/feFlood)

SVG滤镜原语`<feFlood>`使用定义好的颜色`flood-color`和透明度`flood-opacity`填充了滤镜的分区。

+ `（x，y）`、 `width` 、 `height` 定义了绘制矩形的范围
+ `flood-color`（探照灯 或 泛滥）颜色
+ `flood-opacity` （探照灯 或 泛滥）透明度

```html
<svg xmlns="http://www.w3.org/2000/svg" width="200" height="200" >
	<filter id="floodFilter" filterUnits="userSpaceOnUse">
		<feFlood x="10" y="10" width="100" height="100" flood-color="green" flood-opacity="0.5"/>
	</filter>
	<use filter="url(#floodFilter)"/>
	<circle cx="10" cy="10" r="3" fill="red" />
	<circle cx="110" cy="110" r="3" fill="red" />
</svg>
```

<svg xmlns="http://www.w3.org/2000/svg" width="200" height="200" >
	<filter id="floodFilter" filterUnits="userSpaceOnUse">
		<feFlood x="10" y="10" width="100" height="100" flood-color="green" flood-opacity="0.5"/>
	</filter>
	<use filter="url(#floodFilter)"/>
	<circle cx="10" cy="10" r="3" fill="red" />
	<circle cx="110" cy="110" r="3" fill="red" />
</svg>


## feColorMatrix（RGBA颜色矩阵）

[参考-1](http://www.w3cplus.com/svg/finessing-fecolormatrix.html) / 
[参考-2](https://blog.csdn.net/chy555chy/article/details/53364310)

```html
<svg xmlns="http://www.w3.org/2000/svg" >
	<filter id="linear">
	    <feColorMatrix
	      type="matrix"
	      values="R 0 0 0 0
	              0 G 0 0 0
	              0 0 B 0 0
	              0 0 0 A 0 "/>
	    </feColorMatrix>
	 </filter>
</svg>
```

矩阵计算RGBA自已每行的最终值，每个RGBA通道有自身的RGBA通道。最后一个值是一个乘数。最后RGBA的值从上向下读，像下面这个列表：

```
/* R G B A 1 */
1 0 0 0 0 // R = 1*R + 0*G + 0*B + 0*A + 0
0 1 0 0 0 // G = 0*R + 1*G + 0*B + 0*A + 0
0 0 1 0 0 // B = 0*R + 0*G + 1*B + 0*A + 0
0 0 0 1 0 // A = 0*R + 0*G + 0*B + 1*A + 0
```

## feGaussianBlur（高斯模糊）

SVG高斯模糊滤镜可以将图像进行模糊处理。要使用高斯模糊滤镜，可以使用`<feGaussianBlur>`元素。`<feGaussianBlur>`元素的`stdDeviation`属性决定图像的模糊尺寸大小。它的数值越大，图像的模糊尺寸越大。

+ `in` 标识为给定的滤镜原始输入：`SourceGraphic` / `SourceAlpha` / `BackgroundImage` / `BackgroundAlpha` / `FillPaint` / `StrokePaint` / `<filter-primitive-reference>`

+ `stdDeviation` 模糊量

```html
  <filter id="SourceGraphic">
    <feGaussianBlur in="SourceGraphic" stdDeviation="10" />
  </filter>
```


<svg xmlns="http://www.w3.org/2000/svg" width="1000" height="300" >
  <filter id="SourceGraphic">
    <feGaussianBlur in="SourceGraphic" stdDeviation="10" />
  </filter>
  <filter id="SourceAlpha">
    <feGaussianBlur in="SourceAlpha" stdDeviation="10" />
  </filter>
  <filter id="BackgroundImage">
    <feGaussianBlur in="BackgroundImage" stdDeviation="10" />
  </filter>
  <filter id="BackgroundAlpha">
    <feGaussianBlur in="BackgroundAlpha" stdDeviation="10" />
  </filter>
  <filter id="FillPaint">
    <feGaussianBlur in="FillPaint" stdDeviation="10" />
  </filter>
  <filter id="StrokePaint">
    <feGaussianBlur in="StrokePaint" stdDeviation="10" />
  </filter>

  <circle cx="60"  cy="60" r="40" fill="green" />
  <circle cx="160" cy="60" r="40" fill="green" filter="url(#SourceGraphic)" />
  <circle cx="260" cy="60" r="40" fill="green" filter="url(#SourceAlpha)" />
  <circle cx="360" cy="60" r="40" fill="green" filter="url(#BackgroundImage)" />
  <circle cx="460" cy="60" r="40" fill="green" filter="url(#BackgroundAlpha)" />
  <circle cx="560" cy="60" r="40" fill="green" filter="url(#FillPaint)" />
  <circle cx="660" cy="60" r="40" fill="green" filter="url(#StrokePaint)" />
	
  <filter id="SourceGraphic2">
    <feGaussianBlur in="SourceGraphic" stdDeviation="2" />
  </filter>
  <filter id="SourceGraphic5">
    <feGaussianBlur in="SourceGraphic" stdDeviation="5" />
  </filter>
  <filter id="SourceGraphic10">
    <feGaussianBlur in="SourceGraphic" stdDeviation="10" />
  </filter>	
  <filter id="SourceGraphic20">
    <feGaussianBlur in="SourceGraphic" stdDeviation="20" />
  </filter>
  <filter id="SourceGraphic50">
    <feGaussianBlur in="SourceGraphic" stdDeviation="50" />
  </filter>
  <filter id="SourceGraphic100">
    <feGaussianBlur in="SourceGraphic" stdDeviation="100" />
  </filter>
  <circle cx="60"  cy="160" r="40" fill="blue" />
  <circle cx="160" cy="160" r="40" fill="blue" filter="url(#SourceGraphic2)" />
  <circle cx="260" cy="160" r="40" fill="blue" filter="url(#SourceGraphic5)" />
  <circle cx="360" cy="160" r="40" fill="blue" filter="url(#SourceGraphic10)" />
  <circle cx="460" cy="160" r="40" fill="blue" filter="url(#SourceGraphic20)" />
  <circle cx="560" cy="160" r="40" fill="blue" filter="url(#SourceGraphic50)" />
  <circle cx="660" cy="160" r="40" fill="blue" filter="url(#SourceGraphic100)" />
</svg>

## feComposite

该滤镜执行两个输入图像的智能像素组合，在图像空间中使用以下`Porter-Duff`合成操作之一：

+ `in` – (see in attribute)

+ `operator` – 'over | in | out | atop | xor | arithmetic

  The compositing operation that is to be performed. All of the operator types except arithmetic match the correspond operation as described in [PORTERDUFF]. The arithmetic operator is described above. If attribute operator is not specified, then the effect is as if a value of 'over' were specified.

+ `k1, k2, k3, k4` – <number>

  Only applicable if operator = 'arithmetic'. If the attribute is not specified, the effect is as if a value of 0 were specified.

+ `in2` – (see in attribute)

  The second input image to the compositing operation. This attribute can take on the same values as the in attribute.



<section>
<div>
<h4>feFlood</h4> 
<svg width="100" height="100" viewBox="0 0 200 200">
  <defs>
    <filter id="floodFilter" filterUnits="userSpaceOnUse">
      <feFlood x="50" y="50" width="100" height="100" flood-color="#6ab150" flood-opacity="1"></feFlood>
    </filter>
  </defs>

  <use style="filter: url(#floodFilter);"></use>
</svg>
</div>

<div>
<h4>feFlood + feMerge</h4>
<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" viewBox="0 0 200 200">
  <defs>
  <filter id="ejemploMerge" filterUnits="userSpaceOnUse" x="0" y="0" width="110%" height="110%">
    <feFlood x="30" y="30" width="100" height="100" flood-color="#6ab150" flood-opacity="1" result="img1"></feFlood>
    <feFlood x="70" y="70" width="100" height="100" flood-color="#ff0000" flood-opacity="1" result="img2"></feFlood>
    <feMerge>
    <feMergeNode in="img2"></feMergeNode>
    <feMergeNode in="img1"></feMergeNode>
    </feMerge>
  </filter>
  </defs>
  <use style="filter: url(#ejemploMerge);"></use>
</svg>
</div>
  
<hr style="border-top: 1px solid #d9d9d9;">
  
<h3>feFlood + feComposite</h3>

<div>
<h4>operator="over"</h4>  
<svg width="100" height="100" viewBox="0 0 200 200">
  <defs>
  <filter id="ejemploOver" filterUnits="userSpaceOnUse" x="0" y="0" width="110%" height="110%">
    <feFlood x="30" y="30" width="100" height="100" flood-color="#6ab150" flood-opacity="1" result="img1"></feFlood>
    <feFlood x="70" y="70" width="100" height="100" flood-color="#ff0000" flood-opacity="1" result="img2"></feFlood>
    <feComposite in="img1" in2="img2" operator="over"></feComposite>
  </filter>
  </defs>
  <use style="filter: url(#ejemploOver);"></use>
</svg>
</div>

<div>
<h4>operator="in"</h4>
<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" viewBox="0 0 200 200">
  <defs>
  <filter id="ejemploIn" filterUnits="userSpaceOnUse" x="0" y="0" width="110%" height="110%">
    <feFlood x="30" y="30" width="100" height="100" flood-color="#6ab150" flood-opacity="1" result="img1"></feFlood>
    <feFlood x="70" y="70" width="100" height="100" flood-color="#ff0000" flood-opacity="1" result="img2"></feFlood>
    <feComposite in="img1" in2="img2" operator="in"></feComposite>
  </filter>
  </defs>
  <use style="filter: url(#ejemploIn);"></use>
</svg>
</div>

<div>
<h4>operator="out"</h4>
<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" viewBox="0 0 200 200">
  <defs>
  <filter id="ejemploOut" filterUnits="userSpaceOnUse" x="0" y="0" width="110%" height="110%">
    <feFlood x="30" y="30" width="100" height="100" flood-color="#6ab150" flood-opacity="1" result="img1"></feFlood>
    <feFlood x="70" y="70" width="100" height="100" flood-color="#ff0000" flood-opacity="1" result="img2"></feFlood>
    <feComposite in="img1" in2="img2" operator="out"></feComposite>
  </filter>
  </defs>
  <use style="filter: url(#ejemploOut);"></use>
</svg>
</div>

<div>
<h4>operator="atop"</h4>
<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" viewBox="0 0 200 200">
  <defs>
  <filter id="ejemploAtop" filterUnits="userSpaceOnUse" x="0" y="0" width="110%" height="110%">
    <feFlood x="30" y="30" width="100" height="100" flood-color="#6ab150" flood-opacity="1" result="img1"></feFlood>
    <feFlood x="70" y="70" width="100" height="100" flood-color="#ff0000" flood-opacity="1" result="img2"></feFlood>
    <feComposite in="img1" in2="img2" operator="atop"></feComposite>
  </filter>
  </defs>
  <use style="filter: url(#ejemploAtop);"></use>
</svg>
</div>

<div>
<h4>operator="xor"</h4>
<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" viewBox="0 0 200 200">
  <defs>
  <filter id="ejemploXor" filterUnits="userSpaceOnUse" x="0" y="0" width="110%" height="110%">
    <feFlood x="30" y="30" width="100" height="100" flood-color="#6ab150" flood-opacity="1" result="img1"></feFlood>
    <feFlood x="70" y="70" width="100" height="100" flood-color="#ff0000" flood-opacity="1" result="img2"></feFlood>
    <feComposite in="img1" in2="img2" operator="xor"></feComposite>
  </filter>
  </defs>
  <use style="filter: url(#ejemploXor);"></use>
</svg>
</div>


<div>
<h4>operator = "arithmetic"</h4>
<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" viewBox="0 0 200 200">
  <defs>
  <filter id="ejemploArithmetic" filterUnits="userSpaceOnUse" x="0" y="0" width="110%" height="110%">
    <feFlood x="30" y="30" width="100" height="100" flood-color="#6ab150" flood-opacity="1" result="img1"></feFlood>
    <feFlood x="70" y="70" width="100" height="100" flood-color="#ff0000" flood-opacity="1" result="img2"></feFlood>
    <feComposite in="img1" in2="img2" operator="arithmetic" k1="0" k2="1" k3=".5" k4="0"></feComposite>
  </filter>
  </defs>
  <use style="filter: url(#ejemploArithmetic);"></use>
</svg>
</div>




<div>
<h4>feFlood + SourceGraphic<br>operator="atop"</h4>
<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" viewBox="0 0 200 200">
 <defs>
  <filter id="compAtop" filterUnits="userSpaceOnUse" x="0" y="0" width="100%" height="100%">
    <feFlood x="30" y="30" width="80" height="80" flood-color="#ff0000" flood-opacity="1" result="img2"></feFlood>
    <feComposite in="img2" in2="SourceGraphic" operator="atop"></feComposite>
  </filter>
  </defs>  

<g id="paw" transform="translate(50 50)" filter="url(#compAtop)">
<path fill="#0D0C0C" d="M51.167,51.782c24.495-0.156,39.767,38.85,29.019,46.806c-12.013,8.893-6.553,1.092-27.459,0
	c-21.968-1.148-23.871,7.955-34.792-0.938C7.014,88.758,34.629,51.781,47.734,51.781C51.167,51.781,51.167,51.782,51.167,51.782z"></path>
<ellipse transform="matrix(0.8373 -0.5467 0.5467 0.8373 -24.3436 16.2985)" fill="#0D0C0C" cx="15.214" cy="49.053" rx="12.961" ry="19.502"></ellipse>
<ellipse transform="matrix(0.9874 -0.1585 0.1585 0.9874 -2.9143 6.3225)" fill="#0D0C0C" cx="38.192" cy="21.438" rx="13.081" ry="21.609"></ellipse>
<ellipse transform="matrix(0.9141 0.4055 -0.4055 0.9141 14.6185 -26.5587)" fill="#0D0C0C" cx="69.994" cy="21.224" rx="12.976" ry="21.25"></ellipse>
<ellipse transform="matrix(0.8197 0.5728 -0.5728 0.8197 46.3587 -41.7584)" fill="#0D0C0C" cx="89.508" cy="52.756" rx="13.514" ry="17.726"></ellipse>
</g>
</svg>
</div>

<hr style="border-top: 1px solid #d9d9d9;">
<div>
<h4>feImage<br>NOT working in Firefox</h4>
<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" viewBox="0 0 200 200">
 <defs>
  <rect width="200" height="200" id="Azul" fill="#009999"></rect>
  <filter id="compAtopAzul" filterUnits="userSpaceOnUse" x="0" y="0" width="100%" height="100%">
    <feImage xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#Azul" x="30" y="30" width="100%" height="100%" result="azul"></feImage>
    <feComposite in="azul" in2="SourceGraphic" operator="atop"></feComposite>
  </filter>  
  </defs> 
  

<g id="paw" transform="translate(50 50)" filter="url(#compAtopAzul)">
<path fill="#0D0C0C" d="M51.167,51.782c24.495-0.156,39.767,38.85,29.019,46.806c-12.013,8.893-6.553,1.092-27.459,0
	c-21.968-1.148-23.871,7.955-34.792-0.938C7.014,88.758,34.629,51.781,47.734,51.781C51.167,51.781,51.167,51.782,51.167,51.782z"></path>
<ellipse transform="matrix(0.8373 -0.5467 0.5467 0.8373 -24.3436 16.2985)" fill="#0D0C0C" cx="15.214" cy="49.053" rx="12.961" ry="19.502"></ellipse>
<ellipse transform="matrix(0.9874 -0.1585 0.1585 0.9874 -2.9143 6.3225)" fill="#0D0C0C" cx="38.192" cy="21.438" rx="13.081" ry="21.609"></ellipse>
<ellipse transform="matrix(0.9141 0.4055 -0.4055 0.9141 14.6185 -26.5587)" fill="#0D0C0C" cx="69.994" cy="21.224" rx="12.976" ry="21.25"></ellipse>
<ellipse transform="matrix(0.8197 0.5728 -0.5728 0.8197 46.3587 -41.7584)" fill="#0D0C0C" cx="89.508" cy="52.756" rx="13.514" ry="17.726"></ellipse>
</g>
</svg>
</div>
</section>  

## feOffset（位移）

偏移滤镜会将输入图形进行移动之后作为结果输出。你可以使用它来上下左右移动图形。通常偏移滤镜都是用于制作drop阴影效果。

+ `<filter>`标签的`（x，y）`分别是需要位移的起始位置
+ `<feOffset>` 中真正被移动的图像是`（x，y）`到`（width-x-dx，height-y-dy）`的矩形区域
`（dx，dy）`分别是相对原始图像向右和向下的位移

## feComponentTransfer

[参考文献-1](https://drafts.fxtf.org/filter-effects/) / 
[参考文献-2](https://www.w3.org/TR/SVG11/filters.html)

`feComponentTransfer`滤镜的主要作用为每个像素点颜色的转换，包括亮度、对比度、色彩平衡的调整等。

其采用的方法为对每一个颜色通道进行独立的操作。

`feComponentTransfer`拥有四个子元素：

+ `feFuncR`
+ `feFuncG`
+ `feFuncB`
+ `feFuncA`

分别对输入值的红色、绿色、蓝色与透明度四个通道的值进行数值处理。

四个子元素包括以下常用属性：

`type`：每一个通道下数值处理的方法类型。取值：`identity` / `table` / `discrete` / `linear` /`gamma`。

`identity`：颜色值不变化。

`table`：颜色值根据提供的tableValues值进行转变。

tableValues为n个值，v0，v1，...，vn-1，0=< v <= 1，值从v0到vn-1依次增大，将颜色通道分为n-1个范围。

如：
```html
<feFuncR type="table" tableValues="0.0 0.7 0.9 1.0" />
```
tableValues共有四个值，原始红色通道别划分为3个范围： 0.00 -- 0.33 ，0.33 -- 0.66 ， 0.66 -- 1.00。

`discrete`：类似table的颜色值计算方法，根据tableValues计算处理颜色值。

区别：tableValues的值可以不是依次增大的，通道划分的范围多一个。

同样，根据tableValues的值的数量，将原始通道划分不同范围。

例如：
```html
</span><feFuncR type="discrete" tableValues="0.0 0.7 0.0 1.0" />
```
原始红色通道别划分为4个范围： 0.00 -- 0.25 ，0.25 -- 0.50 ， 0.50 -- 0.75，0.75 -- 1.00。

`gamma`：使用`gamma`函数进行颜色数值处理。

参数： `amplitude` ， `exponent` ， `offset`

#  Inset Drop-shadow

[Doing an Inset Drop-shadow With SVG Filters](https://www.xanthir.com/b4Yv0)

使用SVG过滤器可能非常强大，但也非常复杂。特别是，SVG可以比CSS的box shadow属性做得更好，因为它可以响应您所做的任何操作的透明度，而不仅仅是隐藏元素的box。

但是，放置阴影过滤器不是一个原语：它是由更简单的过滤器效果构建而成，并且相当复杂：

```html
 <filter id="drop-shadow">
  <feGaussianBlur in="[alpha-channel-of-input]" stdDeviation="[radius]"/>
  <feOffset dx="[offset-x]" dy="[offset-y]" result="offsetblur"/>
  <feFlood flood-color="[color]"/>
  <feComposite in2="offsetblur" operator="in"/>
  <feMerge>
    <feMergeNode/>
    <feMergeNode in="[input-image]"/>
  </feMerge>
</filter> 
```

这只执行简单的放置阴影，元素在其下方投射阴影。修改它来做一个嵌入阴影，元素被“切除”，光源在其中投射阴影，这并不难，但是你必须知道原始阴影的所有小部分在做什么。

让我们把重点放在一边。下面是一个使用SVG过滤器的插入阴影：

<svg>
   <filter id="inset-shadow" x="-50%" y="-50%" width="200%" height="200%">
    <feComponentTransfer in=SourceAlpha>
      <feFuncA type="table" tableValues="1 0" />
    </feComponentTransfer>
    <feGaussianBlur stdDeviation="3"/>
    <feOffset dx="0" dy="-5" result="offsetblur"/>
    <feFlood flood-color="rgb(20, 0, 0)" result="color"/>
    <feComposite in2="offsetblur" operator="in"/>
    <feComposite in2="SourceAlpha" operator="in" />
    <feMerge>
      <feMergeNode in="SourceGraphic" />
      <feMergeNode />
    </feMerge>
  </filter> 
  <circle cx="50" cy="50" r="20" fill="red" filter="url(#inset-shadow)" />
</svg>

```html
<svg>
   <filter id="inset-shadow" x="-50%" y="-50%" width="200%" height="200%">
    <feComponentTransfer in=SourceAlpha>
      <feFuncA type="table" tableValues="1 0" />
    </feComponentTransfer>
    <feGaussianBlur stdDeviation="3"/>
    <feOffset dx="0" dy="-5" result="offsetblur"/>
    <feFlood flood-color="rgb(20, 0, 0)" result="color"/>
    <feComposite in2="offsetblur" operator="in"/>
    <feComposite in2="SourceAlpha" operator="in" />
    <feMerge>
      <feMergeNode in="SourceGraphic" />
      <feMergeNode />
    </feMerge>
  </filter> 
  <circle cx=50 cy=50 r=20 fill=red filter="url(#inset-shadow)" />
</svg>
```

让我们一个节点一个节点地走一遍。

首先，使用`x/y/width/height`属性设置过滤区域。默认情况下，整个过滤操作发生在一个比源图形大10%的框中。因为我们正在使用Blurs，它可以从源代码扩展一点，所以我们需要更多的Spaaaaaace。各方向都大50%一般都可以。

`<fecomponenttransfer>`我们首先用in=“sourceAlpha”获取源图形的alpha通道，然后反转它。.普通阴影使用图形的正常alpha通道，模糊并偏移它以产生阴影。在这里，源图形本身并没有投射阴影——其他一切都是——所以我们需要反转它的alpha。这给了我们一个图像，在图像是实心的情况下是透明的，而在图像是透明的情况下是实心的，我们将变成阴影。

`<fegaussianblur>`现在，我们对图像进行模糊处理，为阴影度做好准备，然后使用`<feoffset>`将其稍微移动一点，这样光源将看起来有点来自侧面。

这也是我们第一次看到结果属性。默认情况下，每个同级过滤器效果都将前一个过滤器效果的输出作为其输入。但是，如果您需要做一些复杂的事情，您可以命名一个特定的过滤效果的输出，并在后面显式地引用它。

接下来的两个元素有些不明显。当我们抓取“源alpha”时，它给了我们一个与源图像具有相同alpha通道的纯黑图像。如果我们想要一个黑色的阴影，那很好，但是如果我们想要给它上色，我们需要做一些工作。在这种情况下，我们使用`<feflood>`生成一个无限的颜色字段（我的示例只是再次使用黑色，无论什么），然后，重要的位使用`<fecomposite operator=“in”>`将其切碎为重叠模糊阴影的位。

有许多复合操作，但是“in”，特别是，基本上将第一个图像的alpha通道（在本例中，隐式地将前一个`<feflood>`效果的结果）与第二个图像的alpha通道（显式地指向offsetblur结果）相乘。就像使用模板一样，in2图像的实体部分让in图像显示出来，而透明部分阻止了它。

这给了我们一个正确的颜色模糊。如果你只想要一个黑色的阴影，你可以去掉这个`<feflood>`和第一个`<fecomposite>`，因为你之前生成的模糊开始变成黑色。

然后，我们再执行另一个`<fecomposite operator=“in”>`，这次使用sourceAlpha再次作为模具蒙版。这就消除了大部分模糊，只留下与源图像重叠的部分。

最后，我们将阴影和源图像合并为`<femerge>`和预定义的source image值，将阴影放在第二位，使其位于顶部。