---
layout: post
title: Web 性能优化
subtitle: 
date: 2020-04-01
author: huangqing
header-img: img/post-bg-javascript.gif
catalog: true
categories: [JavaScript]
tags:
  - javascript
---

## Web性能优化分类

1. 度量标准
2. 编码优化
3. 静态资源优化
4. 交付优化
5. 构建优化
6. 性能监控

![performance](/images/performance/web-performance-optimization.jpg)

## 度量标准与设定目标

度量标准

+ 首次有效绘制（First Meaningful Paint，简称FMP，当主要内容呈现在页面上）
+ 英雄渲染时间（Hero Rendering Times，度量用户体验的新指标，当用户最关心的内容渲染完成）
+ 可交互时间（Time to Interactive，简称TTI，指页面布局已经稳定，关键的页面字体是可见的，并且主进程可用于处理用户输入，基本上用户可以点击UI并与其交互）
+ 输入响应（Input responsiveness，界面响应用户输入所需的时间）
+ 感知速度指数（Perceptual Speed Index，简称PSI，测量页面在加载过程中视觉上的变化速度，分数越低越好）
+ 自定义指标，由业务需求和用户体验来决定。


设定目标

+ 100毫秒的界面响应时间与60FPS
+ 速度指标（Speed Index）小于1250ms
+ 3G网络环境下可交互时间小于5s
+ 重要文件的大小预算小于170kb

以上四种指标的设定都有据可循。详细信息请查看RAIL性能模型。

## 编码优化

编码优化涉及到应用的运行时性能

### 数据读取速度

+ 字面量与局部变量的访问速度最快，数组元素和对象成员相对较慢
+ 变量从局部作用域到全局作用域的搜索过程越长速度越慢
+ 对象嵌套的越深，读取速度就越慢
+ 对象在原型链中存在的位置越深，找到它的速度就越慢

>推荐的做法是缓存对象成员值。将对象成员值缓存到局部变量中会加快访问速度

### DOM

+ 在JS中对DOM进行访问的代价非常高.减少访问DOM的次数(建议缓存DOM属性和元素、把DOM集合的长度缓存到变量中并在迭代中使用)
+ 重排与重绘的代价非常昂贵。如果操作需要进行多次重排与重绘，建议先让元素脱离文档流，处理完毕后再让元素回归文档流，这样浏览器只会进行两次重排与重绘（脱离时和回归时）
+ 善于使用事件委托

### 流程控制

+ 避免使用`for...in`（它能枚举到原型，所以很慢）
+ 在JS中倒序循环会略微提升性能
+ 减少迭代的次数
+ 基于循环的迭代比基于函数的迭代快8倍
+ 用`Map`表代替大量的if-else和switch会提升性能

## 静态资源优化

+ 使用`Brotli`或`Zopfli`进行纯文本压缩
+ 图片优化:尽可能通过`srcset`，`sizes`和`<picture>`元素使用*响应式图片*。还可以通过`<picture>`元素使用WebP格式的图像。

## 交付优化

+ 异步无阻塞加载JS.将Script标签放到页面的最底部;无阻塞加载JS的方法：`defer`、`async`、动态创建`script`标签、使用XHR异步请求JS代码并注入到页面
+ 使用`Intersection Observer`实现懒加载
+ 优先加载关键的CSS
+ 将首屏渲染必须用到的CSS提取出来内嵌到`<head>`中，然后再将剩余部分的CSS用异步的方式加载。可以通过Critical做到这一点。
+ 资源提示（Resource Hints）.Resource Hints（资源提示）定义了HTML中的Link元素与`dns-prefetch`、`preconnect`、`prefetch`与`prerender`之间的关系。
+ Preload:快速响应的用户界面.要严格限制每个JS任务执行时间不能超过100毫秒

## 构建优化

项目在构建过程中是否进行了合理的优化，会对Web应用的性能有着巨大的影响。例如：影响构建后文件的体积、代码执行效率、文件加载时间、首次有效绘制指标等。

+  使用预编译
+  使用 Tree-shaking、Scope hoisting、Code-splitting
+  服务端渲染（SSR）.服务端渲染（Server Side Render，简称SSR）的意义在于弥补主要内容在前端渲染的成本，减少白屏时间，提升首次有效绘制的速度。可以使用服务端渲染来获得更快的首次有效绘制。
+  使用`import`函数动态导入模块
+  使用HTTP缓存头.正确设置`expires`，`cache-control`和其他HTTP缓存头;推荐使用`Cache-control: immutable`避免重新验证。

## 其他

+ HTTP2
+ 使用最高级的CDN（付费的比免费的强的多）
+ 优化字体
+ 其他垂直领域的性能优化

