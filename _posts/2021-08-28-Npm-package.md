---
layout:     post
title:      NPM package
subtitle:   v7
date:       2021-08-28
author:     huangqing
header-img: img/post-bg-npm.jpg
catalog: true
categories: [Web]
tags:
    - npm
---

![npm package.json mindmap](/images/npm/npm-v7-package.png)

[npm package.json v7](https://docs.npmjs.com/cli/v7/configuring-npm/package-json#private)

## files

`files` 定义了哪些文件应该被包括在 npm install 后的 node_modules中。

有些文件是自动暴露出来的，不管你是不是配置了files，比如：

+ package.json
+ README / CHANGELOG / LICENSE

vite 中是这样配置的：
```json
{
  "files": [ "bin", "dist", "client.d.ts" ]
}
```

## bin

`bin` 列出了可执行文件，表示你这个包要对外提供哪些脚本。

在这个包被 `install` 安装时，如果是全局安装 `-g`，`bin` 列出的可执行文件会被添加到 PATH 变量（全局可执行）；如果是局部安装，则会进入到 `node_modules/.bin/` 目录下。

bin 在一些 CLI 工具中用得很频繁，比如 Vue CLI。

在开发 npm 包时，要求发布的可执行脚本要以`#!/usr/bin/env node`开头，用于指明该脚本文件要使用 node 来执行。


## main, browser, module

### 文件优先级



我们使用的模块规范有 ESM 和 commonJS 两种，为了能在 node 环境下原生执行 ESM 规范的脚本文件，`.mjs` 文件就应运而生。

>Node 原始的模块方式 CommonJS 简称为 CJS，而 ES Module 称为 ESM。

当存在 `index.mjs` 和 `index.js` 这种同名不同后缀的文件时，`import './index'` 或者 `require('./index')` 是会优先加载 `index.mjs` 文件

**mjs > js**

### 字段定义

+ `main` : 定义了 npm 包的入口文件，browser 环境和 node 环境均可使用
+ `module` : 定义 npm 包的 ESM 规范的入口文件，browser 环境和 node 环境均可使用
+ `browser` : 定义 npm 包在 browser 环境下的入口文件

### npm包
+ 如果 npm 包导出的是 `ESM` 规范的包，使用 `module`
+ 如果 npm 包只在 `web` 端使用，并且严禁在 server 端使用，使用 `browser`。
+ 如果 npm 包只在 `server` 端使用，使用 `main`
+ 如果 npm 包在 `web` 端和 `server` 端都允许使用，使用 `browser` 和 `main`

## scripts

scripts也基本上每天都用了，但是它的钩子脚本你用过吗？如果没有用过，可以试试，在组织脚本流程时非常好用！

+ `pre`：在一个script执行前执行，比如prebuild，可以在打包前做一些准备工作。
+ `post`：在一个script执行后执行，比如postbuild，可以在打包后做一些收尾工作。

## config

通过`config`配置的参数`xxx`，可以在脚本中通过`npm_package_config_xxx` 的形式引用，比如`port`。
```json
{
  "config": {
    "port": "8080"
  }
}
```

## 依赖相关

### dependencies

`dependencies`可以理解为**生产依赖**，通过`npm install --save`安装的依赖包都会进入到dependencies中。

### devDependencies

`devDependencies`可以理解为**开发环境依赖**，通常是一些工具类的包，比如 webpack, babel等。通过`npm install --save-dev`安装的依赖包都会进入到`devDependencies`中。

### peerDependencies

>我是**package-a**，你装我，你就必须装我的**peerDependencies**。

让“调包侠”将**package-a的依赖**提升到自己的`node_modules`中，这样可以在“调包侠”和`package-a`都需要同一个依赖（比如vue）时，避免重复安装。这常见于开发组件或者库。

注意，一个 npm 包的开发者如果声明了`peerDependencies`,开发环境下在该包目录`npm install`也不会在`node_modules`中安装这些依赖，所以往往还需要借助`devDependencies`。

举个例子，我开发一个组件，不想发布到 npm 时包含了 vue 的代码，这就需要外部提供 vue ，所以我把 vue 定义在 peerDependencies 也无可厚非。但是，在开发组件时，一般还需要本地开发环境跑一个 demo 试试效果，这时候是依赖 vue 的，所以还需要在 devDependencies 中安装 vue 。 vue-router 就是这么做的.

### bundledDependencies

`bundledDependencies`跟上面的依赖都不太一样，配置上不是键值对的形式，而是一个数组。

```json
{
  "bundledDependencies": [
    "vue",
    "vue-router"
  ]
}
```

在运行`npm pack`时，会将对应依赖打包到`tgz`文件中。用得不多，不知道具体的细节，主要还是直接用`npm install`安装 tgz 包的场景比较少，有个概念就行。

### optionalDependencies

`optionalDependencies`用于配置可选的依赖，即使配了这个，代码里也要做好判断（保护），否则运行报错就不好玩了。

```js
try {
  var foo = require('foo')
  var fooVersion = require('foo/package.json').version
} catch (er) {
  foo = null
}
```

## 参考

+ [这还是我最熟悉的package.json吗？](https://cloud.tencent.com/developer/article/1819328)

+ [package.json中你还不清楚的browser，module，main 字段优先级](https://www.cnblogs.com/qianxiaox/p/14041717.html)