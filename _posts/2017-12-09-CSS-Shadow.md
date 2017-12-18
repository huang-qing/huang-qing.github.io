---
layout:     post
title:      CSS3 Shadow
subtitle:   
date:       2017-12-10
author:     huangqing
header-img: img/post-bg-css.jpg
catalog: true
categories: [CSS]
tags:
    - CSS
---

## 简介

The `box-shadow` CSS property is used to add shadow effects around an element's frame. You can specify multiple effects separated by commas if you wish to do so. A box shadow is described by `X` and `Y` offsets relative to the element, `blur` and `spread` radii, and `color`.


## 语法（Syntax）

```css     
 box-shadow:inset? && [ <offset-x> <offset-y> <blur-radius>? <spread-radius>? && <color>? ]
```

Specify a single box-shadow using:

+ Two, three, or four <length> values.
    + If only two values are given, they are interpreted as <offset-x><offset-y> values.
    + If a third value is given, it is interpreted as a <blur-radius>.
    + If a fourth value is given, it is interpreted as a <spread-radius>.
+ Optionally, the inset keyword.
+ Optionally, a <color> value.

To specify multiple shadows, provide a comma-separated list of shadows.

## 例子

<style>

div.shadow {
    font-size:12px;
    width: 26em;
    padding: 0.5em;
    margin: 1.6em auto;
    text-shadow: 1px 1px rgba(240,230,140,0.4);
    white-space: pre;
    background: #F0E68C;
}

div.shadow.A {
    padding: 2em;
    border-radius: 50%;
    box-shadow: inset 0.3em 0.3em 0.9em black;
}
</style>

<div class='shadow A'>border-radius: 50%;
box-shadow: inset 0.3em 0.3em 0.9em black;
</div>

<hr>

<div class='shadow' style="box-shadow: red 0.5em -0.5em 0.4em, gold 0.5em 0.5em 0.4em, lime -0.5em 0.5em 0.4em, blue -0.5em -0.5em 0.4em;"> 0.5em -0.5em 0.4em red,
 0.5em  0.5em 0.4em gold,
-0.5em  0.5em 0.4em lime,
-0.5em -0.5em 0.4em blue; 
</div>

<hr>

<div class='shadow' style="box-shadow: 0px 0px 0px 0.5em;">0 0 0 0.5em;</div>

<div class='shadow' style="box-shadow: 0px 0px 1em;">0 0 1em;</div>

<div class='shadow' style="box-shadow: 1em 0.5em;">1em 0.5em;</div>

<div class='shadow' style="box-shadow: 1em 0.5em 1em;">1em 0.5em 1em;</div>

<hr>

<div class='shadow' style="box-shadow: lightgreen 0.3em 0.3em;">0.3em 0.3em lightgreen;</div>

<div class='shadow' style="box-shadow: lightgreen 0.3em 0.3em 0px 0.6em;">0.3em 0.3em 0 0.6em lightgreen;</div>

<div class='shadow' style="box-shadow: lightgreen 0px 1.5em 0px -0.7em;">0 1.5em 0 -0.7em lightgreen;</div>

<div class='shadow' style="box-shadow: maroon 0px 0px 1em 0.5em;">0 0 1em 0.5em maroon;</div>

<hr>

<div class='shadow' style="box-shadow: red 0.2em 0.4em inset, red -1em -0.7em inset;">inset 0.2em 0.4em red, inset -1em -0.7em red;</div>

<div class='shadow' style="box-shadow: red 11em 0px inset;">inset 11em 0 red;</div>

<div class='shadow' style="box-shadow:red -11em 0px inset;">inset -11em 0 red;</div>

<div class='shadow' style="box-shadow: orange 13em 0px 3em -3em inset, blue -3em 0px 3em -3em inset;">
inset 13em 0 3em -3em orange,
inset -3em 0 3em -3em blue;
</div>

<div class='shadow' style="box-shadow: orange 11em 0px 2em inset;">inset 11em 0 2em orange;</div>

<div class='shadow' style="box-shadow: blue 1em 1em 2em -1em inset, red -1em -1em 2em -1em inset;">
inset 1em 1em 2em -1em blue,
inset -1em -1em 2em -1em red;
</div>

<hr>



## 参考

[Box Shadow](https://developer.mozilla.org/en-US/docs/Web/CSS/box-shadow)

[Box Shadow CSS Generator](https://cssgenerator.org/box-shadow-css-generator.html)

[Box Shadow Inset](http://elektronotdienst-nuernberg.de/bugs/box-shadow_inset.html)