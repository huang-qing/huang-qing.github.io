---
layout:     post
title:      现代前端技术解析
subtitle:   
date:       2019-07-26
author:     huangqing
header-img: img/post-bg-technology-stack.jpg
catalog: true
categories: [TechnologyStack]
tags:
    - TechnologyStack   
    - Book
---


## 1. Web前端技术基础

JavaScript前端语言在Node.js服务器端也可以进行高效的开发。

- Node.js是基于Chrome V8 引擎的 JavaScript 运行环境，使用事件驱动、非阻塞式I/O的模型
- Node.js 的包管理器为 npm 
- `Koa`：一个简洁高效的 Node.js 端 Web 框架

#### 前端开发实现模式演变：
1. 静态黄页
2. 服务器组装动态网页数据
3. 后端为主的MVC
4. 前后端分离
5. 纯前端MV*为主、中间层直出
6. 前端`Virtual DOM`、`MNV*`(model-Native-View-*)、`前后端同构`

#### 浏览器渲染引擎：

工作流程： 解析HTML构建DOM树->构建渲染树->渲染树布局阶段->绘制渲染树

页面生成后，如果页面元素位置发生变化，就要从布局阶段开始重新渲染，也就是页面重排，所以页面重排一定会进行后续重绘。

#### 浏览器数据持久化存储技术：

1. HTTP文件缓存：基于HTTP协议的浏览器端文件级缓存机制
   1. Cache-Control Expires
   2. Etag、If-None-Match
   3. Last-Modified、If-Modified-Since
2. LocalStorage：HTML5的一种本地缓存方案。IE 5MB，Chrome 2.6MB
3. SessionStorage:在关闭浏览器时自动清空
4. indexDB：可在客户端存储大量结构化数据并能在这些数据上使用索引进行高性能检索的一套API。
5. Web SQL：浏览器用于存储较大数据的缓存机制。Chrome支持，由于兼容性问题，应用不广泛。
6. Cookie：辨别用户身份或Session跟踪存储在用户浏览器端的数据
7. CacheStorage：在ServiceWorker规范中定义。使用ServiceWorker可以让Web具有消息推送、离线使用、自动更新的功能。
8. Application Cache：已被标准弃用。

#### Web调试工具：
1. Chrome
2. Fiddler

Node调试工具：node-supervisor、node-inspector

远程调试工具：Vorlon.js、Weinre

## 2. 前端与协议

#### HTTP:

请求头部结构：
1. 请求类型、请求URI、协议版本、扩展内容
2. 请求头部域内容：Accept、Coolkie、Cache-Control、Host
3. 空行
4. 正文内容

响应头部结构：
1. 状态码、状态描述、协议版本、扩展内容
2. 响应头部域内容：Data、Content-Type、Cache-Control、Expires
3. 空行
4. 正文内容

#### HTTP1.1 

长连接：keep-alive

协议切换： Connection:Upgrade Upgrade:websocket

缓存控制: Cache-Control Etag、Last-Modified  

#### web安全机制：

1. XSS（Cross Site Script，跨站脚本攻击）：由带有页面可解析内容的数据未经处理直接插入到页面上解析导致。防范方法是验证输入到页面上所有内容来源数据是否安全，如果可能有脚本标签内容则需要进行必要的转义。
   1. 存储型XSS
   2. 反射型XSS
   3. MXSS（DOM XSS）
2. SQL注入：页面提交数据到服务器端后，在服务端未进行数据验证就将数据直接拼接到SQL语句中执行，产生执行与预期不同的现象。
3. CSRF（Cross-site Request Forgery，跨站请求伪造）：非源站点按照源站点的数据请求格式提交非法数据给源站点服务器的一种攻击方法。可以通过Token（令牌）提交验证阻止跨站伪请求。

DNS劫持：劫持DNS服务器，取得某域名的解析记录控制权，进而修改此域名的解析结果，导致访问IP的修改。

HTTP劫持：网络数据传输通道中网关或防火墙上监视特定信息，满足一定条件时会在正常数据包中插入或修改为攻击者设计的网络数据包。例如：ISP（Internet Service Provider 互联网服务提供商），劫持修改，添加广告。

HTTPS通过加入SSL层加密HTTP数据进行安全传输的HTTP协议，默认使用443端口。

