---
layout: post
title: Vite 插件
subtitle: Vite Plugin
date: 2021-05-11
author: huangqing
header-img: img/post-bg-vue.png
catalog: true
categories: [Vite]
tags:
  - Vite
---

![vite](/images/vite/vite.png)

## Vite插件是什么

使用Vite插件可以扩展Vite能力，比如解析用户自定义的文件输入，在打包代码前转译代码，或者查找第三方模块。

![](/images/vite/vite-devServer.png)

## Vite插件的形式

Vite插件扩展自Rollup插件接口，只是额外多了一些Vite特有选项。

Vite插件是一个拥有名称、创建钩子(build hook)或生成钩子(output generate hook)的对象

## 通用钩子

在开发中，Vite 开发服务器会创建一个插件容器来调用 Rollup 构建钩子，与 Rollup 如出一辙。

以下钩子在服务器启动时被调用：

+ `options`
+ `buildStart`

以下钩子会在每个传入模块请求时被调用：

+ `resolveId`
+ `load`
+ `transform`

以下钩子在服务器关闭时被调用：

+ `buildEnd`
+ `closeBundle`

### 范例： 引入一个虚拟文件

```js
export default function myPlugin(options) {
  const virtualFileId = '@my-virtual-file'

  return {
    name: 'my-plugin', // 必须的，将会显示在 warning 和 error 中
    resolveId(id) {
      if (id === virtualFileId) {
        return virtualFileId // 返回source表明命中，vite不再询问其他插件处理该id请求
      }
    },
    load(id) {
      if (id === virtualFileId) {
        return `export const msg = "from virtual file"` // 返回"virtual-module"模块源码
      }
    }
  }
}
```

这使得可以在 JavaScript 中引入这些文件：

```js
import { msg } from '@my-virtual-file'

console.log(msg)
```

### 范例： 转换自定义文件类型

```js
const fileRegex = /\.(my-file-ext)$/

export default function myPlugin() {
  return {
    name: 'transform-file',

    transform(src, id) {
      if (fileRegex.test(id)) {
        return {
          code: compileFileToJS(src),
          map: null // 如果可行将提供 source map
        }
      }
    }
  }
}
```

## Vite特有钩子

+ `config`: 修改Vite配置
+ `configResolved` ：Vite配置确认
+ `configureServer` ：用于配置dev server
+ `transformIndexHtml` ：用于转换宿主页
+ `handleHotUpdate` ：自定义HMR更新时调用

## 范例：configureServer 自定义中间件

最常见的用例是在内部 `connect` 应用程序中添加自定义中间件:

```JS
const myPlugin = () => ({
  name: 'configure-server',
  configureServer(server) {
    server.middlewares.use((req, res, next) => {
      // 自定义请求处理...
    })
  }
})
```

`注入后置中间件` : 如果你想注入一个在内部中间件 之后 运行的中间件，你可以从 `configureServer` 返回一个函数，将会在内部中间件安装后被调用：

```js
const myPlugin = () => ({
  name: 'configure-server',
  configureServer(server) {
    // 返回一个在内部中间件安装后
    // 被调用的后置钩子
    return () => {
      server.middlewares.use((req, res, next) => {
        // 自定义请求处理...
      })
    }
  }
})
```

## 范例：transform 存储服务器访问

在某些情况下，其他插件钩子可能需要访问开发服务器实例（例如访问 websocket 服务器、文件系统监视程序或模块图）。这个钩子也可以用来存储服务器实例以供其他钩子访问:

```js
const myPlugin = () => {
  let server
  return {
    name: 'configure-server',
    configureServer(_server) {
      server = _server
    },
    transform(code, id) {
      if (server) {
        // 使用 server...
      }
    }
  }
}
```

### 范例： transformIndexHtml

```js
const htmlPlugin = () => {
  return {
    name: 'html-transform',
    transformIndexHtml(html) {
      return html.replace(
        /<title>(.*?)<\/title>/,
        `<title>Title replaced!</title>`
      )
    }
  }
}
```

## 钩子调用顺序

![](/images/vite/vite-hooks-sort.png)

## 插件顺序

+ 别名处理`Alias`
+ 带有`enforce: 'pre'`  的用户插件
+ Vite核心插件
+ 没有 `enforce` 值的用户插件
+ Vite构建插件
+ 带有 `enforce: 'post'` 的用户插件
+ Vite 后置构建插件(minify, manifest, reporting)

![vite-plugin-sort](/images/vite/vite-plugin-sort.png)

+ [Vite 插件 API](https://cn.vitejs.dev/guide/api-plugin.html#transforming-custom-file-types)
+ [精通Vite2之插件开发指南](https://juejin.cn/post/6950217775663513608)
+ [如何编写一个 Vite 插件](https://juejin.cn/post/6876812524338216973)
+ [代码地址](https://github.com/ImpactBox/vite-plugin-auto-routes)
+ [通过 Vite 插件实现基于文件目录的路由](https://mp.weixin.qq.com/s?__biz=MzI3NTM5NDgzOA==&mid=2247497061&idx=3&sn=8c30bd9271d95f9bd3db9d1ea11fbe05&chksm=eb07cd1cdc70440a440d99f4ddb39e03fdd623953eb430ceda2692daddc703d07ed6240c9b38&mpshare=1&scene=24&srcid=0604V4iaWziOYA422bfdhZby&sharer_sharetime=1622820454628&sharer_shareid=3f8e3a43f78ce137b6d0613608887aa1#rd)
