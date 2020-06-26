---
layout: post
title: 长列表滚动的优化
subtitle: 
date: 2020-06-08
author: huangqing
header-img: img/post-bg-javascript.jpg
catalog: true
categories: [JavaScript]
tags:
  - HTML
  - javascript
---

有10万条或者更多的数据需要展示在页面中，应该怎样处理[页面中长列表滚动的优化](https://www.xiabingbao.com/post/scroll/longlist-optimization.html)

1. 懒加载：即监听`scroll`事件或使用`IntersecionObserver`监听
2. 可视区域的渲染：仅在可视区域展示数据，为保证滚动条的完整性，非可视区域使用占位元素的高度后者容器的位移来撑开。


## 懒加载

懒加载的方式有两种：
1. 监听`scroll`事件
2. 使用`IntersecionObserver`监听某个元素。

### scroll

当滚动条滑动到页面最底部或者将要滑动到最底部的时候，去加载下一页的数据。同时图片懒加载或者其他组件的懒加载也可以依赖于滚动事件.

优化滚动事件:
1. 滚动事件添加上防抖和节流
2. 使用`requestAnimationFrame`与`requestIdleCallback`代替定时器.前者适合流畅的动画效果场景，后者适用于分离一些优先级低的操作逻辑

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>滚动事件监听加载</title>
</head>
<style>
    #container {
        height: 200px;
        overflow: auto;
    }
</style>

<body>
    <div id='container'></div>
</body>
<script>
    let start = 1,
        count = 20,
        container = document.querySelector('#container');

    function createDom(start, count) {
        let div = document.createDocumentFragment();
        for (let i = start, len = start + count; i < len; i++) {
            let item = document.createElement('div');
            item.className = 'item';
            let html = '<p>' + i + '</p>';
            item.innerHTML = html;
            div.appendChild(item);
        }

        container.appendChild(div);
    }

    function handleScroller() {
        const maxScrollTop = container.scrollHeight - container.clientHeight;
        const currentScrollTop = container.scrollTop;

        if (maxScrollTop - currentScrollTop < 200) {
            start += count;
            createDom(start, count);
        }
    }

    container.addEventListener('scroll', handleScroller);

    createDom(start, count);

</script>

</html>
```

### [IntersecionObserver](http://www.ruanyifeng.com/blog/2016/11/intersectionobserver_api.html)

使用`IntersecionObserver`也可以实现无限滚动，比如在底部监听一个透明的元素，当该元素可见时就加载新的资源.`IntersecionObserver`的支持程度还不太好，需要polyfill方案。

`IntersecionObserver`在图片懒加载和组件懒加载的过程中，非常有用。

可见（visible）的本质是，目标元素与视口产生一个交叉区，所以这个 API 叫做"交叉观察器"。

```js
var io = new IntersectionObserver(callback, option);

// 开始观察
io.observe(document.getElementById('example'));

// 停止观察
io.unobserve(element);

// 关闭观察器
io.disconnect();
```

## 模拟滚动

模拟滚动最代表的例子就是`iScroll`，监听手势的`touchmove`事件，然后使用CSS3中的`transform`产生位移。

当页面的数据是无限加载时，应当使用`iscroll-infinite`这个类

```js
function updateData(start, count) {
    var div = document.createDocumentFragment();
    for(var i=start, len=start+count; i<len; i++) {
        var item = document.createElement('div');
        item.className = 'item';

        item.innerHTML = '<p>'+i+'</p><p><img src="http://img.ylq.com/2016/1114/20161114041350839.png?v='+i+'"></p>';
        div.appendChild(item);
    }
    document.querySelector('.content').appendChild(div);
}

var myScroller = new IScroll(document.querySelector('.wrapper'), {
    mouseWheel: true,
    probeType: 2
});

var start = 0;
var count = 20;
updateData(start, count);
myScroller.on('scroll', function() {
    if (this.y - this.maxScrollY < 120) {
        start += count;
        updateData(start, count);
        myScroller.refresh();
    }
})

