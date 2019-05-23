---
layout:     post
title:      防抖动（Debouncing）和节流阀（Throttling）
subtitle:   
date:       2017-07-07
author:     huangqing
header-img: img/post-bg-javascript.jpg
catalog: true
categories: [JavaScript]
tags:
    - RxJS
---

# 防抖动（Debouncing）和节流阀（Throttling）

防抖（Debounce）和节流（throttle）都是用来控制某个函数在一定时间内执行多少次的技巧，两者相似而又不同。

当我们给 DOM 绑定事件的时候，加了防抖和节流的函数变得特别有用。为什么呢？因为我们在事件和函数执行之间加了一个控制层。记住，我们是无法控制 DOM 事件触发频率的

## 防抖动（Debounce）

`debounce`和`throttle`很像，`debounce`是空闲时间必须大于或等于 一定值的时候，才会执行调用方法。

防抖技术可以把多个顺序地调用合并成一次。

![debounce](/images/javascript/debounce.png)

前缘（或者“immediate”）:

![前缘 debounce 的例子](/images/javascript/debounce-leading.png)

debounce是空闲时间的间隔控制。比如我们做autocomplete，这时需要我们很好的控制输入文字时调用方法时间间隔。一般时第一个输入的字符马上开始调用，根据一定的时间间隔重复调用执行的方法。对于变态的输入，比如按住某一个建不放的时候特别有用。

debounce主要应用的场景比如：

+ 文本输入keydown 事件，keyup 事件，例如做autocomplete


## Throttle（节流阀）

`throttle`就是函数节流的意思。再说的通俗一点就是函数调用的频度控制器，是连续执行时间间隔控制。

使用 `throttle` 的时候，只允许一个函数在 X 毫秒内执行一次。跟 `debounce` 主要的不同在于，throttle 保证 X 毫秒内至少执行一次。

主要应用的场景比如：

+ 鼠标移动，mousemove 事件
+ DOM 元素动态定位
+ window对象的resize和scroll(无限滚动) 事件

有人形象的把上面说的事件形象的比喻成机关枪的扫射，throttle就是机关枪的扳机，你不放扳机，它就一直扫射。我们开发时用的上面这些事件也是一样，你不松开鼠标，它的事件就一直触发。

回到window resize和scroll事件的基本优化提到的优化：

```javascript
var resizeTimer=null;
$(window).on('resize',function(){
       if(resizeTimer){
           clearTimeout(resizeTimer)
       }
       resizeTimer=setTimeout(function(){
           console.log("window resize");
       },400);
   }
);
```

`setTimeout`和`clearTimeout`其实就是一个简单的 `throttle`，很多好的控制了resize事件的调用频度




## 插件

throttle和debounce控制函数：

```javascript
/*
* 频率控制 返回函数连续调用时，fn 执行频率限定为每多少时间执行一次
* @param fn {function}  需要调用的函数
* @param delay  {number}    延迟时间，单位毫秒
* @param immediate  {bool} 给 immediate参数传递false 绑定的函数先执行，而不是delay后后执行。
* @return {function}实际调用函数
*/
var throttle = function (fn,delay, immediate, debounce) {
   var curr = +new Date(),//当前事件
       last_call = 0,
       last_exec = 0,
       timer = null,
       diff, //时间差
       context,//上下文
       args,
       exec = function () {
           last_exec = curr;
           fn.apply(context, args);
       };
   return function () {
       curr= +new Date();
       context = this,
       args = arguments,
       diff = curr - (debounce ? last_call : last_exec) - delay;
       clearTimeout(timer);
       if (debounce) {
           if (immediate) {
               timer = setTimeout(exec, delay);
           } else if (diff >= 0) {
               exec();
           }
       } else {
           if (diff >= 0) {
               exec();
           } else if (immediate) {
               timer = setTimeout(exec, -diff);
           }
       }
       last_call = curr;
   }
};
 
/*
* 空闲控制 返回函数连续调用时，空闲时间必须大于或等于 delay，fn 才会执行
* @param fn {function}  要调用的函数
* @param delay   {number}    空闲时间
* @param immediate  {bool} 给 immediate参数传递false 绑定的函数先执行，而不是delay后后执行。
* @return {function}实际调用函数
*/
 
var debounce = function (fn, delay, immediate) {
   return throttle(fn, delay, immediate, true);
};
```

+ [Underscore.js](http://www.bootcss.com/p/underscore/)就对throttle和debounce进行封装。

+ jQuery也有一个throttle和debounce的插件：[jQuery throttle / debounce](https://github.com/cowboy/jquery-throttle-debounce)。

## 结论:

debounce：把触发非常频繁的事件（比如按键）合并成一次执行。

throttle：保证每 X 毫秒恒定的执行次数，比如每200ms检查下滚动位置，并触发 CSS 动画。

## 参考

[javascript函数的throttle和debounce](http://www.css88.com/archives/4648)

[实例解析防抖动（Debouncing）和节流阀（Throttling）](http://www.css88.com/archives/tag/throttle)