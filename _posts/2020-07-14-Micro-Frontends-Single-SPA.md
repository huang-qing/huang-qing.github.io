---
layout: post
title: 实现微前端
subtitle: 
date: 2020-07-14
author: huangqing
header-img: img/post-bg-micro-frontends.jpg
catalog: true
categories: [Micro-Frontends]
tags:
  - 微前端
---

## 框架

### 客户端框架

![客户端框架](/images/micro-frontends/micro-frontends-client.png)

下列框架实现了这种（或类似的）模式：

+ Piral

+ Open Components

+ qiankun

+ Luigi

+ Frint.js

### 服务端框架

![服务端框架](/images/micro-frontends/micro-frontends-server.png)

下列框架实现这种（或类似的）模式：

+ Mosaic

+ PuzzleJs

+ Podium

+ Micromono


### 帮助（Helper）库

帮助库或是提供共享依赖、路由事件的基础架构，或是将不同的微前端及其生命周期组织起来 。

![帮助（Helper）库](/images/micro-frontends/micro-frontends-help.png)

下面的库可用于削减模板代码：

+ Module Federation

+ Siteless

+ Single SPA

+ Postal.js

+ EventBus


## 实现微前端的十种方式

### 1 Vue和React、Jquery共同开发,通信

[实现微前端的十种方式 【第一种】](https://segmentfault.com/a/1190000023100330?utm_source=tag-newest)

[实现微前端的十种方式 【第二种】](https://mp.weixin.qq.com/s?__biz=MzI2NTk2NzUxNg==&mid=2247487404&idx=1&sn=ffe076ae2b180679284ed62dceb4ae8a&chksm=ea940d5fdde3844901783f706752422cb82379d2636e3a941b3d1d303b5aafa83d5303ea68fd&mpshare=1&scene=24&srcid=0708ZbKWaJJHwhqxf0FQBB6n&sharer_sharetime=1594167691544&sharer_shareid=3f8e3a43f78ce137b6d0613608887aa1#rd)

**Vue和React、Jquery共同开发,通信**

```json
babel配置

{
    "presets": [
        "@babel/preset-react",
        [
            "@babel/preset-env",
            {
                "targets": {
                    "electron": "4"
                },
                "modules": false
            }
        ]
    ],
    "plugins": [
        "@babel/plugin-proposal-class-properties",
        "@babel/plugin-syntax-async-generators",
        "@babel/plugin-syntax-dynamic-import"
    ]
}
```

Jquery兼容性最好，vue和react不相上下，需要一些特定的babel进行处理

**React的入口**
```js
import React from 'react';
import dva from 'dva';
import App from './App';

import demo from '../App/Dva-model/demo.js';
const app = dva();

app.model(demo);
app.router(({ history, app: store }) => (
  <App history={history} getState={store._store.getState} dispatch={store._store.dispatch} />
));

app.start('#root');
```

以React组件为基座，vue为子应用.进入某个React组件，然后调用vue渲染

**定义vue入口文件**
```js
function vueRender(props) {
  new Vue({
    router,
    store,
    render: (h) => h(App),
  }).$mount('#test');
}

export default vueRender;
```

在React组件中调用vue渲染

```js
import vueRender from '@v/startVUe.js';
...
  componentDidMount() {
    vueRender(this.props);
  }
```

这样vue能接受到React组件的props，然后也可以正常通过vue渲染，使用vue的技术栈了

**如何通信，数据共享**

这其实是最简单的，类似 redux 应该是最简单的,在基座中生成state和reducer，也可以通过事件总线,Map数据维护一份
像dva封装了dispatch在props中，只要react基座文件connect了，那么就可以拿到dispatch，可以调用任意的reducer和effects.