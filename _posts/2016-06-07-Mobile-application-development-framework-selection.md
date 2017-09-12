---
layout:     post
title:      移动应用开发框架选型
subtitle:   
date:       2016-06-07
author:     huangqing
header-img: img/post-bg-framework-app.jpg
catalog: true
categories: [framework]
tags:
    - App
    - framework
---



# 移动应用开发框架选型 2016

## 移动开发模式

移动开发主要分为原生模式（Native App）开发，混合模式（Hybrid App）开发，Web App模式。

### 1.1	Web APP

Web App 指采用Html5语言写出的App，不需要下载安装。类似于现在所说的轻应用。生存在浏览器中的应用，基本上可以说是触屏版的网页应用。

#### 优点：
1)	开发成本低

2)	更新快

3)	更新无需通知用户，不需要手动升级

4)	能够跨多个平台和终端。

#### 缺点：

1)	临时性的入口

2)	无法获取系统级别的通知，提醒，动效等等

3)	用户留存率低

4)	设计受限制诸多

5)	体验较差

### 1.2	Hybrid App

Hybrid APP指的是半原生半Web的混合类App。需要下载安装，看上去类似Native App，但只有很少的UI Web View，访问的内容是 Web 。

例如Store里的 新闻类APP，视频类APP普遍采取的是Native的框架，Web的内容。

Hybrid App 极力去打造类似于Native App 的体验，但仍受限于技术，网速，等等很多因素。尚不完美。

#### 优点：

1)	支持多平台访问

2)	手机功能都可访问

3)	适用于应用商店

4)	部分支持离线功能

#### 缺点：

1)	未知的部署时间

2)	用户体验不如本地应用（低端机，特别是android上较为明显）

3)	性能速度较慢（需链接网络）

4)	该技术尚未发展成熟，依然是一门新技术

### 1.3	Native App

Native APP 指的是原生程序，一般依托于操作系统，有很强的交互，是一个完整的App，可拓展性强。需要用户下载安装使用。

#### 优点：

1)	打造完美的用户体验

2)	性能稳定

3)	操作速度快，上手流畅

4)	访问本地资源（通讯录，相册）

5)	设计出色的动效，转场

6)	拥有系统级别的贴心通知或提醒

7)	用户留存率高

#### 缺点：

1)	分发成本高（不同平台有不同的开发语言和界面适配）

2)	维护成本高（例如一款App已更新至V5版本，但仍有用户在使用V2， V3， V4版本，需要更多的开发人员维护之前的版本）

3)	更新缓慢，根据不同平台，提交–审核–上线 等等不同的流程，需要经过的流程较复杂

### 1.4	开发模式对比

![开发模式对比](/images/mobileApp/Dev.jpg)

### 1.5	开发模式选择

如果技术实力雄厚，小组具备ios、android等平台人才，且开发周期充足，可以原生模式。

纯web模式交互性比较差，尤其涉及平台底层api，会遇到技术瓶颈。

混合模式，通过webview渲染界面，通过接口调用本地方法,这样就可以使用html、javascript、css编写程序，并通过js调用系统的底层api，技术门槛低，过度依赖某一产品或框架值得注意，在选型时注意框架的性能。

## 2	技术选型

通过实践，现有的移动App开发框架具有以下的不足：

1)	使用的是jquery mobile 框架，这个框架过于臃肿、性能低下、扩展性低，对于移动应用的用户体验性的考虑基本没有。

2)	没有模块化设计，代码比较散乱

3)	流程设计上不太符合移动用户的操作习惯

4)	UI设计上让用户感觉不太像app应用

5)	移动设备的一些特性不支持，比如滑动手势。

结合在移动开发模式上使用的技术方案、资源消耗、产品发布周期。建议仍然使用Hybrid App 开发模式。但在实现的框架、技术选型上一定要注意以下几点：

1)	用户的体验性

2)	高性能

3)	开源、免费

4)	模块化

5)	跨平台

6)	完善的开发文档

7)	高效、可靠的框架维护团队

8)	需要掌握的技术栈

9)	可调试

## 3	Hybrid App 开发框架

目前流行的移动开发框架有以下几个：jquery mobile、sencha touch、ionic、mui。