前端实时协议：Ajax、WebSocket、Poll（轮询）、Long-poll（长轮询）、DDP

WebSocket在网络传输的最小单位是帧，数据的传输也可以理解为流式传输。

Long-poll:典型的应用场景就是网站通过对应的移动客户端进行扫描二维码登录。设置一个较长的超时时间即可。

#### RESTful

RESTful（Representational State Transfer，表述性状态转化）数据协议：并不是一种具体的协议，而是定义了一种网络应用软件之间的架构关系并提出了一套与之对应的网络之间的交互调用的规则。

RESTful风格接口定义：
1. POST     `path/v1/book`      新增
2. DELETE   `path/v1/book`      删除
3. PUT      `path/v1/book`      全量更新
4. DISPATCH `path/v1/book`      部分更新
5. GET      `path/v1/book`      获取信息

#### Hybrid APP：

1. Hybird App 可用的系统网络资源更少。
2. 支持更新的浏览器特性。
3. 可实现离线应用。 新的浏览器特性或Native的文件读取机制
4. 较多的机型考虑
5. 支持与Native交互。Native:摄像头、定位、传感器、本地文件访问

Web到Native的协议调用：
1. URI请求：Native应用在移动端系统中注册一个Scheme协议的URI，这个URI可以在系统的任意地方授权访问调用一段原生方法或原生界面。
2. 通过`addJavascriptInterface`注入方法到页面应用。

Native到Web协议调用：
1. `loadUrl`（android）、`stringByEvaluatingJavaScriptFromString`（IOS）
2. JSBridge

## 3.前端端三层结构与应用

HTML CSS Javascript

#### Web语义化标签

![](/images/book/2019-10-16_170102.png)

+ `<nav>`是导航菜单的结构
+ `<articel>`是存放页面的文章内容

#### AMP HTML：

流动网页提速，google推行的一个提升页面资源载入效率的HTML提议规范。

+ 浏览器对同一个域名下的资源最多支持4-6个并行下载数，为了增大资源下载并行数，常常将HTML、JavaScript、CSS、图片资源分域存放。分域也可将静态资源请求进行服务器端的负载均衡，并隔离cookie。
+ 只允许异步的script脚本
+ 只加载静态的资源
+ 不能让内容阻塞渲染
+ 不在关键路径中加载第三方Javascript
+ 所有CSS必须内联
+ 字体使用声明必须高效
+ 最小化样式声明
+ 只运行GPU加速动画
+ 处理好资源加载顺序问题
+ 页面必须立即加载
+ 提升AMP元素性能

#### Shadow DOM:

HTML的一个规范，它允许浏览器开发者封装自己的HTML标签、CSS样式和特定的JavaScript代码，同时也可以让开发人员创建类似`<video>`这样的自定义一级标签，创建这些新标签内容和相关的API被称为Web Component。

TypeScript概况：JavaScript除了主要遵循ECMAScript 6+标准外，还遵循另一种语法规范

#### ECMAScript 5:

+ ECMAScript 5 Object:
  + getPrototypeOf
  + getOwnPropertyDescriptor
  + getOwnPropertyNames
  + create
  + defineProperty
  + defineProperties
  + seal
  + freeze
  + preventExtensions
  + isSealed
  + isFrozen
  + isExtensible
  + keys
+ ECMAScript 5 Array:
  + indexOf
  + lastIndexOf
  + lastIndexOf
  + every
  + some
  + forEach
  + map
  + filter
  + reduce
  + reduceRight
+ Function.prototype.bind
+ String.prototype.trim
+ Date.now()、Date().toJSON

#### ECMAScript 6:

+ 块级作用域变量声明关键字 let、const,执行速度比var快约65%
+ 字符串模板
+ 解构赋值 `let [a,b,c]=[11,22]` `let {one,two,tree}={two:2,three:3,one:1}`
+ 数组
  + `[...arr]` ...进行浅拷贝
  + Array.from
  + Array.of
  + Array.prototype.copyWithin
  + Array.prototype.fill
  + Array.prototype.find
  + Array.prototype.findIndex
  + for...of
  + entries()   键值对进行遍历
  + keys()      键名进行遍历
  + values()    键值进行遍历
