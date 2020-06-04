---
layout:     post
title:      CSS Grid Layout - How To Use
subtitle:   如何使用 CSS Grid 快速而又灵活的布局
date:       2018-03-16
author:     huangqing
header-img: img/post-bg-css.jpg
catalog: true
categories: [CSS]
tags:
    - CSS
---


`CSS Grid(网格)` 模块是创建网站布局一个非常棒的工具。它能使你快速地进行布局尝试，比你尝试过的任何其他布局系统都快。


## 要创建的网格

我们将模仿一个经典网站布局，从非常基本的 Grid(网格) 开始：

![](/images/css/1_lICyKxZL1ID50fbw2MSXOQ.gif)

首先，我将解释我们需要的 `HTML` 和 `CSS` ，我已经将其分解为四个部分。 一旦你了解了这些，我们将继续进行布局试验。


#### 1、HTML 结构

我们需要的第一件事是一点 `HTML` 。 一个网格容器（将变成一个网格元素）和网格项（`header`, `menu`, `content`, `footer`）。

```html
<div class="container">
  <div class="header">HEADER</div>
  <div class="menu">MENU</div>
  <div class="content">CONTENT</div>
  <div class="footer">FOOTER</div>
</div>
```

#### 2、设置基本的 CSS

那么我们需要在 `container` 元素设置 `display: grid`; ，将其设置为网格容器，并指定我们需要多少行和列。这是基本的CSS：

```css
.container {
    display: grid;    
    grid-template-columns: repeat(12, 1fr);
    grid-template-rows: 50px 350px 50px;
    grid-gap: 5px;
}
```

我将在后面介绍内容，但是我首先让你了解一下。

以上代码的意思是：使用 `grid-template-columns` 属性创建一个 12 列的网格，每个列都是一个单位宽度（总宽度的 1/12 ）。（愚人码头注：其中， `repeat(12, 1fr)` 意思是 12 个重复的 `1fr` 值。  `fr` 是网格容器剩余空间的等分单位。）使用 `grid-template-rows` 属性创建 3 行，第一行高度是 50px ，第二行高度是 350px 和第三行高度是 50px。最后，使用 `grid-gap` 属性在网格中的网格项之间添加一个间隙。

#### 3、添加 grid-template-areas

这个属性被称为网格区域，也叫模板区域，能够让我们轻松地进行布局实验。

要将它添加到网格中，我们只需给网格容器加一个 `grid-template-areas` 属性即可。 语法可能有点奇怪，因为它不像其他的 CSS 语法。例如：

```css
.container {
    display: grid;
    grid-gap: 5px;    
    grid-template-columns: repeat(12, 1fr);
    grid-template-rows: 50px 350px 50px;
    grid-template-areas:
        "h h h h h h h h h h h h"
        "m m c c c c c c c c c c"
        "f f f f f f f f f f f f";
}
```

`grid-template-areas` 属性背后的逻辑是你在代码中创建的网格可视化表示。正如你所看到的，它有 3 行 12 列，和我们在 `grid-template-columns` 和 g`rid-template-rows` 中定义的正好呼应。

每行代表一行，用网格术语来说是 `网格轨道(Grid Track)` ，每个字符（ `h`，`m`，`c`，`f`）代表一个网格单元格。愚人码头注：其实是 `网格区域(Grid Area)` 名称，你可以使用任意名称。

四个字母中的每一个现在都形成一个矩形 `grid-area` 。

你可能已经猜到，我选择了字符 `h`，`m`，`c`，`f`，是因为他们是 `header`, `menu`, `content` 和  `footer` 的首字母。 当然，我可以把它们叫做任何想要的名称，但是使用他们所描述的东西的第一个字符更加容易让人理解。

#### 4、给网格项设定网格区域名称

现在我们需要将这些字符与网格中的网格项建立对应的连接。 要做到这一点，我们将在网格项使用  `grid-area` 属性：

```css
.header {
    grid-area: h;
}
.menu {
    grid-area: m;
}
.content {
    grid-area: c;
}
.footer {
   grid-area: f;
}
```

以下是完整的布局效果：

![](/images/css/1_F1NggJ-SsRSq306y3vCong.png)

尝试其他布局

现在，我们开始讨论 Grid(网格) 布局 特性的精妙之处，因为我们可以很容易地对布局进行修改尝试。只需修改  `grid-template-areas` 属性的字符即可。举个例子，把 menu 移到右边：

