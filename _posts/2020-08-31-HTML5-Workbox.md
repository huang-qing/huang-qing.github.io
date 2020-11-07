---
layout:     post
title:      Workbox
subtitle:   PWA
date:       2020-08-31
author:     huangqing
header-img: img/post-bg-html.jpg
catalog: true
categories: [HTML5]
tags:
    - HTML5
    - PWA
---


由于直接写原生的`sw.js`，比较繁琐和复杂，所以一些工具就出现了，而`workbox`是其中的佼佼者，由google团队推出

![workbox](/images/html5/workbox.jpg)

## 基本配置

首先，需要在项目的`sw.js`文件中，引入`workbox`的官方js，这里用了我们自己的静态资源：

```js
importScripts(
    "../workbox.js"
);
```

其中`importScripts`是`webworker`中加载js的方式。

引入workbox后，全局会挂载一个workbox对象

```js
if (workbox) {
    console.log('workbox加载成功');
} else {
    console.log('workbox加载失败');
}
```

然后需要在使用其他的api前，提前使用配置

```js
//关闭控制台中的输出
workbox.setConfig({ debug: false });
```

也可以统一指定存储时cache的名称：

```js
//设置缓存cachestorage的名称
workbox.core.setCacheNameDetails({
    prefix:'edu-cms',
    suffix:'v1'
});
```

## precache

workbox的缓存分为两种，一种的`precache`，一种的`runtimecache`。

`precache`对应的是在`installing`阶段进行读取缓存的操作。它让开发人员可以确定缓存文件的时间和长度，以及在不进入网络的情况下将其提供给浏览器，这意味着它可以用于创建Web离线工作的应用。

**工作原理**

首次加载Web应用程序时，workbox会下载指定的资源，并存储具体内容和相关修订的信息在indexedDB中。

当资源内容和sw.js更新后，workbox会去比对资源，然后将新的资源存入cache，并修改indexedDB中的版本信息。

这种方式适合于上线后就不会经常变动的静态资源。

```js
//precache 适用于支持跨域的cdn和域内静态资源
workbox.precaching.suppressWarnings();
workbox.precaching.precacheAndRoute([
    './main.css'
]);
```

## runtimecache

运行时缓存是在`install`之后，`activated`和`fetch`阶段做的事情。

既然在`fetch`阶段发送，那么`runtimecache` 往往应对着各种类型的资源，对于不同类型的资源往往也有不同的缓存策略。

### 缓存策略

workbox提供的缓存策划有以下几种:

+ staleWhileRevalidate
+ networkFirst
+ cacheFirst
+ networkOnly
+ cacheOnly

Workbox附带了一组可以与这些策略一起使用的插件。

+ workbox.expiration.Plugin
+ workbox.cacheableResponse.Plugin
+ workbox.broadcastUpdate.Plugin
+ workbox.backgroundSync.Plugin

要使用任何这些插件（或自定义插件），你只需将实例传递给插件选项。

####  staleWhileRevalidate

这种策略的意思是当请求的路由有对应的 Cache 缓存结果就直接返回， 在返回 Cache 缓存结果的同时会在后台发起网络请求拿到请求结果并更新 Cache 缓存，如果本来就没有 Cache 缓存的话，直接就发起网络请求并返回结果，这对用户来说是一种非常安全的策略，能保证用户最快速的拿到请求的结果。

但是也有一定的缺点，就是还是会有网络请求占用了用户的网络带宽。

![staleWhileRevalidate](/images/html5/staleWhileRevalidate.png)

```js
workbox.routing.registerRoute(
    new RegExp('https://edu-cms\.nosdn\.127\.net/topics/'),
    workbox.strategies.staleWhileRevalidate({
        //cache名称
        cacheName: 'lf-sw:static',
        plugins: [
            new workbox.expiration.Plugin({
                //cache最大数量
                maxEntries: 30
            })
        ]
    })
);
```

#### networkFirst

这种策略就是当请求路由是被匹配的，就采用网络优先的策略，也就是优先尝试拿到网络请求的返回结果，如果拿到网络请求的结果，就将结果返回给客户端并且写入 Cache 缓存。

如果网络请求失败，那最后被缓存的 Cache 缓存结果就会被返回到客户端，这种策略一般适用于返回结果不太固定或对实时性有要求的请求，为网络请求失败进行兜底

![networkFirst](/images/html5/networkFirst.png)

```js

//自定义要缓存的html列表
var cacheList = [
    '/Hexo/public/demo/PWADemo/workbox/index.html'
];
workbox.routing.registerRoute(
    //自定义过滤方法
    function(event) {
        // 需要缓存的HTML路径列表
        if (event.url.host === 'localhost:63342') {
            if (~cacheList.indexOf(event.url.pathname)) return true;
            else return false;
        } else {
            return false;
        }
    },
    workbox.strategies.networkFirst({
        cacheName: 'lf-sw:html',
        plugins: [
            new workbox.expiration.Plugin({
                maxEntries: 10
            })
        ]
    })
);
```

