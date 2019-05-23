---
layout:     post
title:      2017 年崛起的 JS 项目
subtitle:   
date:       2018-01-22
author:     huangqing
header-img: img/post-bg-javascript.jpg
catalog: true
categories: [JavaScript]
tags:
    - javascript project
---

和 2016年 一样，又到了我们回顾 2017年 Javascript 领域发展与变化的时候。通过对比各项目过去 12 个月在 GitHub 上新增 star 数量，来评估其在 2017 年度的受关注程度，进而选出 2017 年度 JavaScript 领域崛起的明星项目。

下列图表对比了各个项目在 Github 上于过去 12 个月新增的 star 数量。分析的数据来源为 bestof.js.org 网站 ，一个 WEB 领域优秀项目的精选网站。通过点击项目，可以查看更多信息。

## 最受欢迎项目

下面是不分类别的 2017 年度最受欢迎 Javascript 项目，如果你时间很紧，看这部分就够了。

#### Vue.js 蝉联冠军

![](/images/javascript/1610275355-5a65520daa24b_articlex.png)

Vue.js 再次强势登顶年度排行榜冠军，今年在 GitHub 上新增了超过 40K 的 star。相较于 2016 年的（26K star），今年 Vue.js 领先排行榜第 2 名（ React ）的优势更大了。

那么，是什么令 Vue.js 如此出众？

首先，它学习曲线平缓，使用了与 React 相似语法更让 WEB 开发者熟悉的组件方案；

发展良好的生态圈，社区中涌现出事实上的官方标准库：路由 vue-router，状态管理库: Vuex；

把模板、逻辑和样式放入单个 .vue 文件中的单文件组件设计理念在模块化代行其道的今天显得非常亲切；

被流行的 PHP 框架 Laravel 选为默认的视图引擎（View Engine）；
为 Evan You 个人维护，通过众筹方式来获取支持的开源项目，而不是由 Facebook 或 Google 这样的互联网巨头来主导。

也许正因为上述最后一点，Vue.js 在中国拥有大量的拥趸。不仅被中国最大的电商平台阿里巴巴使用，也获得了 GitLab 与 Adobe 这些公司青睐。

#### React 再次获得亚军

React 和 2016 一样稳占第二名，2017 年它在 GitHub 上获得了超过 27K star（再次明确下，此处我们分析的是今年新增的 star 数量而非所有的 star 数量）。

Create React App，是排行榜的季军，已经成为新建 React 项目的首选方式。它的大获成功让不少 React 样板项目（React Boilerplates）慢慢淡出历史舞台。

Dan Abramov（ Redux 作者，现就职于 Facebook）创建 Create React App 的确是做了一件了不起的工作，他在功能性与简洁性之间取得了巧妙的平衡，比如，它没有集成花哨的样式解决方案（只使用了纯粹的 CSS）和服务器端渲染，却具有恰到好处的封装，这些造就了良好的开发体验。

#### Axios

Axios 库是最广泛使用的 HTTP 客户端。它能同时在用户端（在用户端发起 Ajax 请求）与服务器端（在 Node.js 环境中）使用。

Axios 的成功或许与 Vue.js 有比较大的关系，因为大量的 Vue.js 教程都使用它来发起 API 请求获取数据。

#### Puppeteer

Puppeteer 是今年的大事件之一，是 Google Chrome 团队开发的一个无界面 Chrome 浏览器，即一个在后台运行，且能被代码驱动和控制的浏览器。

它可作如下用途:

+ 在真实浏览器中进行自动化界面测试；
+ 用生成页面快照的方式来实现服务端渲染；
+ 利用 Google Chrome “保存为 PDF” 的功能生成PDF文件；


## 前端框架

前端框架方面向来是兵家必争之地，不过如今已呈三家鼎足分立，大局尘埃落定之势。

![](/images/javascript/1344243124-5a65971941f2d_articlex.png)

#### Vue、React、Angular 三足鼎立

毫无疑问，目前公认的 3 大 UI 框架分别是 Vue.js，React 和 Angular 。

我们习惯称他们为框架，但准确地讲只有 Angular 是框架，Vue.js 和 React 应归类为库。

前文中，我们已经分析了 Vue.js 的成功因素和它的集成方案。

与 Vue.js 相对应的，React 方面却依然处于碎片化的状态，开发者需要根据自身项目的情况，进行技术选型：

+ 页面间的路由切换问题；
+ 如何获取数据；
+ 如何把数据绑定到表单；
+ 如何存储应用的状态；

相反，Angular 生态圈则更可控也更稳定。有一种叫Angular 准则 的最佳实践用来指导开发。

