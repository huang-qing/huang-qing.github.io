---
layout:     post
title:      Learn Vue - plugins
subtitle:   plugins
date:       2021-06-04
author:     huangqing
header-img: img/post-bg-vue.png
catalog: true
categories: [Vue]
tags:
    - Vue   
---

[Vue3 插件](https://v3.cn.vuejs.org/guide/plugins.html)

插件是自包含的代码，通常向 Vue 添加全局级功能。它可以是公开 `install()` 方法的 `object`，也可以是 `function`

插件的功能范围没有严格的限制——一般有下面几种：

1. 添加全局方法或者 property。如：vue-custom-element

2. 添加全局资源：指令/过滤器/过渡等。如：vue-touch

3. 通过全局 mixin 来添加一些组件选项。如: vue-router

4. 添加全局实例方法，通过把它们添加到 `config.globalProperties` 上实现。

5. 一个库，提供自己的 API，同时提供上面提到的一个或多个功能。如 : vue-router

## 编写插件

插件被添加到应用程序中时，如果它是一个对象，就会调用 `install` 方法。如果它是一个 `function`，则函数本身将被调用。在这两种情况下——它都会收到两个参数：由 Vue 的 `createApp` 生成的 `app` 对象和用户传入的选项。

```js
export default {
  install: (app, options) => {
    // Plugin code goes here
    app.config.globalProperties.$translate = key=>{}
    // 使用provide进行全局注入
    app.provide('i18n', options)
  }
}
```


## 参考

[Vue3 插件开发详解尝鲜版](https://blog.csdn.net/winnie_man_wei/article/details/106377844)