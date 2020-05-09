---
layout:     post
title:      Learn Vue - 云平台
subtitle:   云平台技术栈学习
date:       2020-04-30
author:     huangqing
header-img: img/post-bg-vue.png
catalog: true
categories: [Vue]
tags:
    - Vue   
---

## 环境

1. [Nodejs 10+](https://nodejs.org/dist/)
2. vue `npm install vue`
3. vue-cli `npm install -g vue-cli`
4. yarn `npm install -g yarn`
5. UI:[ant design vue](https://www.antdv.com/docs/vue/introduce-cn/)

## vscode插件

1. Ant Design Vue helper
2. Chinese (Simplified) Language Pack for Visual Studio Code
3. Git Easy
4. JavaScript (ES6) code snippets
5. koroFileHeader
6. Path Intellisense
7. Vetur  Vue多功能集成插件

[vscode 前端常用插件推荐](https://blog.csdn.net/jiandan1127/article/details/85957003)

## chrome 调试

[vue-devtools](https://github.com/vuejs/vue-devtools)

## 基于 ant design vue pro

拉取代码：

```npm
git clone --depth=1 https://github.com/sendya/ant-design-pro-vue.git my-project
```

配置淘宝镜像：

```
yarn config set registry https://registry.npm.taobao.org
```

依赖包：
```npm
yarn install 
```

运行：
```npm
yarn run serve
```



## 技术点

## Vue 基础

1. [vuejs guide](https://cn.vuejs.org/v2/guide/)    
2. [vuejs api](https://cn.vuejs.org/v2/api/)

----
1. 模板语法： `v-cloak`
2. 数据绑定
3. 事件绑定
   1. 事件修饰符
4. 属性绑定
5. 样式绑定
6. 指令
7. 计算属性
8. 侦听器
9.  过滤器
10. 传值：
    1.  父子传值：`props`
    2.  非父子：事件中心 `var eventHub=new Vue()`  `eventHub.$on('add-todo',addTodo)`  `eventHub.$off('add-todo')`
11. 组件插槽：
    1.  具名插槽
    2.  作用域插槽：父组件对子组件内容进行加工处理`slot-scope`废弃->`v-slot`

## 前端交互

1. Promise
2. fetch
3. axios
4. async await

## Vue 路由 Vue-router

1. [Vue Router](https://router.vuejs.org/zh/) ,Vue 官方路由管理器。
2. [数据获取](https://router.vuejs.org/zh/guide/advanced/data-fetching.html)
3. [滚动行为](https://router.vuejs.org/zh/guide/advanced/scroll-behavior.html)
4. [路由懒加载](https://router.vuejs.org/zh/guide/advanced/lazy-loading.html)
----
1. HTML5历史模式或`hash`模式
   1. html ` <router-link to="/foo">Go to Foo</router-link>` ;  `<router-view></router-view>`
   2. `{path:'/user',component:User}`
   3. 重定向：`{path:'/',redirect:'/user'}`
2. 嵌套路由 `{path:'',component:Register,children:[{...},{...}]}`
3. 动态路由匹配:
   1. `{path:'/user/:id',component:User}`
   2. `$route.params.id` 
4. 路由参数:
   1. 字符串：`{path:'/user/:id',component:User,props:true}` ; `props:['id']`
   2. 对象(静态)：`{path:'/user/:id',component:User,props:{name:'xx',age:20}}`  `propos['id','name','age']`
   3. 函数：`props:route=>{ {id:route.params.id,name:'xx',age:20} }`
5. 命名路由：`{path:'/user/:id',component:User,name:'user'}` ; `<router-link :to="{name：'user',params:{id：123}}">User</router-link>`
6. 编程式路由：
   1. `this.$router.push('hash地址')`
   2. `this.$router.go(n)`
   3. `router.push({name:'/user',params:{userId:123}})`
   4. `router.push({name:'/register',query:{name:123}})` ->`/register?name=123`


## Vue 单文件组件

组成结构：
+ template
+ script
+ style

## Vue 脚手架

[vue cli](https://cli.vuejs.org/zh/)

```
npm install -g @vue/cli
vue --version
```

自定义配置

vue.config.js
```js
module.exports = {
   "devServer":{
      "port":8888,
      "open":true
   }
}

```

## [Element-UI](https://element.eleme.cn/#/zh-CN)

+ `npm i element-ui -S`
+ 导入 Element-UI [相关资源](https://element.eleme.cn/#/zh-CN/component/quickstart)

+ `vue-cli-plugin-element`

## Webpack

1. [Webpack 5.0](https://webpack.js.org/concepts/)
2. [Webpack 5.0 configuration](https://webpack.js.org/configuration/)
3. [Webpack 4.43](https://www.webpackjs.com/concepts/)
----

安装：
```
npm install webpack webpack-cli -D
```

自动打包
```
npm install webpack-dev-server -D
```

生成预览页面
```
npm install html-webpack-plugin -D
```

打包处理js中的高级语法：`babel.config.js`
```js
module.exports={
   presets:['@babel/env']
}
}
```

配置：`webpack.config.js`
```js
const path= require('path');

const HtmlWebpackPlugin = require('html-webpack-plugin');
const htmlPlugin= new HtmlWebpackPlugin({
   template:'./src/index.html',
   filename:'index.html'
});

const VueLoaderPlugin=require('vue-loader/lib/plugin');

module.exports = {
    mode:'development', //production development
    entry:'./src/index.js',
    output:{
        path: path.resolve(__dirname, "dist"),
        filename:'bundle.js'
    },
    module:{
      rules:[
       // npm i style-loader css-loader -D
       {test:/\.css$/,use:['style-loader','css-loader']},
       // npm i less-loader less -D
       {test:/\.less$/,use['style-loader','css-loader','less-loader']},
       // npm i sass-loader node-sass -D
       {test:/\.scss$/,use:['style-loader','css-loader','sass-loader']},
       // npm i url-loader file-loader -D
       {test:/\.jpg|png|gif|bmp|ttf|eot|svg|woff|woff2$/,use:'url-loader?limit=10240'},
       // npm i babel-loader @babel/core @babel/runtime @babel/preset-env -D
       {test:/\.js$/,use:'babel-loader',exclude:/node_modules/},
       // npm i vue-loader vue-template-compiler -D
       {test:/\.vue$/,loader:'vue-loader'}
      ]
    },
    plugins:[
       htmlPlugin,
       new VueLoaderPlugin()
    ]
}
```

命令： `package.json`
```js
"script":{
    "build":"webpack -p",
    "dev":"webpack-dev-server --open"
}
```
