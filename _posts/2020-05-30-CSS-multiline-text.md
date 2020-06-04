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

## [CSS定制多行省略](https://segmentfault.com/a/1190000008649988)

<style>
@-webkit-keyframes width-change {
  0%,100%{width: 320px} 50%{width:260px}
}
</style>
<div style="font-size:12px;line-height: 18px;-webkit-animation: width-change 20s ease infinite;background: rgb(230, 230, 230);">
    <div style="float:right;margin-left: -50px;width:100%;position:relative;background: hsla(229, 100%, 75%, 0.5);">
    腾讯成立于1998年11月，是目前中国领先的互联网增值服务提供商之一。成立10多年来，腾讯一直秉承“一切以用户价值为依归”的经营理念，为亿级海量用户提供稳定优质的各类服务，始终处于稳健发展状态。2004年6月16日，腾讯控股有限公司在香港联交所主板公开上市(股票代号700)。</div>
    <div style="float:right;position:relative;width:50px;height: 108px;color:transparent;background: hsla(334, 100%, 75%, 0.5);">placeholder</div>
    <div style="float:right;width:50px;height:18px;position: relative;background: hsla(27, 100%, 75%, 0.5);">...更多</div>
</div>

```html
<style>
    @-webkit-keyframes width-change {

        0%,
        100% {
            width: 320px
        }

        50% {
            width: 260px
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
        腾讯成立于1998年11月，是目前中国领先的互联网增值服务提供商之一。成立10多年来，腾讯一直秉承“一切以用户价值为依归”的经营理念，为亿级海量用户提供稳定优质的各类服务，始终处于稳健发展状态。2004年6月16日，腾讯控股有限公司在香港联交所主板公开上市(股票代号700)。
    </div>
    <div class="content-placeholder">placeholder</div>
    <div class="content-more">...更多</div>
</div>
```

利用右浮动原理——右浮动元素从右到左依次排列，不够空间则换行。蓝色块、粉色块、橙色块依次右浮动，蓝色块高度小于6行文字时，橙色块在右边，蓝色块高度大于6行文字时，左下角刚好够橙色块排列的空间，于是橙色块就到左边了

## javascript

[Clamp.js](https://github.com/josephschmitt/Clamp.js)

[jQuery插件-jQuery.dotdotdot](https://github.com/FrDH/dotdotdot-js)