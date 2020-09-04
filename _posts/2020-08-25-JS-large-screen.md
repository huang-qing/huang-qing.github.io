---
layout:     post
title:      如何挑选数据可视化框架及平台
subtitle:   
date:       2020-08-25
author:     huangqing
header-img: img/post-bg-javascript.jpg
catalog: true
categories: [JavaScript]
tags:
    - javascript 
    - 可视化
---


数据可视化分成两种大的类型：

可视化库，比如 D3、Echarts 等，这些是面向开发者的，开发者可以基于这些库开发可视化应用。

可视化平台，比如 Grafana、Superset 等，这些面向分析师和一般使用者，直接拿来分析数据，无需开发。


## 如何快速了解一个项目？

于开源库的选择，除了功能是否能满足，更重要的是要看这个项目本身的发展情况，我最喜欢的方法是看 GitHub 的 `contributors` 页面，在这里能看出这个项目是否活跃

运行 `cloc` 看代码行数，运行一下就能得到两个重要信息，一个是代码的语言分布，知道这个项目主要用什么语言写的，另一个是代码规模，很多项目看 Star 数量和首页似乎差距不大，一算代码行数就发现有数量级的区别，代码数量太少的肯定功能有限。

## D3 - 可视化界的 jQuery

ploty.js 在 2012 年就有了，一直都有更新，从功能和项目活跃度看是最好的，代码规模有 9w 多行，远超前面几个，它背后是一家公司，这家公司除了 ploty.js 还有给分析师用的 Dash 应用产品，我们将在下一篇里介绍。

论功能丰富度 `ploty.js` > `nivo` > `vx`，其它都没必要看了，D3 虽然很火，但在专业图表领域能拿得出手的其实只有 ploty.js，而 ploty.js 也并不是只用 D3，它还依赖了 gl-plot2d、regl 等基于 WebGL 的库，因为 D3 的接口基于 DOM，用来操作 SVG 没问题，但无法用来操作 Canvas 和 WebGL，这使得 D3 无法用于大数据量及需要像素级别操作的特效。

总体来说 D3 是一款成功的可视化基础设施，它的源码值得学习，但不推荐直接使用它例子开发图表，一方面基于它写的代码上手门槛高，另一方面是它的定位并非图表库，要用的话最好是其它基于 D3 开发的图表库，比如 Ploty.js。

## ECharts - 穷人的 Highcharts

Echarts 和 Highcharts 的 API 非常像，很多公司其实是当成了免费的 Highcharts 来用，但 Echarts 其实有不少自己独特的功能，其中最具亮点的就是 Echarts-gl，它能实现三维图表和地球的展示，这点其他开源库基本没有，而其它商业图表都是基于 SVG 的，只能弄仿三维效果，只有 Echarts-gl 是基于 WebGL 做的真三维，可以配置光照、材质、阴影等，还有独家的后期特效功能。

## Vega - 图表库也能低代码

## G2/F2 - 《The Grammar Of Graphics》的追随者

《The Grammar Of Graphics》里作者主要是借鉴了面向对象的思想，将图表拆分成了很多组成部分，比如数据源、数据转换、坐标系、图形等，通过将它们组装起来实现各种展现效果，从技术角度倒是不新奇，但这本书通过数学的方式来进行组合和分析，看起来非常高级。

## Chart.js - 顶级推广案例

## Highcharts - 小镇里的世界级组件

## AnyChart - 闷声赚钱的图表库

## amCharts - 唯一可以免费商用的商业图表库

## Google Chart - 图表库的云服务

## 小结

虽然有很多开源图表库，但绝大部分不是已经弃坑就是将要弃坑了，目前真正能用的就只有 ECharts、Ploty.js、Vega 和 G2。
