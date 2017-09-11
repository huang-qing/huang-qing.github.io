---
layout:     post
title:      RESTful API 设计指南
subtitle:   
date:       2017-04-22
author:     huangqing
header-img: img/post-bg-restful.jpg
catalog: true
categories: [RESTful]
tags:
    - RESTful   
---




# RESTful API 设计指南

## 介绍

现在的网络应用程序，分为前端和后端两个部分， *采用客户端/服务器* 模式。

后端建立在分布式体系上，通过互联网通信，具有高延时（high latency）、高并发等特点；前端设备层出不穷（如手机、平板、桌面电脑、其他专用设备......）。

![ each client app has its own embedded business logic](/images/restful/947566-1f50d8c6c8b443f0.png)

因此，必须有一种统一的机制，方便不同的前端设备与后端进行通信。这导致API构架的流行，甚至出现"API First"的设计思想。


![ central API which hold all the business logic](/images/restful/947566-472033adb66aad2e.png)

目前，有三种主流的Web服务交互方案：

+  SOAP（Simple Object Access protocol，简单对象访问协议）

+  XML-RPC 

+  RESTful。

RESTful架构，是目前最流行的一种互联网软件架构。它结构清晰、符合标准、易于理解、扩展方便。RESTful API是目前比较成熟的一套互联网应用程序的API设计理论。

## 概念简述

REST（Representational State Transfer）这个词，是Roy Thomas Fielding在他2000年的博士论文中提出的。

Fielding是HTTP协议（1.0版和1.1版）的主要设计者、Apache服务器软件的作者之一、Apache基金会的第一任主席。

他写的关于RESTful的论文是这样描述的：

>"本文研究计算机科学两大前沿----软件和网络----的交叉点。长期以来，软件研究主要关注软件设计的分类、设计方法的演化，很少客观地评估不同的设计选择对系统行为的影响。而相反地，网络研究主要关注系统之间通信行为的细节、如何改进特定通信机制的表现，常常忽视了一个事实，那就是改变应用程序的互动风格比改变互动协议，对整体表现有更大的影响。我这篇文章的写作目的，就是想在符合架构原理的前提下，理解和评估以网络为基础的应用软件的架构设计，得到一个功能强、性能好、适宜通信的架构。"



Fielding将他对互联网软件的架构原则，定名为REST，即Representational State Transfer的缩写，翻译为"表现层状态转化"。

如果一个架构符合REST原则，就称它为RESTful架构。

## 主要的原则：

### 资源（Resources）

REST的名称"表现层状态转化"中，省略了主语。"表现层"其实指的是"资源"（Resources）的"表现层"。

所谓"资源"，就是网络上的一个实体，或者说是网络上的一个具体信息。它可以是一段文本、一张图片、一首歌曲、一种服务，总之就是一个具体的实在。

你可以用一个URI（统一资源定位符）指向它，每种资源对应一个特定的URI。要获取这个资源，访问它的URI就可以，因此URI就成了每一个资源的地址或独一无二的识别符。

### 无状态性

客户端和服务器之间的交互在请求之间是无状态的。

从客户端到服务器的每个请求都必须包含理解请求所必需的信息。如果服务器在请求之间的任何时间点重启，客户端不会得到通知。

此外，无状态请求可以由任何可用服务器回答，这十分适合云计算之类的环境。客户端可以缓存数据以改进性能。

### 统一接口

所有资源都共享统一的界面，以便在客户端和服务器之间传输状态。

使用的是标准的 HTTP 方法，使用 GET、PUT、POST 和 DELETE等。Hypermedia 是应用程序状态的引擎，资源表示通过超链接互联。

### 表现层（Representation）

"资源"是一种信息实体，它可以有多种外在表现形式。我们把"资源"具体呈现出来的形式，叫做它的"表现层"（Representation）。

例如：文本可以用txt格式表现，也可以用HTML格式、XML格式、JSON格式表现，甚至可以采用二进制格式；图片可以用JPG格式表现，也可以用PNG格式表现。

URI应该只代表"资源"的位置，它的具体表现形式，在HTTP请求的头信息中用Accept和Content-Type字段指定，这两个字段才是对"表现层"的描述。

### 分层系统