+ 函数参数
  + 默认参数    `function sayHi(name='hq')`
  + 不定参数    `function sayHi(...name)`
  + 扩展参数    `function sayHello(name1,name2)   sayHello(...name)`
  + 箭头函数
  + 增强对象
    + 属性简写
    + 返回变量或对象作为属性名 `[getKey('family')]:'hq'`
    + 对象方法属性简写声明 
  + 类 `class` `extends`
  + 模块module  `import/export`
  + 循环迭代器 for...of、map、forEach
  + 集合： Map、Set、WeakMap、WeakSet
  + 生成器Generator：Generator可以认为是一个可中断执行的特殊函数，声明方法是在函数名后面加上`*`来与普通函数区分。`const generator=function *()`
  + Promise
  + Symbol: 基本数据类型，一般作属性键值，并且能避免对象属性键的命名冲突。
  + Proxy：可以用来拦截某个对象的属性访问方法，然后重载对象的`.`运算符。
  + 统一码
  + Reflect对象和tail calls 尾调用

#### ECMAScript 7+

+ 幂指数操作符 `x**y`
+ Array.prototype.includes:判断是否包含某个元素
+ 异步函数 async/await。可以认为是对Generator的一种封装简化

总结一下Javascript实现异步的方法：setTimeout、事件监听、观察者模式、$.Deferred、promise、generator、async/wait、第三方async库

#### TypeScript

+ 强类型支持：目前该特性没有很好的应用优势，反而增加了使用的复杂度
+ Decorator装饰器特性：class、property、method、parameter

#### CSS

+ CSS选择器：！important>内联样式（权重1000）>id选择器（权重100）>类选择器（权重10）>元素选择器（权重1），多种组合情况按照权重相加的原则计算。
+ CSS属性分类
  + 布局：position、flex、float、align
  + 几何类属性：盒模型（margin、padding、width、height、border）、box-shadow、渐变gradient、background类、transform类
  + 文本类属性：font类、line-height、color类、white-space、user-select、text-shadow
  + 动画类属性：transition、animation
  + 查询类：Media query、IE Hack

CSS样式统一化
+ reset 将不同浏览器中标签元素的默认样式全部清除
+ normalize 在整个站样式基本确定的情况下对标签统一使用同一个默认样式规则
+ neat 结合前两种。例如只想对文本相关的元素设置内外边距`5px`，而其他的元素清除不同的浏览器默认样式。

CSS预处理

+ SASS
+ LESS
+ Stylus
+ postCSS

#### 动画实现：

设置setInterval的时间间隔是16ms。人眼能辨识的流畅动画为每秒60帧，这里16ms比1000ms/60帧略效益点，所以这种情况下可以认为动画是流畅的。在很多移动端动画性能优化时，一般使用16ms来进行节流处理连续触发浏览器事件，例如touchmove、scroll事件进行节流。

项目实践中，浏览器端推荐使用Javascript直接实现动画或SVG动画；移动端使用CSS3 transition、CSS3 animation、canvas或requestAnimationFrame。

+ JavaScript直接实现动画
+ 可伸缩矢量图形（SVG）动画
+ CSS3 transition 过渡动画。移动端开发，通过添加`transform:translate3D(0,0,0)`或`transform:trnaslateZ(0)`来开启移动端动画的GPU加速，让动画流程更加流畅。
+ CSS3 animation。通过对关键帧和循环次数的控制，页面标签元素会自动根据设定好的样式改变进行平滑过渡，关键帧状态的控制一般是通过百分比来控制。
+ Canvas动画
+ requestAnimationFrame

####  响应式页面实现：

+ 通过前端或后端判断 userAgent 来跳转不同的页面完成不同设备浏览器的适配，也就是维护两个不同的站点根据用户的设备进行对应的跳转。该方案适合用于功能复杂并对性能要求比较高的站点应用。
+ media query 媒体查询等手段，让页面根据不同的设备浏览器自动改变页面的布局和显示，但不做跳转。该方案适用于访问量较小、性能要求不高的应用场景。

结构层响应式：

