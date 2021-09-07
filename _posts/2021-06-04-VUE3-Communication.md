---
layout:     post
title:      Vue 组件通信的方式
subtitle:   
date:       2021-06-04
author:     huangqing
header-img: img/post-bg-vue.png
catalog: true
categories: [Vue]
tags:
    - Vue   
---

![](/images/vue/vue-communication-8.jpg)


## `props` 父组件 向 子组件 传递值 

1. 在父组件中引入子组件
2. 注册子组件
3. 在页面中使用，子组件标签上 动态绑定传入动态值 / 静态值
4. 在子组件中，使用 `props` 来接受 父组件 传递过了的值


子组件接收的父组件的值分为引用类型和普通类型两种：

+ 普通类型：字符串（String）、数字（Number）、布尔值（Boolean）、空（Null）
+ 引用类型：数组（Array）、对象（Object）

>由于 Vue 是 单向数据流， 子组件 是不能直接 修改 父组件 的 值
>组件中的数据共有三种形式：data、props、computed

## `emit` 子组件 向父组件传递值

子组件通过绑定事件，通过 `this.$emit('函数名'，传递参数)`

## 父组件 通过 `$refs` / `$children` 来获取子组件值

`$refs` :

+ 获取DOM 元素 和 组件实例来获取组件的属性和方法。
+ 通过在 子组件 上绑定 `ref` ，使用 `this.$refs.refName.子组件属性/子组件方法`
  
`$children` :

+ 当前实例的子组件，它返回的是一个子组件的集合。如果想获取哪个组件属性和方法，可以通过 `this.$children[index].子组件属性/方法`


## 子组件 通过 `$parent` 来获取父组件实例的属性和方法

## 使用 `$attrs` 和 `$listeners` （组件嵌套）

组件嵌套情况下使用，`$attrs` 和 `$listeners`(Vue2) 获取父组件实例属性和方法

+ `$attrs` ：包含了父作用域中不被 prop 所识别 (且获取) 的特性绑定 (class 和 style 除外)。当一个组件没有声明任何 prop 时，这里会包含所有父作用域的绑定 (class 和 style 除外)，并且可以通过 `v-bind="$attrs"` 传入内部组件。通常配合 `interitAttrs` 选项一起使用。
+ `$listeners` ：包含了父作用域中的 (不含 .native 修饰器的) `v-on` 事件监听器。它可以通过 `v-on="$listeners"` 传入内部组件

简单来说：`$attrs`与`$listeners` 是两个对象，`$attrs` 里存放的是父组件中绑定的非 `Props `属性，`$listeners`里存放的是父组件中绑定的非原生事件。

## `$emit`/`$on`中央事件总线,跨组件之间传值

通过一个空的Vue实例作为中央事件总线（事件中心），用它来触发事件和监听事件,巧妙而轻量地实现了任何组件间的通信，包括父子、兄弟、跨级。

```js
var Event=new Vue();
Event.$emit(事件名,数据);
Event.$on(事件名,data => {});
```

## `provide` 和 `inject` 实现父组件向子孙孙组件传值

`provide` 和 `inject` 这对选项需要一起使用，以允许一个祖先组件向其所有子孙后代注入一个依赖，不论组件层次有多深，并在起上下游关系成立的时间里始终生效。

`provide` :

+ 是一个对象或返回一个对象的函数
+ 该对象包含可注入其子孙的属性。

`inject` :

+ 是一个字符串数组 或者是一个对象
+ 用来在子组件或者子孙组件中注入 `provide` 提供的父组件属性。

## Vuex

## 参考

+ [Vue 组件通信的 8 种方式](https://segmentfault.com/a/1190000040390222)