document.addEventListener('touchmove', function (e) {
    e.preventDefault();
}, {
	capture: false,
	passive: false
});
</script>
```

[自定义滚动条](https://www.xiabingbao.com/scrollbar/2016/01/25/scrollbar.html)

## 可视区域数据的渲染

保持DOM元素在一个固定的范围，不可以一直无限追加。滚动到可视区域外部的元素，可以使用占位元素或`transform`撑开上面不可见的高度，让滚动条可以正常上下滑动即可。

撑开上半部分不可见区域的高度，有两种方法：
1. 在容器内的最顶部设置一个占位元素，这个占位元素的高度就是消失的所有DOM的高度
2. 给容器或者占位元素一个`transform`的位移,给占位元素一个高度撑起顶部的滚动区域。

### 固定高度的item

每次滚动时，都要计算应当渲染列表的那部分区域。`start`表示列表开始的位置，`fixedScrollTop`表示顶部占位元素的距离。

```js
// 滚动处理函数
function handleScroller() {
    let lastStart = 0; // 上次开始的位置
    const item = document.querySelector('.container .item');
    const itemStyle = getComputedStyle(item);
    //item的高度
    const itemHeight = item.offsetHeight + parseInt(itemStyle['marginTop']) + parseInt(itemStyle['marginBottom']);

    return function() {
        const currentScrollTop = Math.max(document.documentElement.scrollTop, document.body.scrollTop);
        //计算顶部高度，转换为item高度的整数倍
        const fixedScrollTop = currentScrollTop - currentScrollTop % itemHeight;
        let start = Math.floor( currentScrollTop/itemHeight ); // 可视区域开始渲染的位置

        if (lastStart!==start) {
            lastStart = start;
            createDom(start, count, fixedScrollTop);
        }
    }
}
```

设置顶部滚动的高度：

```js
function createDom(start, count, height) {
    const container = document.querySelector('.container');
    // 使用translateY产生位移模拟顶部高度
    // container.style.transform = `translateY(${height}px)`;

    let div = document.createDocumentFragment();
    
    // 创建占位元素
    if (height) {
        let p = document.createElement('p');
        p.style.height = height + 'px';
        div.appendChild(p);
    }

    for(let i=start, len=start+count; i<len; i++) {
        let item = document.createElement('div');
        item.className = 'item';
        item.innerHTML = i;
        div.appendChild(item);
    }

    // 为了方便处理，我们这里采用了更新container中的全部元素
    // 你也可以尝试只增加/删除首位的元素，中间元素不变
    container.innerHTML = '';
    container.appendChild(div);
}
```

### 每个item的高度都不一定


### DOM的复用

频繁的改动DOM，这样会频繁的引起页面的重绘，这里我们的思想是对页面中DOM元素进行复用。从上面滑出的元素，可以直接定位下面再重新装填元素，反之亦然！

```js
// 更新页面中DOM元素的位置
function updateDom(start, count, itemHeight, height) {
    document.querySelector('.container .content').style.transform = 'translateY('+height+'px)';
    for (var i = start, len=start+count; i < len; i++) {
        var index = i % count;
        var cssIndex = (i-start) % len;
        document.querySelector('.item' + index).innerHTML = i;
        document.querySelector('.item' + index).style.transform = 'translateY('+itemHeight*cssIndex+'px)';
    }
}
```

## 数据加载

1. 首先，控件提供数据关键id加载接口，获取全部数据的id数组数据，缓存到客户端。
2. 提供列表头获取接口，获取列表头信息。
3. 客户端获取到了全部数据的id信息，以此数据为基准，提供数据请求的接口，分页请求数据时，传入对应的id数组请求分页数据。
4. 在底层控件配置中新增id配置项，`{name：‘xx’,guid:function(){} }`。指定id列，指定生成新行数据的id生成方法。
5. 新增行时提供，自动调用（4）中生成guid的方法，生成数据id，并插入到客户端缓存和界面中。
6. 编辑新插入的行，提供编辑后保存接口，在编辑完成每个单元格后，进行数据服务端的保存。
7. 删除行时，删除缓存数据和界面对应行，并提供删除接口，进行数据服务端的删除。



