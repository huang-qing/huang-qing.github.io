---
layout:     post
title:      Quadratic Bezier
subtitle:   贝塞尔曲线
date:       2018-11-29
author:     huangqing
header-img: img/post-bg-svg.png
catalog: true
categories: [SVG]
tags:
    - SVG
---

# 二次贝塞尔曲线

[CSS3贝塞尔曲线](http://cubic-bezier.com/)

[二次、三次贝塞尔曲线呈现工具](http://dayu.pw/svgcontrol/)

## 描述

一个标准的3次贝塞尔曲线需要4个点：起始点、终止点（也称锚点）以及两个相互分离的中间点。
贝塞尔曲线需要的4个点。

![](/images/svg/bezier-four-points.gif)

无论SVG, Canvas还是CSS3动画与贝塞尔搞基，都牵扯到这4个点。


## 指令`Q`

```css
Q(q) cx cy x y
```

从当前点画一条到`(x, y)`的二次贝塞尔曲线, 曲线的控制点为`(cx, cy)`. 

![](/images/svg/1415845715278-bezier-quadratic-animation.gif)

典型的Q对应的二次贝塞尔曲线如下图：

![](/images/svg/2014-06-12_161557.png)

对应的指令和参数是：

```
Q x1 y1, x y
```


## 指令`T`

```css
T(t) x y	
```

此命令`只能跟在一个 Q 命令使用`, 假设 `Q` 命令生成曲线 `s`, `T` 命令的作用是从 `s `的终点再画一条到`(x y)`的二次贝塞尔曲线, `曲线的控制点为 s 控制点关于 s 终点的对称点`. T 命令生成的曲线会非常平滑

实例：

![](/images/svg/3042485223-56acefa99077e_articlex.png)


以上曲线的路径表示是: 
```html
<path d="M200,300 Q400,50 600,300 T1000,300"/>
```
我们可以看出:

A 是起点, B 是终点, C 就是控制点.

找出 AC 的重点 D, BC 的重点 E, 连接 DE, 找出其中点 F, F 即这条曲线前半段的一个切点.

**再来看 T 命令, 其实 T 命令是 Q 的一个简写.T命令中的点即是图中的G点**

其控制点 H 就是上个 Q 命令的控制点 C 关于终点 B 的对称点.

使用 T 命令产生的曲线往往比较顺滑。

# 三次贝塞尔曲线

## 指令`C`

```css
C x1 y1, x2 y2, x y	
```

三次贝塞尔曲线指令：C x1 y1, x2 y2, x y两个控制点(x1,y1)和(x2,y2)，(x,y)代表曲线的终点。字母C表示特定动作与行为，这里需要大写，表示标准三次方贝塞尔曲线。

![](/images/svg/2014-06-12_101433.png)

![](/images/svg/21152048-9b5dee31b19349428c453b8bd5e20a3d.gif)


逗号区分纵横轴：

```html
<svg id="svg" width="200" height="100">
    <desc>三次贝塞尔曲线</desc><defs></defs>
    <path d="M20,20 C90,40 130,40 180,20" stroke="#000000" fill="none" style="stroke-width: 2px;"></path>
    <text x="90" y="60">A杯罩</text>
</svg>
```

或者逗号用来区分每个点坐标（主流写法）：

```html
<svg id="svg" width="200" height="100">
    <desc>三次贝塞尔曲线</desc><defs></defs>
    <path d="M20 20 C90 40, 130 40, 180 20" stroke="#000000" fill="none" style="stroke-width: 2px;"></path>
    <text x="90" y="60">A杯罩</text>
</svg>
```

或者不需要逗号：

```html
<svg id="svg" width="200" height="100">
    <desc>三次贝塞尔曲线</desc><defs></defs>
    <path d="M20 20 C90 40 130 40 180 20" stroke="#000000" fill="none" style="stroke-width: 2px;"></path>
    <text x="90" y="60">A杯罩</text>
</svg>
```

## 指令`S`

```css
S(s) cx2 cy2 x y	
```

此命令`只能跟在 C 命令后使用`, 假设 `C` 命令生成曲线 s, `S` 命令的作用是再画一条到 `(x, y)`的三次贝塞尔曲线,曲线的终点控制点是 `(cx2, cy2)`, `曲线的开始控制点是 s 的终点控制点关于 s 终点的对称点`.


实例：

![](/images/svg/3627645763-56acefbe5aa99_articlex.png)

```html
<path d="M100,200 C100,100 250,100 250,200 S400,300 400,200"/>
```

前半段曲线(C 命令) s1:

起点 `A`, 终点 `B`, 起点控制点 `C`, 终点控制点 `D`, 连接 AC, BD, CD;

找到 CD 中点 F, 连接 AC 中点 E 与 F, 连接 BD 中点 G 与 F;

连接 EF, FG, 连接 EF 中点 H 与 FG 中点 I;

I 即为前半段曲线的切点;

后半段曲线(S 命令) s2:

S 只能跟在 C 命令后使用;

s2 的起点 `B`, 终点 `L`, 终点控制点 `K`;

s2 的起点控制点是 s1 的终点控制点 D 关于 s1终点 B 的对称点

下图是更多关于三次贝塞尔曲线的例子:

![](/images/svg/3330912387-56acefcda6f67_articlex.png)

### 其他


张鑫旭提炼了下可以脱颖而出的技能组合：

![](/images/svg/2019-01-28_115028.png)

所以，要想前端有所成，有两条路，一是往前，webGL, canvas, SVG领域，这需要对图形敏感，有设计感，有动画素养，有相当的数学知识，以及最重要的JavaScript控制能力；一是往后，走开发路线，工具，富应用，运维（数据统计、前端安全、前端部署）领域，这需要懂后台、计算机网络、逻辑思考能力，以及最重要的JavaScript开发功力。