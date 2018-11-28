---
layout:     post
title:      CSS3-Matrix 2D
subtitle:   矩阵 2D转换
date:       2018-11-28
author:     huangqing
header-img: img/post-bg-css.jpg
catalog: true
categories: [CSS]
tags:
    - CSS
---

#  CSS Matrix 2D

## 2D Transform Methods

|Function|	Description|
|----|----|
|`matrix(n,n,n,n,n,n)`	|Defines a 2D transformation, using a matrix of six values|
|`translate(x,y)`	|Defines a 2D translation, moving the element along the X- and the Y-axis|
|`translateX(n)`	|Defines a 2D translation, moving the element along the X-axis|
|`translateY(n)`	|Defines a 2D translation, moving the element along the Y-axis|
|`scale(x,y)`	|Defines a 2D scale transformation, changing the elements width and height|
|`scaleX(n)`	|Defines a 2D scale transformation, changing the element's width|
|`scaleY(n)`	|Defines a 2D scale transformation, changing the element's height|
|`rotate(angle)`	|Defines a 2D rotation, the angle is specified in the parameter|
|`skew(x-angle,y-angle)`	|Defines a 2D skew transformation along the X- and the Y-axis|
|`skewX(angle)`	|Defines a 2D skew transformation along the X-axis|
|`skewY(angle)`	|Defines a 2D skew transformation along the Y-axis|

## 概念

`matrix`可以代替：`偏移量（translate）`,`缩放（scale）`，`斜切（skew）`，`旋转（rotate）`四大功能。任意一个通过以上四个功能实现的形状改变的效果都能使用`matrix`来实现。

![matrix功能实现](/images/css/4940396-c8a054bfdb0ac3c5.png)

## 理解

`matrix`的六个参数用字母表示如下：

```css
selector {
    transform: matrix(a, b, c, d, e, f);
}
```

#### 矩阵：


2D 的转换是由一个 3*3 的矩阵表示的，前两行代表转换的值，分别是 a b c d e f，要注意是竖着排的，第一行代表 x 轴发生的变化，第二行代表 y 轴发生的变化，第三行代表 z 轴发生的变化，因为这里是 2D 不涉及 z 轴，所以这里是 0 0 1。

![矩阵](/images/css/4940396-440e8d62330e3a3b.png)


1. `e`和`f` 代表着偏移量`translate`，`x`和`y`轴

2. `a`和`d` 代表着缩放比例`scale`,`x` 和`y`轴

3. `b`和`c` 代表着斜切`skew`（具体参数和角度关系为, `b-->tanθ y轴` `c-->tanθ x轴` ,我们具体操作的时候还是要使用小数的）

4. `abcd` 中的`ad`代表缩放(`scale`),`bc`代表者斜切(`skew`); `abcd`四个参数代表着`旋转`。旋转可以用缩放和斜切一起用来得到, 两者联系在于这个角度θ。具体如下：
`matrix(cosθ,sinθ,-sinθ,cosθ,0,0)`

#### 变换公式

![变换公式](/images/css/4940396-ca61d7c251d6f9b0.png)

+ `ax+cy+e`表示变换后的水平坐标
+ `bx+dy+f`表示变换后的垂直位置

#### 平移

![平移](/images/css/2018-11-28_151004.png)

所以`translate(x,y)`也可以写成`matrix(1,0,0,1,x,y)`

#### 缩放

![缩放](/images/css/2018-11-28_150934.png)

所以`scale(x,y)`也可以写成`matrix(x,0,0,y,0,0)`

#### 旋转

![旋转](/images/css/2018-11-28_151035.png)

所以`rotate(x)`也可以写成`matrix(cos(x),-sin(x),sin(x),cos(x),0,0)`

#### 变形 

![变形](/images/css/2018-11-28_151101.png)

所以`skew(x,y)`也可以写成`matrix(1,tan(y),tan(x) ,1,0,0)`