### 3.1	jquery mobile

jQuery Mobile UI更像是web UI。通过实践， jquery mobile效率不高，过于臃肿，没有模块化的概念，特别是在用户体验性、性能、动画方面有巨大的缺陷。不推荐使用。

![jquery mobile](/images/mobileApp/jquery_mobile.png)
 
### 3.2	sencha touch

sencha touch，组件丰富，有ext知识背景的接受起来比较容易。但企业应用是收费的。暂不考虑使用，不进行详细说明。

![sencha touch](/images/mobileApp/sencha_touch.png)
 
### 3.3	ionic

ionic 基于cordova,采用angularjs实现前后台交互逻辑,并且提供移动定制界面页签。性能高效，使用虚拟DOM渲染、硬件加速、触摸手势优化等技术。提供移动优化HTML，设计美观的CSS和JS组件库组件。命令打包app应用，实现一次开发，处处部署。

![ionic](/images/mobileApp/ionic.jpg)

 
|是否免费|	是	|遵循MIT协议
|----	|----	|----	|
|开发模式   |hybird	|       |
|是否跨平台  |	是	|     |
|社区活跃度： |	活跃	| |
|是否支持android|	是	| |
|是否支持IOS|	是	| |
|开发环境	|文本编辑webstorm，三方工具需破解；vscode；图形拖拽编辑ionic creater；没有集成开发环境	|
|开发语言	|typescript、angularjs、ionic标签	|
|是否支持打包	|是|	命令行输入:ionic build android ，ionic build ios|
|是否支持在线调试|	是|	ionic run配合Google devtool（需翻墙）|
|是否支持模拟器调试|	是|	ionic emulate配合Google devtool（需翻墙）|
|是否支持浏览器调试|	是	|命令行输入ionic serve|
|使用案例|	http://showcase.ionicframework.com/ ||
|在线示例|	http://codepen.io/ionic ||


### 3.4	mui

mui，基于html5+和native.js并结合自身前端js封装和css库提供了一整套实现方案，同时提供了集成开发环境H5 builder，国产软件，文档还算丰富，并提供了大量实现示例。

|是否免费|	是	|遵循MIT协议|
|----	|----	|----	|
|开发模式	|hybird	||
|是否跨平台	|是	|
|社区活跃度： |	比较活跃	||
|是否支持android	|是	||
|是否支持IOS|	是	||
|开发环境|	集成开发环境Hbuilder，集成度很高，但登陆、新建工程需要注册账号，打包需要上传云服务	||
|开发语言	|dom操作、mui封装方法、mui css	||
|是否支持打包	|是	|Hbuilder打包 需上传至mui云服务器|
|是否支持在线调试	|是|	Hbuilder配合Google devtool（需翻墙）|
|是否支持模拟器调试|	是	|Hbuilder配合Google devtool（需翻墙）|
|是否支持浏览器调试|	部分支持	大部分页面不支持，所以基本不可用| |
|使用案例	|http://ask.dcloud.net.cn/docs/ ||


### 3.5	react native

react native facebook 推出的移动开发框架。其实现的思路和方式实质超越了hybird移动开发方案。框架中提供分别针对ios和android的原生组件api，基于javascript和react 开发具有原生应用效果的app应用。理念是：学习一次，处处使用。

|是否免费	|是|	遵循MIT协议|
|----	|----	|----	|
|开发模式	|hybird	||
|是否跨平台|	是	||
|社区活跃度： |	活跃	||
|是否支持android|	是	||
|是否支持IOS|	是	||
|开发环境	|文本编辑webstorm、vscode 没有集成开发环境	||
|开发语言|	react、javascript	||
|是否支持打包|	是	||
|是否支持在线调试|	是	||
|是否支持模拟器调试|	是	||
|是否支持浏览器调试|	是	||
|使用案例	|http://facebook.github.io/react-native/docs/getting-started.html  ||

### 3.6	xamarin

xamarin，微软提供的移动应用开发解决方案。可以使用C#语言编写应用代码，一套代码生成原生的ios、android、windows app应用。2016年4月开始，正在逐步开源中。社区生态在初步发展阶段，鉴于微软移动应用上的不成熟和滞后，此方案的效果有待开发者社区进一步验证。
