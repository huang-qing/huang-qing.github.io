# CSS实现滚动视差

## 何为滚动视差

视差滚动（Parallax Scrolling）是指让多层背景以不同的速度移动，形成立体的运动效果，带来非常出色的视觉体验。 作为网页设计的热点趋势，越来越多的网站应用了这项技术。

![](/images/css/parallax-scrollling-640.gif)

通常而言，滚动视差在前端需要辅助 `Javascript` 才能实现。当然，其实 `CSS` 在实现滚动视差效果方面，也有着不俗的能力。下面就让我们来见识一二：

认识 `background-attachment`

`background-attachment` 算是一个比较生僻的属性，基本上平时写业务样式都用不到这个属性。但是它本身很有意思。

`background-attachment`：如果指定了 `background-image` ，那么 `background-attachment` 决定背景是在视口中固定的还是随着包含它的区块滚动的。

单单从定义上有点难以理解，随下面几个 Demo 了解下 `background-attachment` 到底是什么意思：

```css
background-attachment: scroll
```

`scroll` 此关键字表示背景相对于元素本身固定， 而不是随着它的内容滚动。

```css
background-attachment: local
```

`local` 此关键字表示背景相对于元素的内容固定。如果一个元素拥有滚动机制，背景将会随着元素的内容滚动， 并且背景的绘制区域和定位区域是相对于可滚动的区域而不是包含他们的边框。

```css
background-attachment: fixed
```

`fixed` 此关键字表示背景相对于视口固定。即使一个元素拥有滚动机制，背景也不会随着元素的内容滚动。

注意一下 `scroll` 与 `fixed`，一个是相对元素本身固定，一个是相对视口固定，有点类似 `position` 定位的 `absolute` 和 `fixed`。

可以感受下 3 种不同取值的不同效果：
[CodePen Demo — bg-attachment Demo](https://codepen.io/Chokcoco/pen/xJJorg)

使用 `background-attachment: fixed` 实现滚动视差

首先，我们使用 `background-attachment: fixed` 来实现滚动视差。`fixed` 此关键字表示背景相对于视口固定。即使一个元素拥有滚动机制，背景也不会随着元素的内容滚动。

这里的关键在于，即使一个元素拥有滚动机制，背景也不会随着元素的内容滚动。也就是说，`背景图从一开始就已经被固定死在初始所在的位置`。

我们使用，图文混合排布的方式，实现滚动视差，`HTML` 结构如下，`.g-word` 表示内容结构，`.g-img` 表示背景图片结构：

```html
<section class="g-word">Header</section>
<section class="g-img">IMG1</section>
<section class="g-word">Content1</section>
<section class="g-img">IMG2</section>
<section class="g-word">Content2</section>
<section class="g-img">IMG3</section>
<section class="g-word">Footer</section>
```

关键 CSS：

```css
section {
    height: 100vh;
}

.g-img {
    background-image: url(...);
    background-attachment: fixed;
    background-size: cover;
    background-position: center center;
}
```

[CodePen Demo](https://codepen.io/Chokcoco/pen/JBaQoY)

## 使用 `transform: translate3d` 实现滚动视差

言归正传，下面介绍另外一种使用 CSS 实现的滚动视差效果，利用的是 CSS 3D。

原理就是：

我们给容器设置上 `transform-style: preserve-3d` 和` perspective: xpx`，那么处于这个容器的子元素就将位于3D空间中，
再给子元素设置不同的` transform: translateZ()`，这个时候，不同元素在 `3D` Z轴方向距离屏幕（我们的眼睛）的距离也就不一样
滚动滚动条，由于子元素设置了不同的 `transform: translateZ()`，那么他们滚动的上下距离 translateY 相对屏幕（我们的眼睛），也是不一样的，这就达到了滚动视差的效果。

关于 `transform-style: preserve-3d` 以及 `perspective` 本文不做过多篇幅展开，默认读者都有所了解，还不是特别清楚的，可以先了解下 `CSS 3D`。

核心代码表示就是：

```html
<div class="g-container">
    <div class="section-one">translateZ(-1)</div>
    <div class="section-two">translateZ(-2)</div>
    <div class="section-three">translateZ(-3)</div>
</div>
```

```css
html {
    height: 100%;
    overflow: hidden;
}

body {
    perspective: 1px;
    transform-style: preserve-3d;
    height: 100%;
    overflow-y: scroll;
    overflow-x: hidden;
}

.g-container {
    height: 150%;

    .section-one {
        transform: translateZ(-1px);
    }
    .section-two {
        transform: translateZ(-2px);
    }
    .section-three {
        transform: translateZ(-3px);
    }
}
```