组件无法了解它与之交互的中间层以外的组件。通过将系统知识限制在单个层，可以限制整个系统的复杂性，促进了底层的独立性。

### 状态转化（State Transfer）

访问一个网站，就代表了客户端和服务器的一个互动过程。在这个过程中，势必涉及到数据和状态的变化。

互联网通信协议HTTP协议，是一个无状态协议。这意味着，所有的状态都保存在服务器端。因此，如果客户端想要操作服务器，必须通过某种手段，让服务器端发生"状态转化"（State Transfer）。而这种转化是建立在表现层之上的，所以就是"表现层状态转化"。

## 简单总结

什么是RESTful架构：

1.  每一个URI代表一种资源；

2.  客户端和服务器之间，传递这种资源的某种表现层；

3.  客户端通过四个HTTP动词，对服务器端资源进行操作，实现"表现层状态转化"。

## 误区

### URI包含动词。

"资源"表示一种实体，所以应该是名词，URI不应该有动词，动词应该放在HTTP协议中。

举例来说，某个URI是`/posts/show/1`，其中show是动词，这个URI就设计错了，正确的写法应该是`/posts/1`，然后用GET方法表示show。
如果某些动作是HTTP动词表示不了的，你就应该把动作做成一种资源。比如网上汇款，从账户1向账户2汇款500元，错误的URI是：

~~~javascript
POST /accounts/1/transfer/500/to/2
~~~

正确的写法是把动词transfer改成名词transaction，资源不能是动词，但是可以是一种服务：

~~~javascript
POST /transaction HTTP/1.1
Host: 127.0.0.1
from=1&to=2&amount=500.00
~~~

## RESTful API

### 协议

API与用户的通信协议，总是使用**HTTPs**协议(SSL/TLS)。

### 域名

应该尽量将API部署在专用域名之下。
    
`https://api.example.com`

如果确定API很简单，不会有进一步扩展，可以考虑放在主域名下。
    
`https://example.org/api/`

### 版本（Versioning）：

将API的版本号放入URL。

`https://api.example.com/v1/`

