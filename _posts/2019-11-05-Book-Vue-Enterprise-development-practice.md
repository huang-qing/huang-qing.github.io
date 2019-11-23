---
layout:     post
title:      Vue企业开发实战
subtitle:   
date:       2019-11-05
author:     huangqing
header-img: img/post-bg-vue.png
catalog: true
categories: [Vue]
tags:
    - Vue   
    - Book
---


## 第1章 项目架构设计

技术栈:

+ 自动化工具（Gulp、Grunt）
+ 前端组件化框架（Vue.js、React.js）
+ 前端工程化（一套技术思想）
+ 前端模块化（SeaJS、RequireJS、ECMAScript）

选型：Vue.js+Webpack

+ Node
+ 包管理工具NPM，淘宝镜像CNPM（`npm install -g cnpm --registry=https://registry.npm.taobao.org`）
+ Vue-cli安装及初始化vue项目。创建并启动一个带热重载、保存时静态检查，以及可用于生产环境构建配置的项目。
  + 全局安装Vue-cli：`cnmp install vue-cli -g`
  + 全局安装Webpack：`cnpm install webpack -g`
  + 创建vue项目：`vue init webpack <项目名称>`
  + 运行命令：`npm run dev`
+ UI框架iView
  + 安装iView：`cnpm install iview --save`
  + 引入iView：
    + `import iView from 'iview';`
    + `import 'iview/dist/styles/iview.css'`
    + `Vue.use(iView)`
    + babel-plugin-import插件可以从组建库中引入需要的模块
+ 路由vue-router
  + 支持嵌套路由、组件惰性载入、视图切换动画、具名路径
  + `cnpm install vue-router --save`
  + `import Router form 'vue-router'`
  + `Vue.use(Router)`
+ 数据共享，VueX
+ ECMAScript6语法
+ axios插件发送异步请求
  + `cnpm install axios --save`
+ 前后端完全分离，后台数据通过Mock数据进行模拟

MVVM分层架构方式，优点在于:易维护、可扩展、易复用、灵活性高。

模块化：解决一个复杂问题是自顶向下逐层把系统分成若干模块的过程，有多种反应其内部特性，同事模块化还可以解耦实现并行开发。
主要模块化解决方案有：AMD（requierjs）、CMD（seajs）、CommonJS、ES6。模块化用来分割、组织和打包软件。

![](/images/vue/2019-11-05_214115.png)

完全分离方式：
1. 分离开发集成部署
2. 分离开发分离部署。前端使用纯HTML通过接口的方式进行数据交互，降低系统的复杂度，部署时单独部署到一台服务器上，使用代理进行数据的交互。

`npm install` 中 `--save` 与 `--save-dev`的区别
+ `--save`会把依赖包添加到`package.json`文件`dependencies`下
+ `--save-dev`会把依赖包添加到`package.json`文件`devDependencies`下
+ dependencies是产品上线运行时的依赖，devDependencies是产品开发时的依赖。

构建工具Webpack:

1. 解决JavaScript和CSS的依赖问题
2. 性能优化：文件合并和文件压缩
3. 效率提升：添加CSS3前缀。将多种静态资源js、css、less转化成一个静态文件。
  
## 第2章 ES6的使用

![](/images/vue/2019-11-05_223015.png)

## 第3章 路由配置

![](/images/vue/2019-11-05_224252.png)

`history` 是HTML5新增的API，可以用来操作浏览器的`session history`

路由懒加载：结合Vue的异步组件和Webpack的代码分割功能，实现路由组件的懒加载。

## 第4章 Vue.js

![](/images/vue/2019-11-05_224355.png)

Visual Studio Code 编辑器中，书写.vue文件也需要安装对应的插件（`Vetur`）来增加对文件的支持。

style标签中CSS样式，属性scoped表明这里写的CSS样式只适用于该组件，限定样式的作用域。

在.vue组件中，data必须是一个函数，它返回一个对象，这个对象数据供组件实现。

生命周期函数：
1. beforeCreate（创建前）：组件实例刚刚被创建，组件属性计算之前，比如data属性等
2. created（创建后）：组件实例刚创建完成，属性已经绑定，此时DOM还未生成，$le属性还不存在
3. beforeMount（载入前）：模板编译、挂载之前
4. mounted（载入后）：模板编译、挂载之前
5. beforUpdate（更新前）：组件更新之前
6. updated（更新后）：组件更新之后
7. beforeDestroy（销毁前）：组件销毁前调用
8. destroyed（销毁后）：组件销毁后调用

>控制台输出分组 console.group("-----")

## 第5章 服务端通信

![](/images/vue/2019-11-05_230219.png)

connect-mock-middleware
+ 支持mockJs语法
+ 支持json、jsonp
+ 修改mock数据不需要重新加载

Mock.js
+ 根据数据模板生成模拟数据
+ 模拟AJAX请求，生成并返回模拟数据
+ 基于HTML模板生成模拟数据

snail mock
+ snail mock 能够模拟服务功能，生成接口的url服务地址供调用
+ `cnpm install -g snail-cline`
+ `snail mock`

Axios基本介绍：Axios是一个基于Promise、用于浏览器和node.js的HTTP客户端，常用于处理AJAX请求。
+ 从浏览器中创建XmLHttpRequest
+ 从node.js发出http请求
+ 支持Promise API
+ 拦截请求和响应
+ 转换请求和响应数据
+ 取消请求
+ 自动转换JSON数据
+ 客户端可以防止CSRF/XSRF(伪造站点请求)恶意请求

## 第6章 Vue.js指令

![](/images/vue/2019-11-06_103654.png)

key属性：尽可能在使用`v-for`指令时提供key属性，除非遍历输出的DOM内容非常简单。
```html
{% raw %}
<li v-for="(item,index) in lesson" :key="index">
  {{index}} - {{item.name}} - {{item.type}}
</li>
{% endraw %}
```

修饰符：
+ `.stop`：调用event.stopPropagation()
+ `.prevent`:调用event.preventDefault()
+ `.self`:当事件是从侦听器绑定的元素本身触发时才触发回调
+ `.{keycode}`:只在指定键上触发回调

## 第7章：组件详解

![](/images/vue/2019-11-06_112419.png)

## 第8章：计算属性和侦听器

![](/images/vue/2019-11-06_112728.png)

## 第9章：插件的使用

![](/images/vue/2019-11-06_113118.png)

## 第10章：项目总结

![架构设计](/images/vue/2019-11-06_113435.png)

![ES6的使用](/images/vue/2019-11-06_113549.png)

![Vue框架](/images/vue/2019-11-06_113634.png)

![项目框架搭建及配置](/images/vue/2019-11-06_113709.png)

![目录结构](/images/vue/2019-11-06_113805.png)

![公共技能点](/images/vue/2019-11-06_113856.png)