这可能给人一种 Angular 对于多人协同工作更友好的印象。此外，随着支持静态类型的 TypeScript 加入，Angular 也势必能得到更多熟悉 C# 或 Java 的后端开发人员人观注与青睐。

## 少即是多

在三巨头之后，能非常有趣的发现第四名 Preact。

Preact 是一个 React 的小型替代解决方案：有相同的 API，却只有 3KB 的大小。

类似的还有许多竞争者，为了区别于三巨头，它们在浏览器的性能上下功夫，努力做出自己的特色。


## Node.js 框架

![](/images/javascript/3930835936-5a659718907af_articlex.png)

JavaScript 已不仅仅局限于 Web 前端应用，也被越来越多的人使用开发后端应用。Node.js 社区颇具有影响力的 Mikeal Rogers 做出了 Node.js 会在一年内超越 Java的预测。

和其他语言都有事实上的标准框架不同的是（如 Ruby 有 Ruby on Rails，Python 有 Django，PHP 有 Laravel），目前基于 Node.js 写服务端程序还没有一个大家都认可的标准框架。

Express 并不是 2017 年度的 Node.js 框架分类排行冠军，毕竟这个项目已经成立多年，但它已转变为许多框架和 CMS 的基础组件，包括 Feathers、Keystone 和 Nest。

Express 的极简主义设计似乎完美地符合了当今微服务理念的发展趋势：把一个大型程序解耦成几个小的应用。

与去年相比, 今年有 3 个新面孔进入了 node.js 框架分类排行的 TOP 10：

Fastify 是一个受 Hapi 启发而开发的通用 Web 框架，也适合用作 JSON HTTP API 服务器；
Server.js 注重于“开箱即用”的开发体验；
Nest 是一个用 TypeScript 写的框架，其模块化和控制器组合的架构设计，让 Angular 用户感到十分亲切。


## React 生态圈

![](/images/javascript/3647457642-5a659718a15bf_articlex.png)

React 只专注于视图层 (View Layer)，这在为整个生态圈留下更多发展空间的同时，也为自身快速向前发展创造了机会。这个分类下我们会统计基于 React 和 React Native 构建的项目。

Create React App 通过集成优秀的预置和包，解决了新建 React 应用时要进行繁琐复杂的配置问题。今年 Facebook 也继续保持了频繁更新的节奏，使他成为目前 React 生态中最活跃的项目。

作为 Create React App 的一个成功案例，我们可以看 StackBlitz，这是一个在线 IDE，通过 Create React App，让你在数秒内就可以在浏览器中创建一个应用。

即使 Create React App 已被默认为 React 的新建工具包，开发者们仍然可以有其它选项，例如 React boilerplate，这也是十分受人关注的项目，继承了诸如 GraphQL 的很多有用功能。

Ant Design，Ant Design Pro 和 Material UI 是 React 组件的样式工具集，它们能帮助程序员在新建应用时而不再担心样式问题。

第 10 名 Recompose 的人气值也证明了开发者们喜欢 React 的原因：它的“函数式”特性，一切皆函数。Recompose 提供了一全套的函数来帮助你走的更远。

## Vue.js 生态圈

![](/images/javascript/3046574298-5a65971824692_articlex.png)

特约撰稿人: Evan You，虽然我们很欣赏 Vue.js，但不得不承认我们并不太熟悉它的生态圈。为此我们找了一位专家来点评下今年 Vue.js 的发展情况，那么还有谁能比 Vue.js 作者更熟悉呢？
在 2017 年，伴随着 Vue.js 用户的增长，许多 Vue.js 生态圈中的项目也得到了令人惊喜地快速成长。

Element 和 iView 是两个最受欢迎的 UI 组件工具包，专注于桌面端 UI 界面的快速开发。而 Mint UI 与 vux 则相反，是移动端最受欢迎的 UI 工具包。

Vuetify 是一款功能最完备的能同时适用于移动端和桌面端的 Material Design 组件框架内置了包括服务端渲染、PWA、CLI 模板支持等诸多特性。

Nuxt 则是一款基于 Vue.js 的更高级的框架，它能让我们流畅地开发具备服务器端渲染能力的 Vue.js 应用，而它的通用性使我们能用同样的代码库来构建单页引用，甚至生成静态网站。

Weex 是一个可以用 Vue.js 语法和 API 来进行原生渲染的移动桌面应用开发。它由阿里巴巴公司开发，并已运用于世界上一些最高频使用的移动应用中，十分注重性能问题上的优化。

## 移动开发