将版本号放在HTTP头信息中，但不如放入URL方便和直观。[Github](https://developer.github.com/v3/media/#request-specific-version)采用这种做法。

`Accept: vnd.example-com.foo+json; version=1.0`

### 路径（Endpoint）

路径又称"终点"（endpoint），表示API的具体网址。

在RESTful架构中，每个网址代表一种资源（resource），所以网址中不能有动词，只能有名词，而且所用的名词往往与数据库的表格名对应。一般来说，数据库中的表都是同种记录的"集合"（collection），所以API中的名词也应该使用复数。

举例来说，有一个API提供动物园（zoo）的信息，还包括各种动物和雇员的信息，则它的路径应该设计成下面这样。

~~~javascript
https://api.example.com/v1/zoos
https://api.example.com/v1/animals
https://api.example.com/v1/employees
~~~

### HTTP动词

对于资源的具体操作类型，由HTTP动词表示。

常用的HTTP动词有下面五个（括号里是对应的SQL命令）。

  +  GET（SELECT）：从服务器取出资源（一项或多项）。
  +  POST（CREATE）：在服务器新建一个资源。
  +  PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。
  +  PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）。
  +  DELETE（DELETE）：从服务器删除资源。

还有两个不常用的HTTP动词。

  +  HEAD：获取资源的元数据。
  +  OPTIONS：获取信息，关于资源的哪些属性是客户端可以改变的。

下面是一些例子。

~~~javascript

GET /zoos：列出所有动物园
POST /zoos：新建一个动物园
GET /zoos/ID：获取某个指定动物园的信息
PUT /zoos/ID：更新某个指定动物园的信息（提供该动物园的全部信息）
PATCH /zoos/ID：更新某个指定动物园的信息（提供该动物园的部分信息）
DELETE /zoos/ID：删除某个动物园
GET /zoos/ID/animals：列出某个指定动物园的所有动物
DELETE /zoos/ID/animals/ID：删除某个指定动物园的指定动物

~~~

### 过滤信息（Filtering）

如果记录数量很多，服务器不可能都将它们返回给用户。API应该提供参数，过滤返回结果。

下面是一些常见的参数。

~~~javascript

?limit=10：指定返回记录的数量
?offset=10：指定返回记录的开始位置。
?page=2&per_page=100：指定第几页，以及每页的记录数。
?sortby=name&order=asc：指定返回结果按照哪个属性排序，以及排序顺序。
?animal_type_id=1：指定筛选条件

~~~

参数的设计允许存在冗余，即允许API路径和URL参数偶尔有重复。比如，GET /zoo/ID/animals 与 GET /animals?zoo_id=ID 的含义是相同的。

### [状态码（Status Codes）](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)

服务器向用户返回的状态码和提示信息，常见的有以下一些（方括号中是该状态码对应的HTTP动词）。

  +  200 OK - [GET]：服务器成功返回用户请求的数据，该操作是幂等的（Idempotent）。
  +  201 CREATED - [POST/PUT/PATCH]：用户新建或修改数据成功。
  +  202 Accepted - [*]：表示一个请求已经进入后台排队（异步任务）
  +  204 NO CONTENT - [DELETE]：用户删除数据成功。
  +  400 INVALID REQUEST - [POST/PUT/PATCH]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作，该操作是幂等的。
  +  401 Unauthorized - [*]：表示用户没有权限（令牌、用户名、密码错误）。
  +  403 Forbidden - [*] 表示用户得到授权（与401错误相对），但是访问是被禁止的。
  +  404 NOT FOUND - [*]：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的。
  +  406 Not Acceptable - [GET]：用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式）。
  +  410 Gone -[GET]：用户请求的资源被永久删除，且不会再得到的。
  +  422 Unprocesable entity - [POST/PUT/PATCH] 当创建一个对象时，发生一个验证错误。
  +  500 INTERNAL SERVER ERROR - [*]：服务器发生错误，用户将无法判断发出的请求是否成功。

### 错误处理（Error handling）

如果状态码是4xx，就应该向用户返回出错信息。一般来说，返回的信息中将error作为键名，出错信息作为键值即可。

~~~json
{
    error: "Invalid API key"
}
~~~

### 返回结果

针对不同操作，服务器向用户返回的结果应该符合以下规范。

  +  GET /collection：返回资源对象的列表（数组）
  +  GET /collection/resource：返回单个资源对象
  +  POST /collection：返回新生成的资源对象
  +  PUT /collection/resource：返回完整的资源对象
  +  PATCH /collection/resource：返回完整的资源对象
  +  DELETE /collection/resource：返回一个空文档

### Hypermedia API

RESTful API最好做到Hypermedia，即返回结果中提供链接，连向其他API方法，使得用户不查文档，也知道下一步应该做什么。

比如，当用户向api.example.com的根目录发出请求，会得到这样一个文档。

~~~json

{"link": {
    "rel":   "collection https://www.example.com/zoos",
    " href":  "https://api.example.com/zoos",
    "title": "List of zoos",
    "type":  "application/vnd.yourformat+json"
}}

~~~

上面代码表示，文档中有一个link属性，用户读取这个属性就知道下一步该 调用什么API了。

rel表示这个API与当前网址的关系（collection关系，并给出该collection的网址）。

href表示API的路径，title表示API的标题，type表示返回类型。

Hypermedia API的设计被称为[HATEOAS](https://en.wikipedia.org/wiki/HATEOAS)。Github的API就是这种设计，访问[api.github.com](https://api.github.com/)会得到一个所有可用API的网址列表。

~~~json        
{
    "current_user_url": "https://api.github.com/user",
    "authorizations_url":"https://api.github.com/authorizations",
    // ...
}
~~~

从上面可以看到，如果想获取当前用户的信息，应该去访问[api.github.com/user](https://api.github.com/user)，然后就得到了下面结果。

~~~javascript
{
    "message": "Requires authentication",
    "documentation_url": "https://developer.github.com/v3"
}
~~~

上面代码表示，服务器给出了提示信息，以及文档的网址。

### 其他

+  API的身份认证应该使用OAuth 2.0框架。
+  服务器返回的数据格式，应该尽量使用JSON，避免使用XML。



## 参考资料：
+  [RESTful API 设计指南](http://www.ruanyifeng.com/blog/2014/05/restful_api.html)
+  [Principles of good RESTful API Design](https://codeplanet.io/principles-good-restful-api-design/)
+  [Hypertext Transfer Protocol -- HTTP/1.1](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)