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

![vite cli](/images/cli/vite-create-app.png)

## 入门须知

- [commander](https://github.com/tj/commander.js) （实现 NodeJS 命令行）
- [minimist](https://www.npmjs.com/package/minimist) 轻量级的用于解析命令行参数的工具

- [inquirer](https://www.npmjs.com/package/inquirer) （实现命令行之间的交互）
- [enquirer](https://www.npmjs.com/package/enquirer)命令行的提示

- [chalk](https://www.npmjs.com/package/chalk) （控制台字符样式）
- [kolorist](https://www.npmjs.com/package/kolorist) 轻量级的使命令行输出带有色彩的工具
- [log-symbols](https://www.npmjs.com/package/log-symbols) （为各种日志级别提供着色符号）

- [download](https://www.npmjs.com/package/download) （实现文件远程下载）
- [fs-extra](https://www.npmjs.com/package/fs-extra) （增强的基础文件操作库）
- [handlebars](https://www.npmjs.com/package/handlebars) （实现模板字符替换）

- [ora](https://www.npmjs.com/package/ora) （优雅终端 Spinner 等待动画）
- [update-notifier](https://www.npmjs.com/package/update-notifier) （npm 在线检查更新）

## npm init && npx

npm init 用法：

```shell
npm init [--force|-f|--yes|-y|--scope]
npm init <@scope> (same as `npx <@scope>/create`)
npm init [<@scope>/]<name> (same as `npx [<@scope>/]create-<name>`)
```

`npm init <initializer>` 时转换成`npx`命令:

- npm init foo -> npx create-foo
- npm init @usr/foo -> npx @usr/create-foo
- npm init @usr -> npx @usr/create

```shell
# 运行
npm init vue@next
# 相当于
npx create-vue@next
```

```shell
node_modules/.bin/vite -v
# vite/2.6.5 linux-x64 node-v14.16.0

# 等同于
# package.json script: "vite -v"
# npm run vite

npx vite -v
# vite/2.6.5 linux-x64 node-v14.16.0

# 无需全局安装
npx @vue/cli create vue-project
# @vue/cli 相比 npm init vue@next / npx create-vue@next 很慢。
```

## 文件目录

```doc
js-plugin-cli
├─ .gitignore
├─ .npmignore 
├─ .prettierrc 
├─ LICENSE
├─ README.md
├─ bin
│  └─ index.js
├─ lib
│  ├─ init.js
│  ├─ config.js
│  ├─ download.js
│  ├─ mirror.js
│  └─ update.js
└─ package.json
```

## 注册指令

当我们要运行调试脚手架时，通常执行 `node ./bin/index.js` 命令.使用cli时通常使用注册的指令。

打开 package.json 文件，先注册下指令：

```json
 "name": "create-v",
 "main": "./bin/index.js",
  "bin": {
    "create-v": "./bin/index.js"
  },

```

`bin` 下的 `create-v` 就是我们注册的指令,名称尽可能的简介，如 vue vite

## 实现一个简单的cli

让 `create-v -v` 这个命令能够在终端打印出来。

打开 `bin/index.js` 文件，编写以下代码:

```js
#!/usr/bin/env node

// 请求 commander 库
const program = require('commander')

// 从 package.json 文件中请求 version 字段的值，-v和--version是参数
// 命令行执行 create-v template -v
program.version(require('../package.json').version, '-v, --version')

// 命令行执行 create-v template
program
  .command('template')
  .description(
    'Download template from gitlab.'
  )
  .action(() => {
    //TODO:download
  });

// 命令行执行 create-v project_name  或 npx init v project_name
program
  .argument('<project_name>', 'create a project')
  .action((project) => {
    //TDDO:create
  });

// 解析命令行参数
program.parse(process.argv)

```

其中 `#!/usr/bin/env node` （固定第一行）必加，主要是让系统看到这一行的时候，会沿着对应路径查找 `node` 并执行

调试阶段时，为了保证 `create-v` 指令可用，需要在项目下执行 `npm link`（不需要指令时用 `npm unlink` 断开）;

link 完后,我们可以通过`npm ls -g`查看是否成功(有当前文件夹被映射到全局包中说明 link 成功)

测试完成后， 使用 `npm uninstall create-v -g` 对关联进行解绑


## 参考

- [create-vite](https://github.com/vitejs/vite/tree/main/packages/create-vite)
- [前端 CLI 脚手架思路解析-从 0 到 1 搭建 | 掘金技术征文-双节特别篇](https://juejin.cn/post/6879265583205089287)
- [300 行代码替代 Vue-CLI 脚手架的新工具](https://mp.weixin.qq.com/s?__biz=MzkwODIwMDY2OQ==&mid=2247491913&idx=1&sn=84d672761a93a38c5df67b993fa2630d&chksm=c0cf3efbf7b8b7edd9044976ce7f347ae6304a3435dae3341603a7f33f8e10aecd0735017ef6&mpshare=1&scene=24&srcid=1025hioblj7sAXwV5B9MVU4i&sharer_sharetime=1635137245716&sharer_shareid=3f8e3a43f78ce137b6d0613608887aa1#rd)
- [基于 Node 环境的终端 cli 翻译工具](https://mp.weixin.qq.com/s?__biz=MzIyNDU2NTc5Mw==&mid=2247497837&idx=2&sn=a50f2cc413d53f829ee79896b5296fda&chksm=e80fb723df783e35349856b20864fba5056b1847f88cee4bdc36d63bdec4c822355587a26e4f&mpshare=1&scene=24&srcid=1012dUrpT3PO3xY8VBJn343P&sharer_sharetime=1634013391035&sharer_shareid=3f8e3a43f78ce137b6d0613608887aa1#rd)
- [Vue CLI 是如何实现的 -- 终端命令行工具篇](https://mp.weixin.qq.com/s?__biz=Mzg4MTYwMzY1Mw==&mid=2247496577&idx=1&sn=83bb3e99f5b049f48cbcef0ca8e91f92&source=41#wechat_redirect)
- [手把手教你写一个脚手架](https://mp.weixin.qq.com/s?__biz=Mzg4MTYwMzY1Mw==&mid=2247496656&idx=1&sn=8be2316a38fe9e81105bfbf8883a16c2&source=41#wechat_redirect)
- [通过 Vite 的 create-app 学习如何实现一个简易版 CLI](https://mp.weixin.qq.com/s?__biz=MzkwODIwMDY2OQ==&mid=2247489377&idx=1&sn=3f82274d0db6e51f25609fc95e11b99b&chksm=c0ccc8d3f7bb41c540dbf9a3c6a31c7ae7f59bf37e7e7990f3565c1f32be79e28fcc7443d7fb&mpshare=1&scene=24&srcid=0408iqJkUrF8EVkoo15r1J3e&sharer_sharetime=1617864161758&sharer_shareid=3f8e3a43f78ce137b6d0613608887aa1#rd)
