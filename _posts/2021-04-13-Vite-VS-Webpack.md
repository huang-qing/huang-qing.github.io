---
layout: post
title: Vite VS Webpack
subtitle:
date: 2021-04-13
author: huangqing
header-img: img/post-bg-vue.png
catalog: true
categories: [Vite]
tags:
  - Vite
---

## webpack vs vite

底层实现上， Vite 是基于 esbuild 预构建依赖的。

esbuild 使用 go 编写，并且比以 js 编写的打包器预构建依赖, 快 10 - 100 倍。

## webpack

webpack打包过程:

1. 识别入口文件
2. 通过逐层识别模块依赖。（Commonjs、amd或者es6的import，webpack都会对其进行分析。来获取代码的依赖）
3. webpack做的就是分析代码。转换代码，编译代码，输出代码
4. 最终形成打包后的代码

webpack打包原理:

1. 先逐级递归识别依赖，构建依赖图谱
2. 将代码转化成AST抽象语法树
3. 在AST阶段中去处理代码
4. 把AST抽象语法树变成浏览器可以识别的代码， 然后输出

![](/images/vite/webpack-bundle-based-dev-server.jpg)
![](/images/vite/webpack-init.jpg)
![](/images/vite/webpack-server.png)
## vite

当声明一个 `script` 标签类型为 `module` 时,浏览器就会像服务器发起一个`GET`。

`<script type="module" src="/src/main.js"></script>`浏览器请求到了`main.js`文件，检测到内部含有`import`引入的包，又会对其内部的 `import` 引用发起 `HTTP` 请求获取模块的内容文件。

Vite 的主要功能就是通过劫持浏览器的这些请求，并在后端进行相应的处理将项目中使用的文件通过简单的分解与整合，然后再返回给浏览器，vite整个过程中没有对文件进行打包编译，所以其运行速度比原始的webpack开发编译速度快出许多

![](/images/vite/vite-native-esm-based-dev-server.jpg)
![](/images/vite/vite-init.jpg)
![](/images/vite/vite-server.png)


## vite 改进

+ Vite 通过在一开始将应用中的模块区分为 依赖 和 源码 两类，改进了开发服务器启动时间。
+ 依赖 大多为纯 JavaScript 并在开发时不会变动。一些较大的依赖（例如有上百个模块的组件库）处理的代价也很高。依赖也通常会以某些方式（例如 ESM 或者 CommonJS）被拆分到大量小模块中。
+ Vite 将会使用 esbuild 预构建依赖。Esbuild 使用 Go 编写，并且比以 JavaScript 编写的打包器预构建依赖快 10-100 倍。
+ 源码 通常包含一些并非直接是 JavaScript 的文件，需要转换（例如 JSX，CSS 或者 Vue/Svelte 组件），时常会被编辑。同时，并不是所有的源码都需要同时被加载。（例如基于路由拆分的代码模块）。
+ Vite 以 原生 ESM 方式服务源码。这实际上是让浏览器接管了打包程序的部分工作：Vite 只需要在浏览器请求源码时进行转换并按需提供源码。根据情景动态导入的代码，即只在当前屏幕上实际使用时才会被处理
+ 在 Vite 中，HMR 是在原生 ESM 上执行的。当编辑一个文件时，Vite 只需要精确地使已编辑的模块与其最近的 HMR 边界之间的链失效（大多数时候只需要模块本身），使 HMR 更新始终快速，无论应用的大小。
+ Vite 同时利用 HTTP 头来加速整个页面的重新加载（再次让浏览器为我们做更多事情）：源码模块的请求会根据 304 Not Modified 进行协商缓存，而依赖模块请求则会通过 Cache-Control: max-age=31536000,immutable 进行强缓存，因此一旦被缓存它们将不需要再次请求。

## 打包 or 不打包

在 HTTP 1.1 的标准下，每次请求都需要单独建立 TCP 链接，经过完整的通讯过程，非常耗时；

![](/images/vite/http1-1.jpg)


而且每次请求除了请求体中的内容，请求头中也会包含很多数据，大量请求的情况下也会浪费很多资源。

但是这些问题随着 HTTP 2 的出现，也就不复存在了。

![](/images/vite/http-1-2.jpg)


## vite 支持

### Scoped CSS
```html
<style scoped>
/**/
</style>
```

### CSS Module

SFC中使用CSS Module

```html
<template>
  <div :class="$style.messageBox"></div>
</template>

<style module>
  .message-box{
    padding:10px 20px;
    color:#fff;
  }
</style>
```

