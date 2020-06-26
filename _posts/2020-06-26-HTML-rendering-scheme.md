---
layout:     post
title:      前端页面渲染方案
subtitle:   CSR、SSR、NSR、ESR
date:       2020-06-26
author:     huangqing
header-img: img/post-bg-html.jpg
catalog: true
categories: [HTML]
tags:
    - HTML
---


Google不断在推进新的性能相关指标，从原先的`Performance API`中的指标逐步演进成用户性能体验相关的指标。

![](/images/htmls5/rendering.jpg)

对于用户而言，First Paint、First Meaningful Paint和TTI这几个指标可以直接影响到用户体验。

## CSR(浏览器渲染)：Client Side Rendering 

![](/images/html5/csr.png)

浏览器(Client)渲染顾名思义就是所有的页面渲染、逻辑处理、页面路由、接口请求均是在浏览器中发生。其实，*现代主流的前端框架均是这种渲染方式*，这种渲染方式的好处在于实现了前后端架构分离，利于前后端职责分离，并且能够首次渲染迅速有效减少白屏时间。同时，*CSR可以通过在打包编译阶段进行预渲染或者骨架屏生成*，可以进一步提升首次渲染的用户体验。

但是由于和服务端会有多次交互（获取静态资源、获取数据），同时依赖浏览器进行渲染，在移动设备尤其是低配设备上，首屏时间和完全可交互时间是比较长的

## SSR(服务端渲染)：Server Side Rendering

![](/images/html5/ssr.jpg)

服务端渲染则是在服务端完成页面的渲染，在服务端完成页面模板、数据填充、页面渲染，然后将完整的HTML内容返回给到浏览器。由于所有的渲染工作都在服务端完成，因此网站的*首屏时间和TTI都会表现比较好*。

但是，渲染需要在服务端完成，并不能很好进行前后端职责分离，而且白屏时间也会比较长，同时，对于服务端的负载要求也会比较高

## 基于Hydration的SSR和CSR融合

![](/images/html5/hydration-ssr-csr.jpg)

业界提出前后端渲染同构的方案来整合SSR和CSR。整个页面的加载和刷新是通过服务端渲染来实现，在渲染生成的HTML中内嵌JavaScript和数据内容。通过这样的实现，可以达到和SSR相同的首屏时间，并且基于Hybration，可以生成前端的虚拟Dom，避免前端触发二次渲染。

SSR和CSR的页面渲染体验对比：

![](/images/html5/csr-vs-ssr-1.png)

SSR和CSR不同阶段优劣对比：

![](/images/html5/csr-vs-ssr-2.png)


## Serverless SSR

前端使用SSR进行渲染同构有一个非常明显的缺点：服务资源消耗和运维。通常情况下，同构SSR会采用Node服务，需要固定的服务器资源，并且服务器的运维对于前端同学也提出了很高的要求。

![](/images/html5/serverless-ssr.png)

对于前端同学而言，仅仅只需要实现云端函数的实现，对于服务器的运维管理完全不用关心，同时服务器会实现自动弹性伸缩，有效节省了机器资源。

随着Serverless技术的出现，这些问题似乎得到了很好的解决。Serverless基于触发器调用云端部署的函数，然后调用后端服务。


## NSR:Native Side Rendering

![](/images/html5/nsr.png)

UC浏览器在新闻feed流页面加载中采用了NSR（Native Side Rendering），首先在列表页中加载离线页面模板，通过Ajax预加载页面数据，通过Native渲染生成Html数据并且缓存在客户端。

![](/images/html5/nsr-2.png)


## ESR:Edge Side Rendering

![](/images/html5/esr.png)

ESR - Edge Side Rendering，方案的核心思想是，借助边缘计算的能力，将静态内容与动态内容以流式的方式，先后返回给用户。cdn 节点相比于 server，距离用户更近，有着更短的网络延时。在 cdn 节点上，将可缓存的页面静态部分，先快速返回给用户，同时在 cdn 节点上发起动态部分内容请求，并将动态内容在静态部分的响应流后，继续返回给用户。

----
[CSR、SSR、NSR、ESR傻傻分不清楚，一文帮你理清前端渲染方案！](https://mp.weixin.qq.com/s/XGwMVhT1mpC8R72-DQaodA)