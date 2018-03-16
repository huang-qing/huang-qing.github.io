---
layout:     post
title:      高频 dom 操作和页面性能优化探索.👿
subtitle:   
date:       2017-08-09
author:     huangqing
header-img: img/post-bg-html.jpg
catalog: true
categories: [HTML]
tags:
    - DOM
---

## 一、DOM操作影响页面性能的核心问题

通过js操作DOM的代价很高，影响页面性能的主要问题有如下几点：

+ 访问和修改DOM元素
+ 修改DOM元素的样式，导致重绘或重排
+ 通过对DOM元素的事件处理，完成与用户的交互功能

>DOM的修改会导致重绘和重排。
>
>重绘是指一些样式的修改，元素的位置和大小都没有改变；
>重排是指元素的位置或尺寸发生了变化，浏览器需要重新计算渲染树，而新的渲染树建立后，浏览器会重新绘制受影响的元>素。
>

页面重绘的速度要比页面重排的速度快，在页面交互中要尽量避免页面的重排操作。浏览器不会在js执行的时候更新DOM，而是会把这些DOM操作存放在一个队列中，在js执行完之后按顺序一次性执行完毕，因此在js执行过程中用户一直在被阻塞。

#### 1.页面渲染过程

一个页面更新时，渲染过程大致如下：

![](/images/dom/1b882c131d28eb11a0cbc9d5f0173720.jpg)

+ JavaScript: 通过js来制作动画效果或操作DOM实现交互效果
+ Style: 计算样式，如果元素的样式有改变，在这一步重新计算样式，并匹配到对应的DOM上
+ Layout: 根据上一步的DOM样式规则，重新进行布局（重排）
+ Paint: 在多个渲染层上，对新的布局重新绘制（重绘）
+ Composite: 将绘制好的多个渲染层合并，显示到屏幕上

在网页生成的时候，至少会进行一次布局和渲染，在后面用户的操作时，不断的进行重绘或重排，因此如果在js中存在很多DOM操作，就会不断地出发重绘或重排，影响页面性能。

#### 2.DOM操作对页面性能的影响

如前面所说，DOM操作影响页面性能的核心问题主要在于DOM操作导致了页面的重绘或重排，为了减少由于重绘和重排对网页性能的影响，我们要知道都有哪些操作会导致页面的重绘或者重排。

2.1 导致页面重排的一些操作：

+ 内容改变
    + 文本改变或图片尺寸改变

+ DOM元素的几何属性的变化
    + 例如改变DOM元素的宽高值时，原渲染树中的相关节点会失效，浏览器会根据变化后的DOM重新排建渲染树中的相关节点。如果父节点的几何属性变化时，还会使其子节点及后续兄弟节点重新计算位置等，造成一系列的重排。

+ DOM树的结构变化
    + 添加DOM节点、修改DOM节点位置及删除某个节点都是对DOM树的更改，会造成页面的重排。浏览器布局是从上到下的过程，修改当前元素不会对其前边已经遍历过的元素造成影响，但是如果在所有的节点前添加一个新的元素，则后续的所有元素都要进行重排。

+ 获取某些属性
    + 除了渲染树的直接变化，当`获取一些属性值时，浏览器为取得正确的值也会发生重排`，这些属性包括：`offsetTop`、`offsetLeft`、 `offsetWidth`、`offsetHeight`、`scrollTop`、`scrollLeft`、`scrollWidth`、`scrollHeight`、` clientTop`、`clientLeft`、`clientWidth`、`clientHeight`、`getComputedStyle()`。

+ 浏览器窗口尺寸改变
    + 窗口尺寸的改变会影响整个网页内元素的尺寸的改变，即DOM元素的集合属性变化，因此会造成重排。

2.2 导致页面重绘的操作

+ 应用新的样式或者修改任何影响元素外观的属性
    + 只改变了元素的样式，并未改变元素大小、位置，此时只涉及到重绘操作。

+ 重排一定会导致重绘
    + 一个元素的重排一定会影响到渲染树的变化，因此也一定会涉及到页面的重绘。

## 二、高频操作DOM会导致的问题

接下来会分享一下在平时项目中由于高频操作DOM影响网页性能的问题。

`1.1 存在的问题`

抽奖项目中，遇到了这样的由于高频操作DOM，导致页面性能变差的问题。在经历几轮抽奖后，文字滚动速度越来越慢，肉眼能感受到与第一次抽奖时文字滚动速度的明显差别，如持续时间过长或轮次过多，还会造成浏览器假死现象。