根据不同设备浏览器渲染不同的页面内容结构，不是直接进行页面跳转。
+ 前端渲染数据。桌面浏览器和移动端浏览器直接加载到HTML结构是相同的，但是加载的JS文件和CSS文件不同
+ 后端渲染数据（直出层）
+ 媒体响应式
  + 使用Media Query 背景图片代替
  + `<picture>`标签元素
  + 模板判断响应式图片
  + 图片服务器判断输出内容

表现层响应式：

+ 响应式布局。栅格系统解决百分比方式布局。`@media`
+ 屏幕适配布局。`<meta name="viewport" content="width=device-width,initial-scale=1, maximum-scale=1, user-scalable=no"/>`
  + zoom 属性控制方案    `document.body.style.zoom = screen.width/320`
  + REM 屏幕适配方案     `font-size` `rem` (`vh`、`vw`、`vmax`、`vmin`)

## 4.现代前端交互框架

#### 直接操作DOM：

1. jQuery
   + 延时对象: `$.Deferred` 对象来处理异步回调嵌套的问题。
   + 主要实现了选择器、DOM操作方法、事件绑定封装、AJAX、Deferred这五个方面的封装和常见的兼容性问题处理。
2. zepto（移动端）

SPA（Single Page Application，单页面应用）

#### MV*交互模式

1. 前端MVC（Model-View-Controller）模式
   + 让URL的地址内容匹配对应的字符串然后进行相应的操作
   + 使用HTML5的pushState来实现路由。`histroy.pushState(state,title,url)`
2. 前端MVP（Model-View-Presenter）用户进行DOM修改操作时将通过View上的行为触发，然后将修改通知Presenter完成后面Model修改和其他View的更新。Presenter和View的操作绑定通常是双向的。
3. 前端MVVM模式。可以理解为自动化的MVP框架，并且使用ViewModel代替了Presenter，即数据Model的调用和模板内容的渲染不需要我们主动操作。

#### MVVM设计解析：

初始化解析过程：
+ Directive：指令。 例如： `q-html` `q-on`
+ Filter:过滤器。 例如：`q-html="time | formartTime"`
+ 表达式设计。
+ ViewModel设计。
+ 数据变更检测。
  + 手动触发绑定：通过数据对象上定义get()方法和set()方法。
  + 脏数据检测：ViewModel对象的某个属性值发生变化时找到与这个属性值相关的所有元素，然后在比较数据变化，如果变化则进行Directive指令调用，对这个元素进行重新扫描渲染。
  + 对象劫持（Hijacking）：目前使用的比较广泛的方式。使用`Object.defineProperty`和`object.defineProperies`对ViewModel数据对象进行属性 `get()`和`set()`的监听，当有数据读取和赋值操作时则扫描元素节点，运行指定对应节点的Directive指令，这样ViewModel使用通用的等号赋值就可以了。
  + Proxy（ECMAScript6）

![](/images/book/2019-10-21_103624.png)

#### Virtual DOM 交互模式：

整体来看，Virtual DOM的交互模式减少了MVVM或其他框架中对DOM的扫描或操作次数.

核心操作：
1. 创建 Virtual DOM。Javascript直接分析HTML字符串文本来生成Virtual DOM，所以这就比DOM API操作要快。创建Virtual DOM往往就是将一段DOM描述字符串解析成Virtual DOM对象的过程。
2. 对比两个 Virtual DOM 生成差异化 Virtual DOM。对于 Virtual DOM 的对比算法实际上是对于多叉树结构遍历算法，对于多叉树遍历有广度优先算法和深度优先算法。
3. 将差异化 Virtual DOM 渲染到页面上。与以前的交互模式相比，Virtual DOM 最本质的区别在于减少了DOM对象的操作，通过JavaScript对象来代替DOM对象树，并且在页面结构改变进行最小代价的DOM渲染操作，提高了交互的性能和效率。这就是Virtual DOM 交互模式的优势，也是提高前端交互性能的根本原因。


#### MNV*模式实现原理

![](/images/book/2019-10-21_174036.png)

MNV*的基本原理主要是将JSBridge和DOM编程的方式进行结合，让前端能够快速构建开发原生界面的应用，从而脱离DOM的交互模式。

## 5. 前端项目与技术实践

内联的资源大小标准一般为2KB以内，否则可能会导致HTML文件过大，页面首次加载时间过长。

