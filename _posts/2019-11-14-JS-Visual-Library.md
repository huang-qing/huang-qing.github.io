---
layout: post
title: JavaScript数据可视化库
subtitle: 
date: 2019-11-14
author: huangqing
header-img: img/post-bg-javascript.gif
catalog: true
categories: [JavaScript]
tags:
  - javascript
---

## D3.js

star 数：88K

D3.js 可能是最流行和使用最广泛的 JavaScript 数据可视化库。D3 用于基于数据的文档操作，并使用 HTML、SVG 和 CSS 让数据活起来。它基于 Web 标准，结合现代浏览器，不需要与专有框架耦合，将可视化组件和数据驱动的方法结合到 DOM 操作上。它允许你将任意数据绑定到文档对象模型（DOM），然后在文档上应用数据转换。

这里有一个很棒的例子 ：https://github.com/d3/d3/wiki/Gallery

项目地址：https://github.com/d3/d3/

## ThreeJS

star 数：56K

ThreeJS 用 WebGL 创建 3d 动画。该项目的灵活性和抽象性意味着它也可用于 2 维或 3 维的数据可视化。

例如，你可以使用这个指定的模块（https://github.com/davidpiegza/Graph-Visualization）和 WebGL 进行 3D 图形可视化，或者尝试在线试验（https://www.steema.com/files/public/teechart/html5/latest/demos/canvas/webgl/threejs_example.htm）。

项目地址：https://github.com/mrdoob/three.js/

## ChartJS

star 数：45K

一个非常受欢迎的开源 HTML5 图表库，它使用画布元素构建响应式 Web 应用。ChartJS 提供了混合图表类型，新的图表轴类型和漂亮的动画。它的设计简单而优雅，有 8 种基本的图表类型，你可以将该库与 moment.js 结合在一起使用，用于渲染时间轴。

项目地址：https://cdnjs.com/libraries/Chart.js

## Echarts & Highcharts

star 数：37K

百度的 Echarts 项目是一个基于浏览器的交互式图表和可视化库。它是用纯 JavaScript 编写的，基于 zrender 画布。它支持以画布、SVG(4.0+) 和 VML 的形式绘制图表。除了 PC 和移动浏览器外，ECharts 还可以与 node 上的 node-canvas 一起使用，以便进行高效的服务器端渲染（SSR）。

这里是完整的示例库：

http://echarts.baidu.com/echarts2/doc/example-en.html

项目地址：

https://github.com/apache/incubator-echarts

star 数：9K

Highcharts JS 是一个广受欢迎的基于 SVG 的 JavaScript 图表库，针对旧的浏览器可降级到 VML 和画布。世界上最大的 100 家公司中，有 72 家公司（Facebook、Twitter 等）使用了这个库，这使得它成为世界上最流行的 JS 图表 API。

项目地址：

https://github.com/highcharts/highcharts

## Metric-Graphi

star 数：7K

Metric-Graphics 用于可视化和布局时间序列数据。它相对较小（80kb），提供了小而优雅的线条图、散点图、直方图、柱状图和数据表，以及地格图（rug plot）和基本线性回归等特性。

这里有一个交互式的示例库：

https://metricsgraphicsjs.org/examples.htm

项目地址：

https://github.com/metricsgraphics/metrics-graphics


## Raphael

star 数：10K

Raphael 是一个 JavaScript 矢量库，可在 Web 中绘制矢量图形。 该库使用 SVG W3C 和 VML 作为创建图形的基础，因此每个图形对象也是 DOM 对象，你可以附加 JavaScript 事件处理程序。 Raphael 目前支持 Firefox 3.0+、Safari 3.0+、Chrome 5.0+、Opera 9.5+ 和 Internet Explorer 6.0+。

项目地址：

https://github.com/DmitryBaranovskiy/raphael

## React

### Recharts

star 数：12K

Recharts 是一个使用 React 和 D3 构建的图表库，可以作为声明性的 React 组件使用。该库提供原生 SVG 支持，轻量级依赖树（D3 子模块）高度可定制。官网文档中可以找到很多例子。

项目地址：

https://github.com/recharts/recharts

### React Vis

star 数：6K

React-vis 是 Uber 开发的一系列数据可视化组件，包括线 / 面 / 柱状图、热图、散热图、等高线图、六角热图等等。使用该库不需要事先掌握 D3 或任何其他 data-vis 库的知识，并提供了低级模块化的构建块组件，如 x/y 轴。

项目地址：https://github.com/uber/react-vis

### React Virtualized

star 数：17K

React virtualized 是一组 React 组件，有效地呈现大型列表和表格数据。ES6、CommonJS 和 UMD 版本可以在每个分发版中使用，该项目支持 Webpack 4 工作流。请注意，为了避免版本冲突，必须将 react，react-dom 指定为 peer 依赖。

项目地址：https://github.com/bvaughn/react-virtualized

### Victory

star 数：7K+

Victory 在 Web 和 React Native 应用程序中使用相同的 API，以便于跨平台绘制图表。一种优雅而灵活的方式来利用 React 组件来支持实用的数据可视化。

项目地址：

https://github.com/FormidableLabs/victory

## CartoDB

star 数：2K

Carto 是一个位置智能和数据可视化工具，用于发现位置数据中的见解。你可以通过 Web 表单上传地理空间数据（Shapefiles、GeoJSON 等），并在数据集或地图上将其可视化，使用 SQL 进行搜索，并使用 CartoCSS 来应用地图样式。

这里有一些视频演示：https://vimeo.com/channels/carto

项目地址：https://github.com/CartoDB/cartodb

## Raw graphs

star 数：6K

Raw 是电子表格和数据可视化之间的连接，基于 d3.js 库创建基于向量的自定义可视化。它可以处理表格数据（电子表格和 CSV）和从其他应用程序复制和粘贴的文本。因为是 SVG 格式，所以可以使用矢量图形编辑器编辑，或直接嵌入到网页中。

这里有一个例子：https://rawgraphs.io/gallery/

项目地址：

https://github.com/densitydesign/raw

## Metabase

Metabase 是一种相当快速和简单的方法，可以在不了解 SQL 的情况下创建数据仪表盘（分析师和数据专业人士可使用 SQL 模式）。你可以创建片段和度量指标，发送数据到 Slack（通过 MetaBot 在 Slack 中查看数据）等等。它可能是一个很好的工具，可用它在团队内部可视化数据，尽管可能需要做一些维护工作。

项目地址：

https://github.com/metabase/metabase


