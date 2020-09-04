---
layout:     post
title:      Server Worker
subtitle:   
date:       2020-09-03
author:     huangqing
header-img: img/post-bg-html.jpg
catalog: true
categories: [HTML]
tags:
    - HTML
---


## Service Worker 是什么？

service worker 是独立于当前页面的一段运行在浏览器后台进程里的脚本。

service worker不需要用户打开 web 页面，也不需要其他交互，异步地运行在一个完全独立的上下文环境，不会对主线程造成阻塞。基于service worker可以实现消息推送，静默更新以及地理围栏等服务。

service worker提供一种渐进增强的特性，使用特性检测来渐渐增强，不会在老旧的不支持 service workers 的浏览器中产生影响。可以通过service workers解决让应用程序能够离线工作，让存储数据在离线时使用的问题。

注意事项：

1. service worker运行在它们自己的完全独立异步的全局上下文中，也就是说它们有自己的容器。

2. service worker没有直接操作DOM的权限，但是可以通过`postMessage`方法来与Web页面通信，让页面操作DOM。

3. service worker是一个可编程的`网络代理`，允许开发者控制页面上处理的网络请求。

4. 浏览器可能随时回收service worker，在不被使用的时候，它会自己终止，而当它再次被用到的时候，会被重新激活。

5. service worker的生命周期是由事件驱动的而不是通过Client。

## Service Worker支持使用

![](/images/html5/can-use-service-worker.png)

![](/images/html5/sw-important.jpg)

+ 浏览器支持：Service Worker。
+ Service Worker 只能被使用在 https 或者本地的 localhost 环境下,通过service worker可以劫持连接，伪造和过滤响应，为了避免这些问题，只能在HTTPS的网页上注册service workers，防止加载service worker的时候不被坏人篡改。

## 生命周期

Service Worker遵循`下载`、`安装`、`激活`的生命周期，并提供了`fetch`等事件，允许我们对资源的获取实施干预。

Service Worker的生命周期：

![](/images/html5/sw-lifecycle.png)

1. 注册service worker，在网页上生效
2. 安装成功，激活 或者 安装失败（下次加载会尝试重新安装）
3. 激活后，在sw的作用域下作用所有的页面，首次控制sw不会生效，下次加载页面才会生效。
4. sw作用页面后，处理fetch（网络请求）和message（页面消息）事件 或者 被终止（节省内存）。

![](/images/html5/sw-lifecycle-2.png)

1. 初始化阶段: 页面加载、解析 Service Worker 的 js 文件
2. 安装阶段: 安装 Service Worker ，通常在安装中执行缓存
3. 工作阶段：监听各种事件，执行工作
4. 待机阶段：网页关闭，Service Worker 在浏览器处于"静默"的状态，有事件同送等重新开始，随后再次"静默"
5. 更新阶段：新的 sw 触发 install 事件，旧的 sw 出发 activate 事件

![](/images/html5/service-worker-lifecycle.jpg)

![](/images/html5/service-worker-events.jpg)

## Cache

Cache了提供控制缓存的能力：

+ `cache.add(<request|url>)`和`cache.addAll(<url_list>)`：抓取目标URL, 检索并把返回的response对象添加到给定的Cache对象
+ `cache.put<request>, <response>`：同时抓取一个请求及其响应，并将其添加到给定的cache
+ `cache.match(<request>)`：匹配缓存，返回空或`Response`对象
+ `cache.delete(<key>)`：删除缓存对象
+ `cache.keys`：列出所有键
+ Cache的键为`Request`对象，值为`Reponse`对象
+ 方法都会返回`Promise`对象

CacheStorage提供了缓存的管理：

+ `caches.open(<name>)`：创建一个名为`name`的缓存
+ `caches.keys()`：列出所有缓存的键
+ `caches.has(<name>)`：判断缓存是否存在
+ `caches.delete(<name>)`：删除缓存
+ 方法都会返回`Promise`对象

caches API在页面和SW中都可以使用

## 作用域与控制

SW 的默认作用域为基于当前文件 URL 的 `./`。意思就是如果你在`//example.com/foo/bar.js`里注册了一个 SW，那么它默认的作用域为 `//example.com/foo/`。

我们把页面，workers，shared workers 叫做clients。SW 只能对作用域内的clients有效。一旦一个client被“控制”了，那么它的请求都会经过这个 SW。我们可以通过查看`navigator.serviceWorker.controller`是否为 null 来查看一个client是否被 SW 控制

## 注册

通过`serviceWorker.register`注册Service Worker：