代码单行长度不要超过120字符

head中必须定义title、keyword、description，保证基本的SEO页面关键字和内容描述。添加viewport控制页面不缩放。

为`<img>`元素加上alt属性

为表单内部元素`<label>`加上for属性或者将对应控件放在`<label>`标签内部。

CSS类名命名一般由单词、中划线组成，推荐一种规范--所有命名都使用小写，加上ui-等前缀。

CSS属性为值为0，则不需要为0加单位。

去掉URL中引用资源的引号`background-image:url(sprites.png)`

颜色值写法，所有的颜色值要使用小写并尽量缩写至3位

CSS属性书写顺序，先写布局属性（position、display、float、overflow），再写元素内容属性。

CSS规则的Hack形式：

```CSS
.ui-news p{
  color:#000;   /*For all*/
  color:#111\9; /*For all IE*/
  color:#222\0  /*For IE8 and later,Opera without Webkit*/
  color:#333\9\0/*For IE9 and later*/
}
```

Internet Explorer,可以使用条件注释最为预留Hack来使用

```html
<!--[if <keywords>? IE <version>?]>
<link rel="stylesheet" href="./hack.css">
<![endif]-->
```

使用预处理脚本编码开发，如嵌套、变量、嵌套属性、注释、继承。

ECMAScript 6+
+ 变量声明关键字 `let` `const`
+ 字符串拼接使用字符串模板完成
+ 解构赋值尽量使用一层解构 `let [a, b, c] = [11, 22, 33];`
+ 数组拷贝推荐使用`...`,更加简洁高效。 `itemsCopy=[...items]`
+ 数组循环遍历使用 `for ...of`,非必须情况下不推荐使用 `forEach`、`map`、简单循环。
+ 使用ECMAScript6 的类来代替之前的类实现方式，尽量使用`constructor`进行属性成员变量赋值。
+ 模块化多变量导出时尽量使用对象解构，不使用全局导出。尽量不要把import和export写在一行
+ 导出类名时，保持模块名称和文件名相同，类名首字符需要大写
+ 生成器中yield进行异步操作是需要使用`try...catch`包裹
+ 推荐使用Promise
+ 如果不是必须，避免使用迭代器
+ 合理使用Generator，推荐使用async/await,更加简洁

前端防御行编程规范：
+ 对外部数据的安全检测判断 `data.userinfo && data.userinfo.name || '默认命名'`
+ 规范化错误处理。对于AJAX请求或长时间文件读写等可能失败的异步操作，需要进行错误情况的处理或异常捕获处理。

前端组建规范：
+ UI（User Interface，用户界面）组建规范
+ 模块化规范：
  + AMD（Asynchronous Module Definition，异步模块定义）:主要以requireJ为代表，基本原理是定义define和require方法异步请求对应的javascript模块文件到浏览器端运行。
  + CMD（Common Module Definition，通用模块定义）:Seajs提出的一种模块化规范。遵循按需执行依赖的原则，只有在用到某个模块的时候才会执行模块内部的require语句，同时加载完某个依赖模块文件后并不立即执行，在所有依赖模块加载完成后进入主模块逻辑，遇到模块运行语句的时候才执行对应的模块。
  + CommonJS：Node端使用的JavaScript模块化规范，使用require进行模块引入，并使用modules.exports来定义模块导出。
  + import/export：ECMAScript 6 定义的Javascript模块引用方式。

项目组件设计规范：
+ Web Component 组件化
+ MVVM框架组件化
+ 基于Virutal DOM框架的组件化

构建工具设计问题：
+ 模块分析引入:JavaScript AST（Abstract Syntax Tree,抽象语法树，是将JavaScript代码映射成一个树形结构的JSON对象树）
+ 模块化规范支持
+ CSS编译、自动合并图片
+ HTML、JavaScript、CSS资源压缩优化
+ HMTL路径分析替换：提供将文件相对路径自动替换成绝对路径或线上CDN路径的能力
+ 区分开发和上线目录环境
+ 异步文件打包方案
+ 文件目录白名单设置

前端性能优化：

