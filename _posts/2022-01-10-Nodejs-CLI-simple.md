---
layout: post
title: 前端CLI脚手架
subtitle:
date: 2022-01-10
author: huangqing
header-img: img/post-bg-nodejs.png
catalog: true
categories: [Nodejs]
tags:
  - nodejs
---


## `package.json` 中的 `bin` 字段

前端项目还是 `node` 项目，一般都会用 `npm` 做包管理工具，而 `package.json` 是其相关的配置信息。

对 `node` 项目而言，模块导出入口文件由 `package.json` 的 `main` 字段指定，而如果是要安装到`命令行`的工具，则是由 `package.json` 的 `bin` 字段指定。

### 配置单个命令

与包名同名

```json
{
  "name": "pro",
  "bin": "bin/pro.js"
}
```

这样安装的命令名称就是 `pro`。

自定义命令名称（与包名不同名）

```
{
  "name": "pro-cli",
  "bin": {
    "pro": "bin/pro.js"
  }
}
```

这样安装的命令名称也是 `pro`。

### 配置多个命令

```json
{
  "name": "pro-cli",
  "bin": {
    "pro": "bin/pro.js",
    "mini": "bin/mini.js"
  }
}
```

这样安装就有 `pro` 与 `mini` 两个命令。

## 对应 `bin/pro.js` 文件的写法

```js
#!/usr/bin/env node

require('../lib/pro');
```

与普通的 js 文件写法一样，只是前面要加上 #!/usr/bin/env node。

这段前缀代码叫 shebang，具体可以参考 [Shebang (Unix) - Wikipedia](https://en.wikipedia.org/wiki/Shebang_(Unix)).

## 安装方式

### 全局安装

```
npm i -g pro-cli
```

这种安装方式可以在命令行全局使用。

```
pro dev

pro build
```

### 本地安装

```
npm i --save-dev pro-cli
```

这种安装方式需要配合 `npm` 一起使用，比如：

package.json ：

```json
{
  "scripts": {
    "dev": "pro dev",
    "build": "pro build"
  }
}
```

使用 ：

```
npm run dev
npm run build
```


## 参考

- [从 1 到完美，用 node 写一个命令行工具](https://segmentfault.com/a/1190000016555129)