![](/images/javascript/1779497386-5a65971811922_articlex.png)

无所不能的 JavaScript，自然可已用来编写移动应用，这意为着你可以在 WEB 端与 Native 端复用你的组件。

在本分类中，我们为 3 大前端框架找到了对应的解决方案：

+ React: React Native
+ Vue：Weex 和 Quasar
+ Angular：Ionic 和 NativeScript

与 2016 年一样，React Native 两年蝉联头名，它能让我们使用把 JavaScript 编译成能够运行在 iOS、Android 甚至是 Windows 系统的原生 APP 应用。

正如视频 使用 React Native 来跨平台编译APP中所特别强调的：“Write One, Run Everywhere” 的承诺已经变成现实。

## 编译工具：Compilers

![](/images/javascript/3049681174-5a659718c25c1_articlex.png)

这里我们将讨论那些编译到标准 JavaScript 代码的语言。

通常情况下，在搭建自己的构建工作流时需要编译器可能有 2 个原因：

想立即使用最新的 JavaScript 语言特性，并把它应用到尽可能多的浏览器中，这类需求让 Babel 获得了成功，很多项目都依赖于它；
想为语言添加新的特性，比如“类型检查”；
对 Javascript 程序员进行分类有个热门的问题是：你是用类型的，还是不用类型的？

JavaScript 本身带有基本的动态类型，但缺乏静态类型。而很多开发者倾向于在代码中使用类型，尤其在大型项目中，因为这样可以让代码变得更为健壮且易于阅读和理解。

如果你需要类型，有两个主流可选项：微软的 TypeScript 和 Facebook 的 Flow（Facebook 在自己的主要项目 React，React Native，Jest 中都有使用)

你可以从 James Kyle 的文章来感受两者的区别: A Comparison Between Adopting Flow or TypeScript。

## 构建工具：Build Tools

![](/images/javascript/1501585920-5a65971731fc2_articlex.png)

构建工具分类中的排行冠军是 Parcel，这或许是今年最大的惊喜，作为一个 8 月份才在 GitHub 上发布的新项目却已达到 14K 个 star 的关注度。

Parcel 不仅提供现代前端开发所需的各种功能，还有个碾压性的优势：零配置！这是它与依靠大量 "loaders" 的 Webpack 最大区别。

请别误解数字，Webpack 依然是最流行的构建应用，它在 GitHub 上 有 35K 的 star 和超 500 人的贡献者。目前有许多项目使用了它，包括今年最流行的两个项目：Create React App 和 Gatsby。

Webpack 不断在迭代更新，2.0 版本可以让开发者通过动态加载的方式轻松实现“代码分割”的功能。

Webpack 与 Parcel 同时定位于构建 WEB 应用，而 Rollup 则定位于库的构建，它专注于 ES6 模块的性能提升上。

Rollup 已被一些主流的库使用，值得一提的是 React 团队也在 2017 年把它们的构建系统从 Browserify 切换到了 Rollup。

在 React 博客中提到

Rollup 可以预编译并且集成到应用中，能与 React 之类相似的库做到完美配合。
Poi 与 Parcel 有同样的目标：一款现代网络应用构建工具，它默认零配置但你可以通过使用 preset 来扩展。

## 测试框架

![](/images/javascript/3518395679-5a65971722936_articlex.png)

正如我们去年预计的一样 (这是我们第一次预测成功!)，Jest 成了今年测试框架类别中的王者。

Jest 最初是 Facebook 因为 React 组件测试目的而开发的，但最近几个月革命性的版本变更（发布了 22 个大版本）使得它现在能同时用于测试前端、后端代码。

Jest 有几个大的闪光点：

无需配置，默认地设定已经满足通常需求；
强大的开发者体验 (智能观察模式，直观的错误报告)；
语法上与 Mocha 很接近，许多程序员熟悉 describe 和 it 这样的关键词；
不需要额外的库来创建 assertions，已全部内置；
独特的"快照"模式可以作为重新运行测试时的对比基准；
AVA，去年的第一名，仍然有许多吸引人的特点。

这个项目由 Sindre Sorhus 创建并在他所有的项目中使用，熟悉他的同学肯定知道这意味着什么！

相较于 Jest，AVA 更侧重于并行测试上的速度，更轻量，也更接近测试标准，语法上与测试框架 Tape 接近。

## IDE 和编辑器

![](/images/javascript/2012472919-5a6597166f91a_articlex.png)

在这里我们讨论的是利用 WEB 技术来构建的开源的代码编辑器（ Sublime 粉丝们对不住了！）。