JS中导入CSS Module：将css文件命名为`*.module.css`即可

```js
import style from './Message.module.css'

export default{
  computed:{
    $style(){
      return style;
    }
  }
}
```

### CSS预处理器

安装对应的预处理器就可以直接在vite项目中使用。

```html
<style lang="scss">
/* use scss */
</style>
```

或者在JS中导入

```js
import './style.scss'
```

###  `<script setup>` 

正常写法:

```html
<script lang="ts">
import { defineComponent } from "vue";
import { useRouter } from "vue-router";
export default defineComponent({
  setup() {
    const router = useRouter();
    const goLogin = () => {
      router.push("/");
    };
    return { goLogin };
  },
});
</script>
```

`<script setup>`写法:

```html
<script lang="ts" setup="props">
import { useRouter } from "vue-router";
const router = useRouter();
const goLogin = () => {
  router.push("/");
};
</script>
```

### `<style vars>` 

```html
<script lang="ts" setup="props">
const state = reactive({
  color: "#ccc",
});
</script>
<style >
.text {
  color: v-bind("state.color");
}
</style>
```


### Vue3 Jsx支持

vue3中`jsx`支持需要引入插件：`@vitejs/plugin-vue-jsx`

```bash
$ npm i @vitejs/plugin-vue-jsx -D
```

注册插件，`vite.config.js`

```js
import vueJsx from "@vitejs/plugin-vue-jsx";

export default {
  plugins: [vue(), vueJsx()],
}
```

```html
<!-- 1.标记为jsx -->
<script setup lang="jsx">
import { defineComponent } from "vue";
import HelloWorld from "comps/HelloWorld.vue";
import logo from "./assets/logo.png"

// 2.用defineComponent定义组件且要导出
export default defineComponent({
  render: () => (
    <>
      <img alt="Vue logo" src={logo} />
      <HelloWorld msg="Hello Vue 3 + Vite" />
    </>
  ),
});
</script>
```




## 参考

+ [备战2021：vite工程化实践，建议收藏](https://juejin.cn/post/6910014283707318279)
+ [备战2021：Vite2项目最佳实践](https://juejin.cn/post/6924912613750996999)
+ [2021必知必会的vite+vue3项目最佳实践](https://juejin.cn/post/6926822933721513998)
+ [手撸Vite，揭开Vite神秘面纱](https://juejin.cn/post/6960110425438421006)

+ [Vite和Webpack的核心差异](https://mp.weixin.qq.com/s?__biz=MzkwODIwMDY2OQ==&mid=2247488867&idx=1&sn=e3ce0595309d90914de76bedcc7b8daa&chksm=c0cccad1f7bb43c7025c800d97d34148c6ba81d6ce383933f83413717ae6fad7db7071373a99&mpshare=1&scene=24&srcid=0224ivZtmXvQJ7uENAFEammB&sharer_sharetime=1614179088726&sharer_shareid=3f8e3a43f78ce137b6d0613608887aa1#rd)

+ [vite2.0+vue3移动端项目实战](https://mp.weixin.qq.com/s?__biz=MzA4Nzg0MDM5Nw==&mid=2247492764&idx=1&sn=479aed0aace0868377081d1537f53a49&chksm=9031e77ea7466e6852084bafbb047f7c22edf4e5f13e3763da1e00eb101cdcb9a0e050866a09&mpshare=1&scene=24&srcid=0228pfWDyMbKyUv2iVQO9ppi&sharer_sharetime=1614491261067&sharer_shareid=3f8e3a43f78ce137b6d0613608887aa1#rd)
+ [前端新玩具：Vite 及相关思考](https://zhuanlan.zhihu.com/p/268877593)
+ [前端新工具--vite从入门到实战（一）](https://zhuanlan.zhihu.com/p/149033579)
+ [项目升级到Vite踩的坑都在这了！](https://mp.weixin.qq.com/s?__biz=MzkwODIwMDY2OQ==&mid=2247489471&idx=1&sn=1966d911c3798e4a55b7e023449d12c7&chksm=c0ccc80df7bb411b51a617060ab7d4b091636c84b4f7185df47fef592b15ce19abf1fa9f0ef9&mpshare=1&scene=24&srcid=0418cv0kTcJr0Z9EeqcvkxOy&sharer_sharetime=1618713237012&sharer_shareid=3f8e3a43f78ce137b6d0613608887aa1#rd)