main.js
```js
if ('serviceWorker' in navigator && navigator.onLine) {
    navigator.serviceWorker
        .register('/sw.js')
        .then(function (registration) {
            // Registrantion was successful
            console.log('ServiceWorker registration successful width scope:', registration.scope);
            sentMessageToServiceWorker();
        })
        .catch(function (err) {
            // Registrantion failed
            console.log('ServiceWorker registrantion failed:', err);
        });
}
```

sw.js文件被放在这个域的根目录下，和网站同源。这个service work将会收到这个域下的所有`fetch`事件。

如果将service worker文件注册为`/example/sw.js`，那么，service worker只能收到`/example/`路径下的`fetch`事件（例如： `/example/page1/`, `/example/page2/`）。

## 安装和激活

+ `install`事件是SW触发的第一个事件，并且仅触发一次。
+ 添加install事件的监听器，并通过`event.waitUntil`确保在安装完成之前添加这些缓存。
+ `caches.open`创建缓存
+ SW在安装成功并激活之前，不会响应`fetch`或`push`等事件

ws.js
```js
var CACHE_NAME = 'service-worker-demo-site-cache-v2';
var assets = [
    '/',
    '/main.js',
    '/main.css',
    '/github1.png'
];

self.addEventListener('install', function (event) {

    event.waitUntil(
        caches.open(CACHE_NAME)
            .then((cache) => {
                console.log('opened cache');
                return cache.addAll(assets);
            })
    );

    sentMessageToClients();
});
```

## 更新 Service Worker

+ 触发更新的几种情况：
    + 第一次导航到作用域范围内页面的时候
    + 当在24小时内没有进行更新检测并且触发功能性时间如`push`或`sync`的时候
    + SW 的 `URL` 发生变化并调用`.register()`时
+ 当 SW 代码发生变化，SW 会做更新（还将包括引入的脚本）
+ 更新后的 SW 会和原始的 SW 共同存在，并运行它的`install`
+ 如果新的 SW 不是成功状态，比如 404，解析失败，执行中报错或者在 install 过程中被`reject`，它将会被废弃，之前版本的 SW 还是激活状态不变。
+ 一旦新 SW 安装成功，它会进入`wait`状态直到原始 SW 不控制任何 clients。
+ `self.skipWaiting()`可以阻止等待，让新 SW 安装成功后立即激活。


## 请求的响应

添加`fetch`事件的监听，并通过`event.respondWith`劫持响应：

![](/images/html5/sw-fetch.jpg)

ws.js
```js
self.addEventListener('fetch', function(event) {
    event.respondWith(
        caches.open(CACHE_NAME).then((cache) => {
            return cache.match(event.request).then((response) => {
                const fetchPromise = fetch(event.request).then((response) => {
                    cache.put(event.request, response.clone());
                    return response;
                });
                return response || fetchPromise;
            });
        })
    );
});
```

## 资源缓存策略

### 安装：缓存关键资源

在安装过程中，我们可以将运行Web应用所需的静态资源（html、js、css、图片...）加入缓存，如同之前在介绍安装和激活时所作的那样。
而有些资源，并非关键的依赖项，允许缓存失败：

ws.js
```js
self.addEventListener('install', function(event) {
  event.waitUntil(
    caches.open('offline-v1').then(function(cache) {
      cache.addAll(
        // 非依赖项
      );
      return cache.addAll(
        // 关键资源
      );
    })
  );
});
```

非依赖项即使缓存失败，应用在离线状态下依然可用。

### 激活：缓存版本管理\移除旧缓存

在`active`事件中，新版本的SW已经激活，旧版本的SW退出，此时我们可以通过`caches.keys`、`caches.delete`删除旧的缓存。

```js
self.addEventListener('activate', function (event) {
    // 声明缓存白名单，该名单内的缓存目录不会被生成
    var cacheWhitelist = [CACHE_NAME];
    event.waitUntil(
        // 传给 waitUntil() 的 promise 会阻塞其他的事件，直到它完成
        // 确保清理操作会在第一次 fetch 事件之前完成
        caches.keys().then(function (keyList) {
            return Promise.all(keyList.map((key) => {
                if (cacheWhitelist.indexOf(key) === -1) {
                    return caches.delete(key);
                }
            }));
        })
    );
});
```

### 预缓存资源

caches api在页面中同样可以使用，因此我们可以想象这样一个功能：我们在点击某本书的“加入书架”按钮时，下载并缓存这本书的前几章内容，以便用户之后在离线状态下也能阅读书籍的前几章：

```js
onAddBook( bookId ) {
  caches.open(`mysite-books-${bookId}`).then((cache) => {
    return fetch(`/book/${bookId}/chapters`).then((response) => {
      // 缓存前十章内容
      return response.json().data.slice(0, 10).map(chapter => chapter.url)
    }).then((urls) => {
      cache.addAll(urls);
    })
  })
}
```


## 网络响应与缓存策略

