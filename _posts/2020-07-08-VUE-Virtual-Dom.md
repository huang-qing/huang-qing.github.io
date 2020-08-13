---
layout:     post
title:      Vue - Virtual Dom
subtitle:   
date:       2020-07-08
author:     huangqing
header-img: img/post-bg-vue.png
catalog: true
categories: [Vue]
tags:
    - Vue   
---

## 模板转换成视图的过程

+ Vue.js通过编译将template 模板转换成渲染函数(render ) ，执行渲染函数就可以得到一个虚拟节点树

+ 在对 Model 进行操作的时候，会触发对应 Dep 中的 Watcher 对象。Watcher 对象会调用对应的 update 来修改视图。这个过程主要是将新旧虚拟节点进行差异对比，然后根据对比结果进行DOM操作来更新视图。

在Vue的底层实现上，Vue将模板编译成虚拟DOM渲染函数。结合Vue自带的响应系统，在状态改变时，Vue能够智能地计算出重新渲染组件的最小代价并应到DOM操作上。

![](/images/vue/vue-virtual-dom.png)

+ **渲染函数**：渲染函数是用来生成Virtual DOM的。Vue推荐使用模板来构建我们的应用界面，在底层实现中Vue会将模板编译成渲染函数，当然我们也可以不写模板，直接写渲染函数，以获得更好的控制。

+ **VNode 虚拟节点**：它可以代表一个真实的 dom 节点。通过 createElement 方法能将 VNode 渲染成 dom 节点。简单地说，vnode可以理解成节点描述对象，它描述了应该怎样去创建真实的DOM节点。

+ **patch(也叫做patching算法)**：虚拟DOM最核心的部分，它可以将vnode渲染成真实的DOM，这个过程是对比新旧虚拟节点之间有哪些不同，然后根据对比结果找出需要更新的的节点进行更新。这点我们从单词含义就可以看出， patch本身就有补丁、修补的意思，其实际作用是在现有DOM上进行修改来实现更新视图的目的。Vue的Virtual DOM Patching算法是基于`Snabbdom`的实现，并在些基础上作了很多的调整和改进。

## Virtual DOM

Virtual DOM 其实就是一棵以 JavaScript 对象( VNode 节点)作为基础的树，用对象属性来描述节点，实际上它只是一层对真实 DOM 的抽象。

以把Virtual DOM 理解为一个简单的JS对象，并且最少包含标签名( tag)、属性(attrs)和子元素对象( children)三个属性。


## diff

Vue的diff算法是基于snabbdom改造过来的，**仅在同级的vnode间做diff，递归地进行同级vnode的diff，最终实现整个DOM树的更新**。因为跨层级的操作是非常少的，忽略不计，这样时间复杂度就从O(n3)变成O(n)。

![](/images/vue/vue-diff.png)

diff 算法包括几个步骤：

+ 用 JavaScript 对象结构表示 DOM 树的结构；然后用这个树构建一个真正的 DOM 树，插到文档当中

+ 当状态变更的时候，重新构造一棵新的对象树。然后用新的树和旧的树进行比较，记录两棵树差异

+ 把所记录的差异应用到所构建的真正的DOM树上，视图就更新了