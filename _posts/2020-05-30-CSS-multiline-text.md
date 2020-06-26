---
layout:     post
title:      多行文本
subtitle:   多行文本溢出显示省略号(...)
date:       2020-05-30
author:     huangqing
header-img: img/post-bg-css.jpg
catalog: true
categories: [CSS]
tags:
    - CSS
---

## 单行文本溢出显示省略号

```css
.text{
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
```

## WebKit浏览器

`-webkit-line-clamp`:用来限制在一个块元素显示的文本的行数。

```css
.text{
  overflow:hidden;
  text-overflow:ellipsis;
  display:-webkit-box;
  -webkit-line-clamp:2;
  -webkit-box-orient: vertical;
}
```

## [CSS定制多行省略1](https://segmentfault.com/a/1190000008649988)

<style>
@-webkit-keyframes width-change {
  0%,100%{width: 320px} 50%{width:200px}
}
</style>
<div style="font-size:12px;line-height: 18px;-webkit-animation: width-change 10s ease infinite;background: rgb(230, 230, 230);">
    <div style="float:right;margin-left: -50px;width:100%;position:relative;background: hsla(229, 100%, 75%, 0.5);">
    腾讯成立于1998年11月，是目前中国领先的互联网增值服务提供商之一。
    成立10多年来，腾讯一直秉承“一切以用户价值为依归”的经营理念，为亿级海量用户提供稳定优质的各类服务，始终处于稳健发展状态。
    2004年6月16日，腾讯控股有限公司在香港联交所主板公开上市(股票代号700)。</div>
    <div style="float:right;position:relative;width:50px;height: 108px;color:transparent;background: hsla(334, 100%, 75%, 0.5);">placeholder</div>
    <div style="float:right;width:50px;height:18px;position: relative;background: hsla(27, 100%, 75%, 0.5);">...更多</div>
</div>

<div style='clear:both;'></div>

```html
<style>
    @-webkit-keyframes width-change {

        0%,
        100% {
            width: 320px
        }

        50% {
            width: 200px
        }
    }

    .container {
        font-size: 12px;
        line-height: 18px;
        background: rgb(230, 230, 230);
        -webkit-animation: width-change 20s ease infinite;
    }

    .content {
        float: right;
        margin-left: -50px;
        width: 100%;
        position: relative;
        background: hsla(229, 100%, 75%, 0.5);
    }

    .content-placeholder {
        float: right;
        position: relative;
        width: 50px;
        height: 108px;
        color: transparent;
        background: hsla(334, 100%, 75%, 0.5);
    }

    .content-more
    {
        float:right;
        width:50px;
        height:18px;
        position: relative;
        background: hsla(27, 100%, 75%, 0.5);
    }

    .content-more
    {
      float:right;
      width:50px;
      height:18px;
      position: relative;
      background: hsla(27, 100%, 75%, 0.5);
      /*重新定位*/
      left: 100%;
      -webkit-transform: translate(-100%,-100%);
    }
</style>
<div class='container'>
    <div class="content">
        腾讯成立于1998年11月，是目前中国领先的互联网增值服务提供商之一。
        成立10多年来，腾讯一直秉承“一切以用户价值为依归”的经营理念，为亿级海量用户提供稳定优质的各类服务，始终处于稳健发展状态。
        2004年6月16日，腾讯控股有限公司在香港联交所主板公开上市(股票代号700)。
    </div>
    <div class="content-placeholder">placeholder</div>
    <div class="content-more">...更多</div>
</div>
```

利用右浮动原理——右浮动元素从右到左依次排列，不够空间则换行。蓝色块、粉色块、橙色块依次右浮动，蓝色块高度小于6行文字时，橙色块在右边，蓝色块高度大于6行文字时，左下角刚好够橙色块排列的空间，于是橙色块就到左边了


## [CSS定制多行省略2](https://blog.csdn.net/weixin_33805557/article/details/91425021)

```html
<style>
    .wrap {
        /*需要定高*/
        height: 100px;
        /*用来设置显示多少行才省略，值一般为wrap的height值/行数求得，但是这个行数会受到字体大小的限制*/
        /*字体太大了，设置显示很多行也会很丑，都挤一块了，所以这个实际值，要看具体需求和实践*/
        line-height: 25px;
        /*加上此属性显示效果更佳，就算部分浏览器不支持也影响不大*/
        text-align: justify;
        overflow: hidden;
    }

    .wrap:before {
        float: left;
        /*这个值可以随意设定，不论单位还是什么*/
        width: 1em;
        height: 100%;
        content: '';
    }

    .wrap:after {
        float: right;
        /*大小随意，设置em单位最好，可随字体大小变化而自适应*/
        /*如果要采用以下渐变效果，那么这个值要大于before里的width值效果会比较好点*/
        /*值越大，渐变的效果越明显影响的范围越大。*/
        width: 2.5em;
        /*与父元素wrap的行高实际px值一样*/
        height: 25px;
        /*此值要跟自身宽度一样，取负值*/
        margin-left: -2.5em;
        /*此值要跟before宽度一样*/
        padding-right: 1em;
        content: '...';
        text-align: right;
        /*这里开始利用在float布局的基础上进行定位移动了*/
        position: relative;
        /*与父元素wrap的行高实际值一样，取负值*/
        top: -25px;
        left: 100%;
        /*设置渐变效果是为了省略号和内容衔接得自然点，没那么突兀，要注意要跟文字所在的背景的颜色搭配（把white替换成背景色）*/
        background: #fff;
        background: -webkit-gradient(linear, left top, right top, from(rgba(255, 255, 255, 0)), to(white), color-stop(50%, white));
        background: -moz-linear-gradient(to right, rgba(255, 255, 255, 0), white 50%, white);
        background: -o-linear-gradient(to right, rgba(255, 255, 255, 0), white 50%, white);
        background: -ms-linear-gradient(to right, rgba(255, 255, 255, 0), white 50%, white);
        background: linear-gradient(to right, rgba(255, 255, 255, 0), white 50%, white);
    }

    .wrap .text {
        float: right;
        /*该值要等于wrap:before的width值*/
        margin-left: -1em;
        width: 100%;
    }
</style>

<body>

    <div class="wrap">
        <span class="text">
            示例2：
            腾讯成立于1998年11月，是目前中国领先的互联网增值服务提供商之一。成立10多年来，腾讯一直秉承“一切以用户价值为依归”的经营理念，为亿级海量用户提供稳定优质的各类服务，始终处于稳健发展状态。2004年6月16日，腾讯控股有限公司在香港联交所主板公开上市(股票代号700)
        </span>
    </div>

</body>
```

## javascript

[Clamp.js](https://github.com/josephschmitt/Clamp.js)

[jQuery插件-jQuery.dotdotdot](https://github.com/FrDH/dotdotdot-js)