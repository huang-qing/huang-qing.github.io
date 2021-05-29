---
layout: post
title: ESbuild
subtitle: An extremely fast JavaScript bundler
date: 2021-05-22
author: huangqing
header-img: img/post-bg-frontend.jpg
catalog: true
categories: [Frontend Engineering]
tags:
  - 前端工程化
  - esbuild
---

[esbuild](https://github.com/evanw/esbuild)
[esbuild io](https://esbuild.github.io/)

## 介绍

Major features:

![esbuild-speed](/images/esbuild/esbuild-speed.jpg)

- Extreme speed without needing a cache
- ES6 and CommonJS modules
- Tree shaking of ES6 modules
- An API for JavaScript and Go
- TypeScript and JSX syntax
- Source maps
- Minification
- Plugins

## Command-line usage

The command-line interface takes a list of entry points and produces one bundle file per entry point. Here are the available options:

Usage:
esbuild [options] [entry points]

```
Options:
  --name=...            The name of the module
  --bundle              Bundle all dependencies into the output files
  --outfile=...         The output file (for one entry point)
  --outdir=...          The output directory (for multiple entry points)
  --sourcemap           Emit a source map
  --error-limit=...     Maximum error count or 0 to disable (default 10)
  --target=...          Language target (default esnext)
  --platform=...        Platform target (browser or node, default browser)
  --external:M          Exclude module M from the bundle
  --format=...          Output format (iife or cjs)
  --color=...           Force use of color terminal escapes (true or false)

  --minify              Sets all --minify-* flags
  --minify-whitespace   Remove whitespace
  --minify-identifiers  Shorten identifiers
  --minify-syntax       Use equivalent but shorter syntax

  --define:K=V          Substitute K with V while parsing
  --jsx-factory=...     What to use instead of React.createElement
  --jsx-fragment=...    What to use instead of React.Fragment
  --loader:X=L          Use loader L to load file extension X, where L is
                        one of: js, jsx, ts, tsx, json, text, base64

  --trace=...           Write a CPU trace to this file
  --cpuprofile=...      Write a CPU profile to this file
  --version             Print the current version and exit
```

Examples:

```
  # Produces dist/entry_point.js and dist/entry_point.js.map
  esbuild --bundle entry_point.js --outdir=dist --minify --sourcemap

  # Allow JSX syntax in .js files
  esbuild --bundle entry_point.js --outfile=out.js --loader:.js=jsx

  # Substitute the identifier RELEASE for the literal true
  esbuild example.js --outfile=out.js --define:RELEASE=true
```

## ESbuild API

「esbuild」总共提供了 2 个函数： `transform` 、 `build`

## ESbuild 使用介绍

ESbuild 是一个类似 webpack 构建工具。它的构建速度是 webpack 的几十倍。

为什么这么快:

1. js 是单线程串行，esbuild 是新开一个进程，然后多线程并行，充分发挥多核优势
2. go 是纯机器码，肯定要比 JIT 快
3. 不使用 AST，优化了构建流程。（也带来了一些缺点，后面会说）

### 利用 esbuild 编译代码

esbuild 提供了 `writeFileSync/writeFile` 对 code 进行编译, demo 如下

```js
require("fs").writeFileSync("in.ts", "let x: number =   1");

require("esbuild").buildSync({
  entryPoints: ["in.ts"],
  outfile: "out.js",
});
```

### 利用 esbuild 处理 jsx 代码

```js
require("esbuild").transformSync("<div/>", {
  jsxFactory: "h", //默认为 React.CreateElement,可自定义, 如果你想使用 Vue 的 jsx 写法, 将该值换成为 Vue.CreateElement
  loader: "jsx", // 将 loader 设置为 jsx 可以编译 jsx 代码
});

// 同上，默认为 React.Fragment , 可换成对应的 Vue.Fragment。
require("esbuild").transformSync("<>x</>", {
  jsxFragment: "Fragment",
  loader: "jsx",
});
```

### 利用 esbuild 压缩代码体积

```js
var js = "fn = obj => { return obj.x }";

require("esbuild").transformSync(js, {
  minify: true,
});
```

```json
// minify 后
{
  "code": "fn=n=>n.x;\n",
  "map": "",
  "warnings": []
}
```

### 处理其他资源

esbuild 内置了一些文件处理的 loader。 当 esbuild 解析到某后缀时，会自动使用该 loader 进行处理。 当然，你也可以手动指定对应的 loader 处理器，如你想使用 jsx loader 去处理 js 文件。可以按下面的实例进行配置。

目前 ESBuild 内置了 js,jsx,ts,tsx,css,text,binary,dataurl,file 类型的 loader

```js
require("esbuild").buildSync({
  entryPoints: ["app.js"],
  bundle: true,
  loader: { ".js": "jsx" }, // 默认使用 js loader ,手动改为 jsx-loader
  outfile: "out.js",
});
```

### 用 esbuild 启动一个 web server 用于调试（支持热更新）

```js
require("esbuild")
  .serve(
    {},
    {
      entryPoints: ["app.js"],
      bundle: true,
      outfile: "out.js",
    }
  )
  .then((server) => {
    // Call "stop" on the web server when you're done
    server.stop();
  });
```

## 参考资料

- [ESbuild 介绍](https://juejin.cn/post/6918927987056312327)