2016 年由微软主导的 VS Code 与 GitHub 主导的 Atom 在本类别中齐头并进。今年他们也依然处于领先地位，不过在互相较量中，VS Code 己领先它的对手一大截。

每个月 VS Code 都会发布新版本，带来更多实用 IDE 功能同时性能上却没有太大的损耗。即使不安装任何插件，你也有一大堆开箱即用的功能：

+ Git 集成功能；
+ 自动补全：JavaScript 语法，文件路径进行补全，npm 包名字等等；
+ React 语法集成等；

此外，你可以在编辑器中添加 Prettier 插件，这样每次保存时它都会自动格式化文件，在这样的编程环境下编码真是一种享受。

## CSS in JavaScript

![](/images/javascript/672737203-5a6597162252f_articlex.png)

目前 React 社区仍然没有就如何有效管理组件样式这个问题达成共识，即没有标准的解决方案。

如果无需太多自定义的标准样式，可以用 Material UI 或 Ant Design 这样现成的组件工具包。如果需要更高度灵活的自定义，你仍然能使用传统方式：用一个像 Bootstrap 或 Bulma 这样的全局 CSS 样式，通过修改组件的 className 属性来达到目的。这样做缺点是组件无法进行自我样式管理，因为样式分布在单独的文件中。

CSS in JavaScript 概念的出现即是为了解决上述问题。

概念本身很简单：既然我们在 React 中己能通过 JavaScript 来同时控制逻辑和模板部分，何不再进一步，连样式也一并管理了呢？

Styled Components 是今年本类别的冠军，它利用 JavasScript 最近新加入的模板字符串特性，让开发者在 React 组件中直接使用标准的 CSS 语法编写样式。

CSS Modules，作为本类别的亚军，则采用了混合的解决方案。它让开发者自己挑选诸如标准 CSS, SASS, NO slug Less, NO slug Stylus等方式编写样式，再以文件的方式导入到组件中。

Mark Dalgleish，CSS Modules 的作者，写了一篇有意思的文章来阐述 CSS-in-JavaScript 解决方案：A Unified Styling Language。如果你对 CSS-in-Javascript 解决方案仍持怀疑态度的话，那此文绝对不容错过。

## 静态网站生成器

![](/images/javascript/389437627-5a659715ab32c_articlex.png)

静态网站生成器（SSG，Static Site Generator）是指能够生成一坨 HTML、CSS、JS 文件，方便你快速部署到 WEB 服务器上而不需要安装和配置数据库的工具。

静态网站具有速度快，稳定且易于维护的特点。作为 2016 年的亚军，Gatsby 今年成功拨得头筹。它新增了许多新功能来助你优化静态网站：

快速浏览和导出速度；
主动预加载机能；
智能代码分解 (模板 + 网页数据)；
Gatsby 使用 React 来做视图层(View Layer)，构建时候则用 GraphQL 来查询内容。它有一个强大的社区并且 React 官网也是用 Gatsby 的来搭建的.

React Static 是本类别的新面孔。它从 Create React App 项目中获得灵感，定位于做一个轻量的 Gatsby 替代方案，专注于性能和简洁。

此外，值得一提的是 Next.js 也能当静态网站生成器来用。

## GraphQL

![](/images/javascript/138154086-5a6597156f674_articlex.png)

在未来回顾 GraphQL 的历史时，2017 年很有可能会成为一个转折点。

像 the New York Times 这样的大公司开始使用 GraphQL，Relay 和 Apollo (两个主要的 GraphQL 客户端框架) 也在今年发布了两个重要的版本更新。

在这两大库的身后，像 Graphcool 这样的公司也提供了大量的工具和库，而 Vulcan 这样的全栈框架也开始采用 GraphQL 。

值得注意的是今年最有人气的静态网站生成器 Gatsby 也在数据处理中使用了 GraphQL 。

随着越来越多的人加入到 GraphQL 阵营来, 可以预见其在技术上广泛取代 REST 只是一个时间问题。

## 总结

希望我们今年对 JavaScript 领域做出的回顾能够对你有所启发。

可以看到，Vue.js 两年蝉联冠军并且没有丝毫停下来的征兆。

React 生态圈也最终解决了证书问题，继续繁荣发展的势头。

但是如果让我们评选2017 项目之星的话，那绝对是` Prettier`。有了它，我们写代码时的再也不用担心格式问题！

State of JavaScript 2017 survey 收集和分析了 23,000 位开发者的调研问卷，能帮助你从另一个视角来解读社区演化的方向。


[2017 年崛起的 JS 项目](https://segmentfault.com/a/1190000012927293)