[实现demo:](https://gxt19940130.github.io/demo/dom.html)

`1.2 问题分析`

下图为抽奖时文字滚动过程中的timeline记录。

![](/images/dom/b9924f828168e8f9ae67ae665c6eb620.png)

timeline分析：

1. `FPS`:最上面一栏为绿色柱形为帧率(FPS)，顶点值为60fps，上方红色方块表示长帧，这些长帧被Chrome称为jank(卡顿)。

2. `CPU`:第二栏为CPU，蓝色表示loading（网络通信和HTML解析），黄色表示scripting（js执行时间），紫色表示rendering（样式计算和布局，即重排）， 绿色为painting（即重绘）。


更多timeline使用方法可参考：如何使用Chrome Timeline 工具（译）

由上图可以看出，在文字滚动过程中红色方块出现频繁，页面中存在的卡顿过多。帧率的值越低，人眼感受到的效果越差。

接下来选择一段长帧区域放大来看

![](/images/dom/f16ecdf14067cd5e49ba71dd2435d8ff.png)

在这段区域内最大一帧达到了49.7ms，帧率只有20fps，接下来看看这一帧里是什么因素耗时过长

![](/images/dom/a6828cda64e64788f446a344bb5a0762.png)

由上图可以看出，耗时最大的在scripting，js的执行时间达到了44.9ms，占总时间的93.2%，因为主要靠js计算控制DOM的显示内容，所以js运行时间过长。

选取一段FPS值很低的部分查看造成这段值低的原因

![](/images/dom/cb7733afac5fbce131637473bbaa405f.png)

由下图可看出主要为dom.html中的js执行占用时间。

![](/images/dom/4bc93fab71242e6378898245d294fcca.png)

点进dom.html文件，即可定位到该函数

![](/images/dom/2d24c4d01143b8025d6156e2805199bb.png)

由此可知，主要是`rolling`这个函数执行时间过长，对该部分失帧影响较大。而这个函数的主要作用就是实现文字的滚动效果，也可以从代码中看出，这个函数利用的setTimeout来反复执行，并且在这个函数中存在着循环以及大量的DOM操作，造成了页面的失帧等问题。

`1.3 优化方案`

针对该项目中的问题，采取的解决方法是：

一次性生成全部`<li>`，并且隐藏这些`<li>`，随机生成一组随机数数组，只有index与数组里面的随机数相等时，才显示该位置的`<li>`，虽然也会触发重排和重绘，但是性能要远远高于直接操作DOM的添加和删除。

用`requestAnimationFrame`取代`setTimeout`不断生成随机数。

requestAnimationFrame与setTimeout和setInterval类似，都是通过递归调用同一个方法不断更新页面。

 + `setTimeout()`：在特定的时间后执行函数，而且只执行一次，如果在特定时间前想取消执行函数，可以用clearTimeout立即取消执行。但是并不是每次执行setTimeout都会在特定的时间后执行，页面加载后js会按照主线程中的顺序按序执行那个，如果在延迟时间内主线程不空闲，setTimeout里面的函数是不会执行的，它会延迟到主线程空闲时才执行。

+ `setInterval()`：在特定的时间间隔内重复执行函数，除非主动清除它，不然会一直执行下去，清除函数可以使用clearInterval。setInterval也会等到主线程空闲了再执行，但是setInterval去排队时，如果发现自己还在队列中未执行，就会被drop掉，所以可能会造成某段时间的函数未被执行。

+ `requestAnimationFrame()`：它不需要设置时间间隔，它会在浏览器每次刷新之前执行回调函数的任务。这样我们动画的更新就能和浏览器的刷新频率保持一致。requestAnimationFrame在运行时，浏览器会自动优化方法的调用，并且如果页面不是激活状态下的话，动画会自动暂停，有效节省了CPU开销。
在采用上面的方法进行优化后，在经历多轮抽奖后，文字滚动速度依旧正常，网页性能良好，不会出现文字滚动速度越来越慢，最后导致浏览器假死的现象。

[实现demo:](https://gxt19940130.github.io/demo/demo_gxt/dom_by_vue.html)

`1.4 优化前后FPS对比`

优化前文字滚动时的timeline

![](/images/dom/3217ffb2474300c6fd3c025210074128.png)

优化后文字滚动时的timeline

![](/images/dom/b31a72c274182c6f2a85233c412b5ef7.png)

优化前的代码对DOM操作很频繁，因此FPS值普遍偏低，而优化后可以看出红色方块明显减少，FPS值一直处于高值。

`1.5 优化前后CPU占用对比`

优化前文字滚动时的timeline

![](/images/dom/b9924f828168e8f9ae67ae665c6eb620.png)

优化后文字滚动时的timeline

![](/images/dom/fed57e02c481d1355352dbda835f0e64.png)

优化前js的CPU占用率较高，而优化后占用CPU的主要为渲染时间，因为优化后的代码只是控制了节点的显示和隐藏，所以在js上消耗较少，在渲染上消耗较大。

## 三、针对操作DOM的性能优化方法总结

为了减少DOM操作对页面性能产生的影响，在实现页面的交互效果时一定要注意一下几点：

`1.减少在循环内进行DOM操作，在循环外部进行DOM缓存`

```javascript
//优化前代码
function Loop() {
   console.time("loop1");
   for (var count = 0; count < 15000; count++) {
       document.getElementById('text').innerHTML += 'dom';
   }
   console.timeEnd("loop1");
}
```

```javascript
//优化后代码
function Loop2() {
    console.time("loop2");
    var content = '';
    for (var count = 0; count < 15000; count++) {
        content += 'dom';
    }
    document.getElementById('text2').innerHTML += content;
    console.timeEnd("loop2");
}
```

两个函数的执行时间对比：

![](/images/dom/65fd413b5fb38de9bfe9dbb85bfde402.png)

优化前的代码中，每进行一次循环，都会读取一次div的innerHtml属性，并且对这个属性进行了重新赋值，即每循环一次就会操作两次DOM，因此执行时间很长，页面性能差。

在优化后的代码中，将要更新的DOM内容进行缓存，在循环时只操作字符串，循环结束后字符串的值写入到div中，只进行了一次查找innerHtml属性和一次对该属性重新赋值的操作，因此同样的循环次数先，优化后的方法执行时间远远少于优化前。

`2.只控制DOM节点的显示或隐藏，而不是直接去改变DOM结构`

在抽奖项目中频繁操作DOM来控制文字滚动的方法（[demo:](https://gxt19940130.github.io/demo/dom.html) 导致页面性能很差，最后修改为如下代码。

```html
<div class="staff-list" :class="list">
   <ul class="staff-list-ul">
       <li v-for="item in staffList" v-show="isShow($index)">
           <div>{{{item.staff_name | addSpace}}} </div>
           <div class="staff_phone">{{item.phone_no}} </div>
       </li>
   </ul>
</div>
```

上面代码的优化原理即先生成所有DOM节点，但是所有节点均不显示出来，利用vue.js中的v-show，根据计算的随机数来控制显示某个`<li>`，来达到文字滚动效果。

如果采用jquery，则需要将生成的所有`<li>`全部存放在`<ul>`下，并且隐藏它们，在根据生成的随机数组，利用jquery查找`index`与生成的随机数对应的`<li>`并显示，达到文字滚动效果。

优化后[demo: ](https://gxt19940130.github.io/demo/demo_gxt/dom_by_vue.html)


`3.操作DOM前，先把DOM节点删除或隐藏`

```javascript
var list1 = $(".list1");
list1.hide();
for (var i = 0; i < 15000; i++) {
    var item = document.createElement("li");
    item.append(document.createTextNode('0'));
    list1.append(item);
}
list1.show();
```

display属性值为none的元素不在渲染树中，因此对隐藏的元素操作不会引发其他元素的重排。如果要对一个元素进行多次DOM操作，可以先将其隐藏，操作完成后再显示。这样只在隐藏和显示时触发2次重排，而不会是在每次进行操作时都出发一次重排。

页面rendering时间对比：

下图为同样的循环次数下未隐藏节点直接进行DOM操作的rendering时间（图一）和隐藏节点再进行DOM操作的rendering时间（图二）

![](/images/dom/2d4e94b4978467f1cab5c7b0f9640340.png)

![](/images/dom/c904ad494ced5a0e1660ee95a0c7d5d3.png)


由对比图可以看出，总时间、js执行时间以及rendering时间都明显减少，并且避免了painting以及其他的一些操作。

`4. 最小化重绘和重排`

```javascript
//优化前代码
var element = document.getElementById('mydiv');
element.style.height = "100px";  
element.style.borderLeft = "1px";  
element.style.padding = "20px";
```

在上面的代码中，每对element进行一次样式更改都会影响该元素的集合结构，最糟糕情况下会触发三次重排。

优化方式：利用js或jquery对该元素的class重新赋值，获得新的样式，这样减少了多次的DOM操作。

```javascript
//优化后代码
//js操作
.newStyle {  
    height: 100px;  
    border-left: 1px;  
    padding: 20px;  
}  
element.className = "newStyle";
//jquery操作
$(element).css({
    height: 100px;  
    border-left: 1px;  
    padding: 20px;
})
```


参考文章：

[高性能JS-DOM](http://mp.weixin.qq.com/s/Khr9u1cacbBgWHvveL7DPA)

[Effective前端6：避免页面卡顿](https://zhuanlan.zhihu.com/p/25166666?refer=dreawer)

[高性能滚动 scroll 及页面渲染优化](http://web.jobbole.com/86158/)

[脑洞大开：为啥帧率达到 60 fps 就流畅？](http://www.jianshu.com/p/71cba1711de0)

[如何使用Chrome Timeline 工具（译）](http://www.jianshu.com/p/4da0f0bda768)