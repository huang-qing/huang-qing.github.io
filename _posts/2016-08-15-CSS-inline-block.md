---
layout:     post
title:      display:inline-block
subtitle:   CSS display inline-block
date:       2016-08-15
author:     huangqing
header-img: img/post-bg-css.jpg
catalog: true
categories: [CSS]
tags:
    - CSS
---


# display:inline-block

## display值

~~~css

none ：隐藏对象。(注意，与visibility:hidden不同，这货是彻底的消失，不保留物理空间。)

inline ：指定对象为内联元素。

block ：指定对象为块元素。

list-item ：指定对象为列表元素。

inline-block ： 指定对象为内联块元素。

inline-table ：制定对象为内联表格。

table ： 指定对象为表格元素 等同<table>

table-caption ：指定对象为表格标题 等同<caption>

table-cell ： 指定对象为单元格 等同 <td>(好吧，各种等同，我这人比较懒，以下省略各种不必要的文字，大家按照表格对号入座就行)

table-row ：<tr>

table-row-group ：<tbody>

table-column ： <col>

table-column-group ：<colgroup>

table-header-group ： <thead>

table-footer-group ： <tfoot>

box(flex) ： 指定对象为弹性盒模型。

inline-box ： 指定对象为内联块级弹性盒模型。

campact ：指定对象为块元素或基于内容之上的内联元素。

run-in ：指定对象为块对象或基于内容之上的内联对象。

ruby ：指定对象为表格脚注组。

ruby-base ：指定对象为表格脚注组。

ruby-text ：指定对象为表格脚注组。

ruby-base-group ：指定对象为表格脚注组。

ruby-text-group ：指定对象为表格脚注组。
~~~

## inline-block 介绍

>This value causes an element to generate an inline-level block container. 
>The inside of an inline-block is formatted as a block box,and the element itself is formatted as an atomic inline-level box.
>
>`inline-block`元素把自己变成特殊的`inline`元素，对于相邻的元素来说表现出`inline`的特点，允许空格。对于内部元素来说表现出`block`元素的特点，可以设置高度和宽度。
>-W3C

## block 介绍 

block-level elements (块级元素)   
block元素通常被现实为独立的一块，会单独换一行。   
常见的块级元素有 `DIV`, `FORM`, `TABLE`, `P`, `PRE`, `H1~H6`, `DL`, `OL`, `UL` 等。  
block元素可以包含block元素和inline元素；但

## inline 介绍

inline elements (内联元素)   
inline元素则前后不会产生换行，一系列inline元素都在一行内显示，直到该行排满。   
常见的内联元素有 `SPAN`, `A`, `STRONG`, `EM`, `LABEL`, `INPUT`, `SELECT`, `TEXTAREA`, `IMG`, `BR` 等。
inline元素只能包含inline元素。

## block、inline、inlinke-block细节对比

`display:block`

+ block元素会独占一行，多个block元素会各自新起一行。默认情况下，block元素宽度自动填满其父元素宽度。
+ block元素可以设置`width`,`height`属性。块级元素即使设置了宽度,仍然是独占一行。
+ block元素可以设置`margin`和`padding`属性。

`display:inline`

+ inline元素不会独占一行，多个相邻的行内元素会排列在同一行里，直到一行排列不下，才会新换一行，其宽度随元素的内容而变化。
+ inline元素设置`width`,`height`属性无效。
+ inline元素的`margin`和`padding`属性，水平方向的`padding-left`, `padding-right`, `margin-left`, `margin-right`都产生边距效果；但竖直方向的`padding-top`, `padding-bottom`, `margin-top`, `margin-bottom`不会产生边距效果。

`display:inline-block`

简单来说就是将对象呈现为inline对象，但是对象的内容作为block对象呈现。之后的内联对象会被排列在同一行内。比如我们可以给一个`link`（a元素）`inline-block`属性值，使其既具有block的宽度高度特性又具有`inline`的同行特性。

## 兼容性

IE8及以上、Chrome、Firefox、Safari

IE6、7不支持css2.1中的inline-block,只是表现上的相近。这是通过触发浏览器的haslayout来实现的。

>haslayout是Windows Internet Explorer渲染引擎的一个内部组成部分。在InternetExplorer中，一个元素要么自己对自身的内容进行计算大小和组织，要么依赖于父元素来计算尺寸和组织内容。
为了调节这两个不同的概念，渲染引擎采用了 hasLayout 的属性，属性值可以为true或false。
当一个元素的 hasLayout属性值为true时，元素就会对自身的内容进行计算大小和组织，而不依赖于父元素。
display:inline-block所触发的hasLayout是不可逆转的，所以当*display:inline的时候并不会使hasLayout=false。

### inline-block hack 写法

~~~css
{
    display:inline-block;
    *display:inline;
    *zoom:1;
}
~~~

## inline-block格栅单元间隔

### 使用`font-size`和`letter-spacing`去除格栅单元间隔方法

inline-bolck对于相邻的元素表现的是inline的特点，允许空格，在换行的时候会产生空白间隙。在inline-block的外层设置这样式，来清除间隙：

~~~css
{
    font-size:0;
    letter-spacing:-6px;
}
~~~

1. 设置换行符or制表符or空格符的`font-size`为0，从而使之失去宽度。
2. `letter-spacing`是一个修复IE6、7下某些元素inline-block后1px间隙的bug和不支持font-size:0;的浏览器而存在的。

样式如下：

~~~css

.display-inline-block-container {
    font-size:0;/* 所有浏览器 */
    *word-spacing:-1px;/* IE6、7 */
}

@media screen and (-webkit-min-device-pixel-ratio:0){
    /* firefox 中 letter-spacing 会导致脱离普通流的元素水平位移 */
    .dib-wrap{
        letter-spacing:-5px;/* Safari 等不支持字体大小为 0 的浏览器, N 根据父级字体调节*/
    }
}

.display-inline-block-container>.display-inline-block{
    display: inline-block;
    *display:inline;
    *zoom:1;
    font-size: 12px;
    letter-spacing: normal;
    word-spacing: normal;
    vertical-align:top;
}
~~~

### [YUI 3 CSS Grids](http://yuilibrary.com/yui/docs/cssgrids/) 

使用`letter-spacing`和`word-spacing`去除格栅单元间隔方法（注意，其针对的是block水平的元素，因此对IE8-浏览器做了hack处理）：

~~~css

.yui3-g {
    letter-spacing: -0.31em; /* webkit */
    *letter-spacing: normal; /* IE < 8 重置 */
    word-spacing: -0.43em; /* IE < 8 && gecko */
}
 
.yui3-u {
    display: inline-block;
    zoom: 1; *display: inline; /* IE < 8: 伪造 inline-block */
    letter-spacing: normal;
    word-spacing: normal;
    vertical-align: top;
}
~~~