```css
grid-template-areas:
        "h h h h h h h h h h h h"
        "c c c c c c c c c c m m"
        "f f f f f f f f f f f f";
```

修改后的布局效果：

![](/images/css/1_DXjJkGRM_aVa2lVTOj0NyQ.png)

我们可以使用点` . `来创建空白的网格单元格。

```css
grid-template-areas:
        ". h h h h h h h h h h ."
        "c c c c c c c c c c m m"
        ". f f f f f f f f f f .";
```

修改的布局效果看起来是这样的：

![](/images/css/1_Uf_PFaU_EA82a-eBA7DHmw.png)


#### 添加响应式布局

将 `Grid(网格) 布局`与`响应式布局`结合起来，简直就是一个杀手锏。因为在 Grid(网格) 布局之前，仅使用 HTML 和 CSS 实现的响应式布局不可能的做到简单而又完美。假设你想在移动设备上查看的是：标题旁边是菜单。那么你可以简单地这样做：

```css
@media screen and (max-width: 640px) {
    .container {
        grid-template-areas:
            "m m m m m m h h h h h h"
            "c c c c c c c c c c c c"
            "f f f f f f f f f f f f";
    }
}
```

你可以看到以下效果：

![](/images/css/1_NDx9-qlf2I3YHv5Kjs4HOQ.gif)

请记住，所有这些更改都是使用纯 `CSS` 完成的，不需要修改` HTML` 。 无论 `div` 标签如何在 HTML 中是怎么样的顺序结构，我们都可以随意转换。（愚人码头注：这点与 flexbox 类似，网格项（grid items）的源顺序无关紧要。你的 CSS 可以以任何顺序放置它们，这使得使用` 媒体查询（media queries）`重新排列网格变得非常容易。）

`这被称为结构和表现分离， Grid(网格) 布局真正做到了这点，对于 CSS 来说是一个巨大的进步`。

它允许 HTML 成为它想要的样子: 作为内容的标记。HTML 结构不再受限于样式表现，比如不要为了实现某种布局而多次嵌套，现在这些都可以让 CSS 来完成。

## [minmax()函数](https://www.w3cplus.com/css3/how-the-minmax-function-works.html)

minmax()六种类型的值：
+ 长度值
+ 百分比值
+ 弹性值
+ max-content
+ min-content
+ auto


使用minmax()函数，可以指定网格中黄色单元格宽度保持在100px至200px之间。随着浏览器窗口的大小改变，这绝对值将会改变，但总是在这两个范围之内变化。

### 长度值:

```css
.grid{
    display: grid;
    grid-template-columns: minmax(100px,200px) 1fr 1fr;
}
```

### 百分比值:

```css
.grid{
    display:grid;
    grid-template-columns: minmax(200px,50%) 1fr 1fr;
}
```

### 弹性长度:

```css
.grid{
    display:grid;
    grid-template-columns: minmax(200px,1fr) 1fr 1fr;
}
```

![](/images/css/grid-layout-fr.gif)


### max-content

`max-content`关键词是一个特殊的值，它代表了单元格“最理想的大小”。网格单元格最小的宽度围绕它的内容。

```css
.grid{
    display:grid;
    grid-template-columns:minmax(max-content,max-content) 1fr 1fr;
}
```

![](/images/css/grid-layout-max-content.gif)

### max-content

`min-content`关键词和`max-content`一样，是一种特殊的值。它代表单元格最小宽度，可以不让内容溢出单元格，除非是不可避免的.
```css
.grid{
    display:grid;
    grid-template-columns: minmax(min-content,min-content) 1fr 1fr;
}
```

![](/images/css/grid-layout-min-content.gif)

### auto

通过`min-width`或`min-height`来指定

```css
.grid{
    display: grid;
    grid-template-columns: minmax(auto,auto) 1fr 1fr;
}
```

![](/images/css/grid-layout-auto.gif)

### 不使用媒体查询实现响应式布局

格中的每个列的最小宽度为200px。根据浏览器视窗大小，网格数量会根据共最合理的宽度进行变化。

```css
.grid{
    display:grid;
    grid-template-columns: repeat(auto-fit, minmax(200px,1fr));
}
```

![](/images/css/grid-layout-responsive.gif)

+ `repeat()`：这个函数允许我们给网格多个列指定相同的值。它也接受两个值：重复的次娄和重复的值
+ `auto-fit`：给repeat()函数使用这个关键词，来替代重复次数。这可以根据每列的宽度灵活的改变网格的列数