+ 前端性能测试：
  + Performance Timing API：支持IE9以上WebKit浏览器用于记录页面加载和解析过程中关键时间点的机制。浏览器加载和解析一个HTML文件的详细过程先后经历unload、redirect、App Cache、DNS、TCP、Request、Response、Processing、onload。
  + Profile工具：用于测试页面脚本运行时系统内存和CPU资源占用情况的API
  + 页面埋点计时
  + 资源加载时序图
+ 移动端浏览器前端优化策略：
  + 网络加载类
    1. 首屏数据请求提前 
    2. 首屏加载和按需加载，非首屏内容滚屏加载，保证首屏内容最小化。一般推荐移动端界面首屏数据展示延时最长不超过3秒。
    3. 模块化资源并行下载
    4. inline首屏必备的CSS和JavaScript
    5. meta dns prefectch 设置DNS预解析
    6. 资源预加载
    7. 合理利用MTU（TCP网络传输的最大传输单元）策略。尽量保证页面的HTML内容在1KB以内。
    8. 
  + 缓存类
    1. 合理利用浏览器缓存
    2. 静态资源离线方案
    3. 尝试使用AMP HTML
  + 图片类
    1. 图片压缩处理
    2. 使用较小的图片，合理使用 base64 内嵌图片（2KB以内）
    3. 使用更高压缩比格式的图片，如webp
    4. 图片懒加载
    5. 使用Media Query 或 srcset 根据不同屏幕加载不同大小图片
    6. 使用iconfont代替图片图标
    7. 定义图片大小限制。单张图片一般建议不超过30KB.推荐10KB。
  + 脚本类
    1. 尽量使用id选择器
    2. 合理缓存DOM对象
    3. 页面元素尽量使用事件代理，避免直接事件绑定
    4. 使用touchstart代替click。touchstart事件和click事件触发时间之间存在300毫秒的延时，在没有touchmove滚动处理时，可以使用touchstart事件代替click事件。
    5. 避免 touchmove，scroll连续触发回调的事件节流，例如设置每隔16ms才处理一次事件。
    6. 避免使用eval、with、使用join代替连接符`+`。
    7. 尽量使用ECMAScript 6+的特性来编程
  + 渲染类
    1. 使用viewport固定屏幕渲染
    2. 避免各种形式重排重绘
    3. 使用CSS3动画，开启GPU加速。`transform:translateZ(0)`
    4. 合理使用Canvas和requestAnimationFrame
    5. SVG代替图片
    6. 不滥用float。推荐使用固定布局或flex-box弹性布局
    7. 不滥用web字体或过多font-size声明
  + 架构协议类
    1. 尝试使用SPDY和HTTP2
    2. 使用后端数据渲染
    3. 使用Naive View 代替 DOM 的性能劣势

前期在设计构建、组件的解决方案时要解决好异步的自动处理问题。


前端用户数据分析：
+ 用户访问统计：
  + PV（Page View）
  + UV（Unique Visitor）：前端页面统计中一个最有价值的统计指标，因为其直接反应页面的访问用户数
    + 根据浏览器Cookie和IP统计
    + 结合用户浏览器标识userAgent和IP统计
  + VV（Visit View）
  + IP（访问站点的不同IP数）

## 6. 前端跨栈技术

Node后端开发基础概述
+ 服务器知识基础
+ 简单的数据库设计能力 MongoDB（NoSQL数据库）
+ 后端MVC设计理念
+ 后端异步。Promise、Generator、async/await .Koa框架。
+ 模块化思想
+ 中间件技术
+ 接口设计规范。RESTful
+ 后端部署技术和基本运维能力

早期MEAN：使用Express作为Web框架进行小型Web站点建设，结合主流技术以M（Mysql）、E（Express）、A（Angular）、N（Node）最为典型

Node后端数据渲染：也称之为直出层。

前后端同构：只开发一套项目代码，既可以用来实现前端的Javascript加载渲染也可以用于后端的直出。技术实现上来看，至少有三种思路：
1. 数据模板的前端渲染和后台直出
2. MVVM的前端实现和后台直出
3. Virtual DOM的前端渲染和后端直出

前端项目开发流程设计：构建项目开发的Codebase
1. 前端框架选型
2. 模块化方案
3. 代码规范化
4. 构建自动化
5. 组件化目录设计
6. 代码优化处理
7. 数据统计
8. 同构项目结构设计