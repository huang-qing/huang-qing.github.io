---
layout: post
title: tsconfig
subtitle:
date: 2021-05-22
author: huangqing
header-img: img/post-bg-frontend.jpg
catalog: true
categories: [Frontend Engineering]
tags:
  - 前端工程化
  - tsconfig
---

## 使用tsconfig.json

+ 不带任何输入文件的情况下调用`tsc`，编译器会从当前目录开始去查找`tsconfig.json`文件，逐级向上搜索父目录。
+ 不带任何输入文件的情况下调用`tsc`，且使用命令行参数`--project`（或`-p`）指定一个包含`tsconfig.json`文件的目录。

## 配置项

### `compilerOptions` : 编译配置

### `files` : 指定一个包含相对或绝对文件路径的列表

```json
{
    "compilerOptions": {
        "module": "commonjs",
        "noImplicitAny": true,
        "removeComments": true,
        "preserveConstEnums": true,
        "sourceMap": true
    },
    "files": [
        "core.ts",
        "sys.ts",
        "types.ts",
    ]
}
```

### `include` : 包含的文件，指定一个文件glob匹配模式列表

### `exclude` : 排除的文件，指定一个文件glob匹配模式列表

支持的glob通配符有：

+ `*` 匹配0或多个字符（不包括目录分隔符）
+ `?` 匹配一个任意字符（不包括目录分隔符）
+ `**/` 递归匹配任意子目录

```json
{
    "compilerOptions": {
        "module": "system",
        "noImplicitAny": true,
        "removeComments": true,
        "preserveConstEnums": true,
        "outFile": "../../built/local/tsc.js",
        "sourceMap": true
    },
    "include": [
        "src/**/*"
    ],
    "exclude": [
        "node_modules",
        "**/*.spec.ts"
    ]
}
```
### `typeRoots` ：默认所有可见的"`@types`"包会在编译过程中被包含进来，指定了`typeRoots`，只有`typeRoots`下面的包才会被包含进来

```json
{
   "compilerOptions": {
       "typeRoots" : ["./typings"]
   }
}
```

### `types` : 指定了`types`，只有被列出来的包才会被包含进来

```json
{
   "compilerOptions": {
        "types" : ["node", "lodash", "express"]
   }
}
```

### `extends`继承配置

```json
{
  "extends": "./configs/base",
  "files": [
    "main.ts",
    "supplemental.ts"
  ]
}
```


## 参考资料

- [工程配置](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/tsconfig.json.html)