### 仅使用缓存

```js
event.respondWith(caches.match(event.request));
```

### 仅使用网络

```js
event.respondWith(fetch(event.request));
```

### 缓存优先

先尝试在缓存中寻找资源，若不存在，则请求网络资源，同时在网络资源返回时放入缓存：

```js
event.respondWith(
  caches.open(CACHE_NAME).then((cache) => {
    return cache.match(event.request).then((response) => {
      return response || fetch(event.request).then((response) => {
        cache.put(event.request, response.clone());
        return response;
      });
    });
  })
);
```

### 网络优先

```js
event.respondWith(
  fetch(event.request).catch(() => {
    return caches.match(event.request);
  })
);
```

### 缓存优先并更新

优先使用缓存，但同时仍然发起网络请求并更新资源：

```js
event.respondWith(
  caches.open(CACHE_NAME).then((cache) => {
    return cache.match(event.request).then((response) => {
      const fetchPromise = fetch(event.request).then((response) => {
        cache.put(event.request, response.clone());
        return response;
      });
      return response || fetchPromise;
    });
  })
);
```

### 自定义404/离线页面

自定义离线响应：

```js
event.respondWith(
    caches.match(event.request).then((response) => {
      return response || fetch(event.request);
    }).catch(() => caches.match('/offline.html'));
  );
```
需要注意的是，缓存空间在资源紧张时会被回收


## 信息通讯

### postMessage

#### 从页面到 Service Worker

main.js
```js
if ('serviceWorker' in window.navigator) {
  navigator.serviceWorker.register('./sw.js', { scope: './' })
    .then(function (reg) {
      console.log('success', reg);
      navigator.serviceWorker.controller && navigator.serviceWorker.controller.postMessage("this message is from page");
    });
}
```

为了保证 Service Worker 能够接收到信息，我们在它被注册完成之后再发送信息，和普通的 `window.postMessage` 的使用方法不同，为了向 Service Worker 发送信息，我们要在 ServiceWorker 实例上调用 `postMessage` 方法，这里我们使用到的是 `navigator.serviceWorker.controller` 。

sw.js

```js
this.addEventListener('message', function (event) {
  console.log(event.data); // this message is from page
});
```
在 service worker 文件中我们可以直接在 this 上绑定 message 事件，这样就能够接收到页面发来的信息了。

#### 从 Service Worker 到页面

从 Service Worker 发送信息到页面，不同于页面向 Service Worker 发送信息，我们需要在 `WindowClient` 实例上调用 `postMessage` 方法才能达到目的。而在页面的JS文件中，监听 `navigator.serviceWorker` 的 `message` 事件即可收到信息。

最简单的方法就是从页面发送过来的消息中获取 WindowClient 实例，使用的是 `event.source` ，不过这种方法只能向消息的来源页面发送信息。

ws.js
```js
this.addEventListener('message', function (event) {
  event.source.postMessage('this message is from sw.js, to page');
});
```

main.js
```js
navigator.serviceWorker.addEventListener('message', function (e) {
  console.log(e.data); // this message is from sw.js, to page
});
```

如果不想受到这个限制，则可以在 serivce worker 文件中使用 this.clients 来获取其他的页面，并发送消息。

```js
self.clients.matchAll().then(clients => {
    var time = (new Date()).getTime();
    clients.forEach(client => {
        client.postMessage('this message is from sw.js, to page all client ' + time);
    });
})
```

### 使用 Message Channel 来通信

顾名思义，通信管道，可以实现两端的通信。

基本用法：

```js
var channel = new MessageChannel();
var port1 = channel.port1;
var port2 = channel.port2;
port1.onmessage = function(event) {
    console.log("port1收到来自port2的数据：" + event.data);
}
port2.onmessage = function(event) {
    console.log("port2收到来自port1的数据：" + event.data);
}

port1.postMessage("发送给port2");
port2.postMessage("发送给port1");
```

web worker兄弟线程通信

```js
let worker1 = new Worker('./worker1.js');
let worker2 = new Worker('./worker2.js');
let ms = new MessageChannel();

// 把 port1 分配给 worker1
worker1.postMessage('main', [ms.port1]);
// 把 port2 分配给 worker2
worker2.postMessage('main', [ms.port2]);

//worker1.js
self.onmessage = function(e) {
    console.log('worker1', e.ports);
    if (e.data === 'main') {
        const port = e.ports[0];
        port.postMessage(`worker1: Hi! I'm worker1`);
        port.onmessage = function(ev){
            console.log('reveice: ',ev.data,ev.origin);
        }
    }       
}

