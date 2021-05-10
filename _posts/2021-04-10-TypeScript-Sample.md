---
layout: post
title: TypeScript 简介
subtitle:
date: 2021-04-10
author: huangqing
header-img: img/post-bg-html.jpg
catalog: true
categories: [TypeScript]
tags:
  - TypeScript
---

## TypeScript 的特性

TypeScript 是静态类型

类型系统按照「类型检查的时机」来分类，可以分为动态类型和静态类型。

动态类型是指在运行时才会进行类型检查，这种语言的类型错误往往会导致运行时错误。JavaScript 是一门解释型语言[4]，没有编译阶段，所以它是动态类型。

静态类型是指编译阶段就能确定每个变量的类型，这种语言的类型错误往往会导致语法错误。TypeScript 在运行前需要先编译为 JavaScript，而在编译阶段就会进行类型检查。

## 安装 TypeScript

TypeScript 的命令行工具安装方法如下：

```
npm install -g typescript
```

编译一个 TypeScript 文件：

```
tsc hello.ts
```

主流的编辑器都支持 TypeScript，推荐使用 [Visual Studio Code](https://code.visualstudio.com/)。

## 基础

[tsconfig](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)

## 声明文件

[声明文件](https://ts.xcatliu.com/basics/declaration-files.html)

- `declare var` 声明全局变量
- `declare function` 声明全局方法
- `declare class` 声明全局类
- `declare enum` 声明全局枚举类型
- `declare namespace` 声明（含有子属性的）全局对象
- `interface` 和 `type` 声明全局类型
- `export` 导出变量
- `export namespace` 导出（含有子属性的）对象
- `export default` ES6 默认导出
- `export = commonjs` 导出模块
- `export as namespace` UMD 库声明全局变量
- `declare global` 扩展全局变量
- `declare module` 扩展模块
- `/// <reference />` 三斜线指令

## 导出模块

There is an export import syntax for legacy modules, and a standard export format for modern ES6 modules:

```js
// export the default export of a legacy (`export =`) module
export import MessageBase = require('./message-base');

// export the default export of a modern (`export default`) module
export { default as MessageBase } from './message-base';

// when '--isolatedModules' flag is provided it requires using 'export type'.
export type { default as MessageBase } from './message-base';

// export an interface from a legacy module
import Types = require('./message-types');
export type IMessage = Types.IMessage;

// export an interface from a modern module
export { IMessage } from './message-types';

// export all
export * from './message-base'
```

**导出模块的命名空间**

```js
import * as utils form './utils/mjs'
export * as utils from './utils.mjs'
```

## 教程

- [TypeScript 入门教程](https://ts.xcatliu.com/)
- [深入理解 TypeScript](https://jkchao.github.io/typescript-book-chinese/#why)
