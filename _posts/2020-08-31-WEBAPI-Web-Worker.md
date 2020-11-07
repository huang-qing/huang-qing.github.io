---
layout:     post
title:      Web Worker
subtitle:   
date:       2020-08-31
author:     huangqing
header-img: img/post-bg-html.jpg
catalog: true
categories: [WEB API]
tags:
    - WEB API
---

## 概述

Web Worker 的作用，就是为 JavaScript 创造多线程环境，允许主线程创建 Worker 线程，将一些任务分配给后者运行。在主线程运行的同时，Worker 线程在后台运行，两者互不干扰。等到 Worker 线程完成计算任务，再把结果返回给主线程。这样的好处是，一些计算密集型或高延迟的任务，被 Worker 线程负担了，主线程（通常负责 UI 交互）就会很流畅，不会被阻塞或拖慢。

Worker 线程一旦新建成功，就会始终运行，不会被主线程上的活动（比如用户点击按钮、提交表单）打断。这样有利于随时响应主线程的通信。但是，这也造成了 Worker 比较耗费资源，不应该过度使用，而且一旦使用完毕，就应该关闭。

Web Worker 有以下几个使用注意点。

（1）同源限制

分配给 Worker 线程运行的脚本文件，必须与主线程的脚本文件同源。

（2）DOM 限制

Worker 线程所在的全局对象，与主线程不一样，无法读取主线程所在网页的 DOM 对象，也无法使用document、window、parent这些对象。但是，Worker 线程可以navigator对象和location对象。

（3）通信联系

Worker 线程和主线程不在同一个上下文环境，它们不能直接通信，必须通过消息完成。

（4）脚本限制

Worker 线程不能执行alert()方法和confirm()方法，但可以使用 XMLHttpRequest 对象发出 AJAX 请求。

（5）文件限制

Worker 线程无法读取本地文件，即不能打开本机的文件系统（file://），它所加载的脚本，必须来自网络。

## API

### 主线程

浏览器原生提供`Worker()`构造函数，用来供主线程生成 Worker 线程。

```js
var worker = new Worker(jsUrl, options);

var worker = new Worker('worker.js', { name : 'worker' });
```

Worker()构造函数返回一个 Worker 线程对象，用来供主线程操作 Worker。Worker 线程对象的属性和方法如下:

+ Worker.onerror：指定 error 事件的监听函数。
+ Worker.onmessage：指定 message 事件的监听函数，发送过来的数据在Event.data属性中。
+ Worker.onmessageerror：指定 messageerror 事件的监听函数。发送的数据无法序列化成字符串时，会触发这个事件。
+ Worker.postMessage()：向 Worker 线程发送消息。
+ Worker.terminate()：立即终止 Worker 线程。

### Worker 线程

Web Worker 有自己的全局对象，不是主线程的window，而是一个专门为 Worker 定制的全局对象。因此定义在window上面的对象和方法不是全部都可以使用。

Worker 线程有一些自己的全局属性和方法。

+ self.name： Worker 的名字。该属性只读，由构造函数指定。
+ self.onmessage：指定message事件的监听函数。
+ self.onmessageerror：指定 messageerror 事件的监听函数。发送的数据无法序列化成字符串时，会触发这个事件。
+ self.close()：关闭 Worker 线程。
+ self.postMessage()：向产生这个 Worker 线程发送消息。
+ self.importScripts()：加载 JS 脚本。


## Worker 线程完成轮询

html

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>web worker</title>
</head>

<body>
    <div id='text'>web worker init</div>
</body>

<script src="worker-main.js"></script>

</html>
```

main: /worker-main.js

```js
//主线程
var worker = new Worker("worker.js");

worker.postMessage({
    msg: 'hello world from worker main'
});

worker.onmessage = function (message) {
    var data = message.data;
    document.querySelector("#text").innerHTML = data.msg;
}

worker.onerror = function (error) {
    console.log(
        error.filename,
        error.lineno,
        error.message
    );
}

```

worker.js /worker.js

```js
//worker线程 执行轮询
var i = 1;

self.onmessage = function (message) {
    var data = message.data,
        i = 1;

    console.log(data);
    setInterval(function () {
        sentMsg();
    }, 1000);

}

async function sentMsg() {
    var response,
        data;

    response = await fetch('./data.json');
    data = await response.json();
    data.msg = data.msg + " count:" + (++i);
    console.log(data);
    postMessage(data);
}

```

data.json /data.json

```json
{
    "msg": "Hi from data.json"
}

```