//worker2.js
self.onmessage = function(e) {
    if (e.data === 'main') {
        const port = e.ports[0];
        port.onmessage = function(e) {
            console.log('receive: ', e.data);
            port.postMessage('worker2: ' + e.data);
        }
    }
}
```

## 后台同步(Background Sync)

基本只有Google自家的Chrome可用

![](/images/html5/sw-sync.png)

1. 首先，你需要在Service Worker中监听sync事件；
2. 然后，在浏览器中发起后台同步sync（图中第一步）；
3. 之后，会触发Service Worker的sync事件，在该监听的回调中进行操作，例如向后端发起请求（图中第二步）
4. 最后，可以在Service Worker中对服务端返回的数据进行处理。

由于Service Worker在用户关闭该网站后仍可以运行，因此该流程名为“后台同步”实在是非常贴切。

### client触发sync事件

main.js
```js
navigator.serviceWorker.ready.then(function (registration) {
    var tag = "sample_sync";
    document.getElementById('js-sync-btn').addEventListener('click', function () {
        registration.sync.register(tag).then(function () {
            console.log('后台同步已触发', tag);
        }).catch(function (err) {
            console.log('后台同步触发失败', err);
        });
    });
});
```

由于后台同步功能需要在Service Worker注册完成后触发，因此较好的一个方式是在`navigator.serviceWorker.ready`之后绑定相关操作。例如上面的代码中，我们在ready后绑定了按钮的点击事件。当按钮被点击后，会使用`registration.sync.register()`方法来触发Service Worker的`sync`事件。

register()方法可以注册一个后台同步事件，其中接收的参数tag用于作为这个后台同步的唯一标识。


### 在Service Worker中监听sync事件

为Service Worker添加sync事件的监听：

sw.js
```js
self.addEventListener('sync', function (e) {
    console.log(`service worker需要进行后台同步，tag: ${e.tag}`);
    var init = {
        method: 'GET'
    };
    if (e.tag === 'sample_sync') {
        var request = new Request(`sync?name=AlienZHOU`, init);
        e.waitUntil(
            fetch(request).then(function (response) {
                response.json().then(console.log.bind(console));
                return response;
            })
        );
    }
});
```
需要特别注意的是，fetch请求一定要放在e.waitUntil()内。因为我们要保证“后台同步”，将Promise对象放在e.waitUntil()内可以确保在用户离开我们的网站后，Service Worker会持续在后台运行，等待该请求完成。

### 在后台同步时获取所需的数据

使用postMessage:

1. client触发sync事件；
2. 在sync注册完成后，使用postMessage和Service Worker通信；
3. 在Service Worker的sync事件回调中等待message事件的消息；
5. 收到message事件的消息后，将其中的信息提交到服务端。

使用indexedDB:

client先将数据存在indexedDB，待Servcie Worker需要时再从indexedDB读取使用即可


## Push消息推送（国内无法使用）

![](/images/html5/sw-push.jpg)

### 接收

在 sw.js 的内部通过事件监听的方式执行对应的回调函数，接收外部的推送信息，只需要添加 `push` 事件监听即可

sw.js
```js
self.addEventListener('push', function(event) {
 const options = {
    body: 'Yay it works.',
    icon: 'images/icon.png',
    badge: 'images/badge.png'
 }
 self.registration.showNotification(title, options);
})

```

消息推送，配合 Web Server For Chrome 更方便

### Firebase 云信息传递 (FCM)

Firebase 云信息传递 (FCM) 是一种跨平台消息传递解决方案，可供您免费、可靠地传递消息。可以发送通知消息以再次吸引用户并留住他们。在即时通讯等使用情形中，一条消息可将最多 4KB 的有效负载传送至客户端应用。

浏览器的 Service Worker 的消息推送主要依赖 FCM，服务端消息推送传递到 FCM，然后再由 FCM 推送到客户端


## 开发调试

### Update on reload

![](/images/html5/update-on-reload.png)

这样把生命周期变得对开发友好了，每次跳转将会：
1. 重新获取 SW
2. 尽管字节一致，也会重新安装，也就是说install事件被执行并且更新缓存。
3. 跳过 waiting，激活新的 SW。
4. 导航到这个页面。

### Skip Waiting

![](/images/html5/chrome-skip-waiting.png)

如果你有个 SW 在等待状态，你可以点击 skipWaiting 让它立即变为激活状态。

### 强制刷新

如果你强制刷新页面，那么会绕过 SW，变成不受控，这个功能已被定为规范，所以在其他支持 SW的浏览器中也适用。

## 库与工具

+ `Workbox`：Workbox是谷歌推出的方便大家使用PWA/SW的库。
+ `workbox-webpack-plugin`： Workbox还提供了workbox-webpack-plugin方便在Webpack打包的项目中更方便的使用Workbox。
+ `register-service-worker`：使用register-service-worker来简化SW的注册和回调钩子：