#### cacheFirst

这个策略的意思就是当匹配到请求之后直接从 Cache 缓存中取得结果，如果 Cache 缓存中没有结果，那就会发起网络请求，拿到网络请求结果并将结果更新至 Cache 缓存，并将结果返回给客户端。这种策略比较适合结果不怎么变动且对实时性要求不高的请求

![cacheFirst](/images/html5/cacheFirst.png)

```js
workbox.routing.registerRoute(
    new RegExp('https://edu-image\.nosdn\.127\.net/'),
    workbox.strategies.cacheFirst({
        cacheName: 'lf-sw:img',
        plugins: [
            //如果要拿到域外的资源，必须配置
            //因为跨域使用fetch配置了
            //mode: 'no-cors',所以status返回值为0，故而需要兼容
            new workbox.cacheableResponse.Plugin({
                statuses: [0, 200]
            }),
            new workbox.expiration.Plugin({
                maxEntries: 40,
                //缓存的时间
                maxAgeSeconds: 12 * 60 * 60
            })
        ]
    })
);
```

#### networkOnly

比较直接的策略，直接强制使用正常的网络请求，并将结果返回给客户端，这种策略比较适合对实时性要求非常高的请求。

![networkOnly](/images/html5/networkOnly.png)

#### cacheOnly

这个策略也比较直接，直接使用 Cache 缓存的结果，并将结果返回给客户端，这种策略比较适合一上线就不会变的静态资源请求。

![cacheOnly](/images/html5/cacheOnly.png)

## 示例

```js
//首先是异常处理
self.addEventListener('error', function(e) {
  self.clients.matchAll()
    .then(function (clients) {
      if (clients && clients.length) {
        clients[0].postMessage({ 
          type: 'ERROR',
          msg: e.message || null,
          stack: e.error ? e.error.stack : null
        });
      }
    });
});

self.addEventListener('unhandledrejection', function(e) {
  self.clients.matchAll()
    .then(function (clients) {
      if (clients && clients.length) {
        clients[0].postMessage({
          type: 'REJECTION',
          msg: e.reason ? e.reason.message : null,
          stack: e.reason ? e.reason.stack : null
        });
      }
    });
})
//然后引入workbox
importScripts('https://g.alicdn.com/kg/workbox/3.3.0/workbox-sw.js');
workbox.setConfig({
  debug: false,
  modulePathPrefix: 'https://g.alicdn.com/kg/workbox/3.3.0/'
});
//直接激活跳过等待阶段
workbox.skipWaiting();
workbox.clientsClaim();
//定义要缓存的html
var cacheList = [
  '/',
  '/tbhome/home-2017',
  '/tbhome/page/market-list'
];
//html采用networkFirst策略，支持离线也能大体访问
workbox.routing.registerRoute(
  function(event) {
    // 需要缓存的HTML路径列表
    if (event.url.host === 'www.taobao.com') {
      if (~cacheList.indexOf(event.url.pathname)) return true;
      else return false;
    } else {
      return false;
    }
  },
  workbox.strategies.networkFirst({
    cacheName: 'tbh:html',
    plugins: [
      new workbox.expiration.Plugin({
        maxEntries: 10
      })
    ]
  })
);
//静态资源采用staleWhileRevalidate策略，安全可靠
workbox.routing.registerRoute(
  new RegExp('https://g\.alicdn\.com/'),
  workbox.strategies.staleWhileRevalidate({
    cacheName: 'tbh:static',
    plugins: [
      new workbox.expiration.Plugin({
        maxEntries: 20
      })
    ]
  })
);
//图片采用cacheFirst策略，提升速度
workbox.routing.registerRoute(
  new RegExp('https://img\.alicdn\.com/'),
  workbox.strategies.cacheFirst({
    cacheName: 'tbh:img',
    plugins: [
      new workbox.cacheableResponse.Plugin({
        statuses: [0, 200]
      }),
      new workbox.expiration.Plugin({
        maxEntries: 20,
        maxAgeSeconds: 12 * 60 * 60
      })
    ]
  })
);

workbox.routing.registerRoute(
  new RegExp('https://gtms01\.alicdn\.com/'),
  workbox.strategies.cacheFirst({
    cacheName: 'tbh:img',
    plugins: [
      new workbox.cacheableResponse.Plugin({
        statuses: [0, 200]
      }),
      new workbox.expiration.Plugin({
        maxEntries: 30,
        maxAgeSeconds: 12 * 60 * 60
      })
    ]
  })
);
```



