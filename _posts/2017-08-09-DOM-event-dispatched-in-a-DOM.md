---
layout:     post
title:      监听页面 DOM 变动并高效响应.👿
subtitle:   
date:       2017-08-09
author:     huangqing
header-img: img/post-bg-html.jpg
catalog: true
categories: [HTML]
tags:
    - DOM
---

# 监听页面 DOM 变动并高效响应

## JavaScript 中事件的发生阶段（捕获-命中-冒泡）:


![Graphical representation of an event dispatched in a DOM tree using the DOM event flow](/images/dom/eventflow.svg)

[Graphical representation of an event dispatched in a DOM tree using the DOM event flow](https://www.w3.org/TR/DOM-Level-3-Events/#dom-event-architecture)

开始的时候我一直在 window 状态改变涉及到的事件中寻找，一圈搜寻下来发现也就 onload 事件最接近了，所以我们看看 MDN 对该事件的定义：

>The load event is fired when a resource and its dependent resources have finished loading.

怎么理解资源及其依赖资源已加载完毕呢？简单来说，如果一个页面涉及到图片资源，那么 `onload` 事件会在页面完全载入（包括图片、css文件等等）后触发。一个简单的监听事件用 JavaScript 应该这样书写（注意不同环境下 load 和 onload 的差异）：


```javascript
  window.addEventListener("load", function(event) {
    console.log("All resources finished loading!");
  });
  
  // or
  window.onload=function(){
    console.log("All resources finished loading!");
  };
  
  // jQuery
  $( window ).on( "load", handler )
```

```HTML
 < body onload="SomeJavaScriptCode">
```

说到 `onload` 事件，有一个 jQuery 中相似的事件一定会被提及—— `ready` 事件。jQuery 中这样定义这个事件：

>Specify a function to execute when the DOM is fully loaded.

需要知道的是 jQuery 定义的 ready 事件实质上是为 `DOMContentLoaded` 事件设计的，所以当我们谈论加载时应该区分的事件其实是 onload（*接口 UIEvent*） 以及 DOMContentLoaded（*接口 Event*），MDN 这样描述 DOMContentLoaded：

>当初始HTML文档被完全加载和解析时，DOMContentLoaded 事件被触发，而无需等待样式表、图像和子框架完成加载。另一个不同的事件 load 应该仅用于检测一个完全加载的页面。

所以可以知道，当一个页面加载时应先触发 DOMContentLoaded 然后才是 onload. 类似的事件及区别包括以下几类：

+ DOMContentLoaded: 当初始HTML文档被完全加载和解析时，DOMContentLoaded 事件被触发，而无需等待样式表、图像和子框架完成加载；
+ readystatechange: 一个document 的 Document.readyState 属性描述了文档的加载状态，当这个状态发生了变化，就会触发该事件；
+ load: 当一个资源及其依赖资源已完成加载时，将触发load事件；
+ beforeunload: 当浏览器窗口，文档或其资源将要卸载时，会触发beforeunload事件。
+ unload: 当文档或一个子资源正在被卸载时, 触发 unload事件。

细心点会发现上面在介绍事件时提到了 `UIEvent` 以及 `Event`，这是什么呢？这些都是事件——可以被 JavaScript 侦测到的行为。其他的事件接口还包括 `KeyboardEvent` ／ `VRDisplayEvent` 等等:

## DOM3 级事件规定了以下几类事件 :

+ UI 事件
+ 焦点事件 
+ 鼠标事件
+ 滚轮事件
+ 文本事件
+ 键盘事件
+ 合成事件
+ 变动事件
+ 变动名称事件

其他：

+ HTML5 事件
+ 设备事件
+ 触摸与手势事件

## [Web API 接口](https://developer.mozilla.org/zh-CN/docs/Web/API)

监听页面中动态生成的元素,应该在变动事件中做出选择, DOM2 定义的如下变动事件：[事件类型一览表](https://developer.mozilla.org/zh-CN/docs/Web/Events)

+ DOMSubtreeModified: 在DOM结构发生任何变化的时候。这个事件在其他事件触发后都会触发；
+ DOMNodeInserted: 当一个节点作为子节点被插入到另一个节点中时触发；
+ DOMNodeRemoved: 在节点从其父节点中移除时触发；
+ DOMNodeInsertedIntoDocument: 在一个节点被直接插入文档或通过子树间接插入文档之后触发。这个事件在 DOMNodeInserted 之后触发；
+ DOMNodeRemovedFromDocument: 在一个节点被直接从文档移除或通过子树间接从文档移除之前触发。这个事件在 DOMNodeRemoved 之后触发；
+ DOMAttrModified: 在特性被修改之后触发；
+ DOMCharacterDataModified: 在文本节点的值发生变化时触发；

## [Mutation Observer API ](http://javascript.ruanyifeng.com/dom/mutationobserver.html):

Mutation Observer API 用来监视 DOM 变动。DOM 的任何变动，比如节点的增减、属性的变动、文本内容的变动，这个 API 都可以得到通知。

概念上，它很接近事件，可以理解为当 DOM 发生变动，就会触发 Mutation Observer 事件。但是，它与事件有一个本质不同：事件是同步触发，也就是说，DOM 的变动立刻会触发相应的事件；Mutation Observer 则是异步触发，DOM 的变动并不会马上触发，而是要等到当前所有 DOM 操作都结束才触发。

这样设计是为了应付 DOM 变动频繁的特点。举例来说，如果在文档中连续插入1000个<p>元素，就会连续触发1000个插入事件，执行每个事件的回调函数，这很可能造成浏览器的卡顿；而 Mutation Observer 完全不同，只在1000个段落都插入结束后才会触发，而且只触发一次。

Mutation Observer 有以下特点:

+ 它等待所有脚本任务完成后，才会运行，即采用异步方式。
+ 它把 DOM 变动记录封装成一个数组进行处理，而不是一条条地个别处理 DOM 变动。
+ 它既可以观察发生在 DOM 的所有类型变动，也可以观察某一类变动。

MutationObserver 的构造函数比较简单，传入一个回调函数即可（回调函数接受两个参数，第一个是变动数组，第二个是观察器实例）：

```javascript
let observer = new MutationObserver(callback);
```

## JavaScript 中的截流／节流函数:

在前端开发中，页面有时会绑定scroll或resize事件等频繁触发的事件，也就意味着在正常的操作之内，会多次调用绑定的程序，然而有些时候javascript需要处理的事情特别多，频繁出发就会导致性能下降、成页面卡顿甚至是浏览器奔溃。

形像的比喻是水龙头或机枪，你可以控制它的流量或频率。throttle 的关注点是连续的执行间隔时间。

函数节流的原理也挺简单，一样还是定时器。当我触发一个时间时，先setTimout让这个事件延迟一会再执行，如果在这个时间间隔内又触发了事件，那我们就清除原来的定时器，再setTimeout一个新的定时器延迟一会执行。函数节流的出发点，就是让一个函数不要执行得太频繁，减少一些过快的调用来节流。

