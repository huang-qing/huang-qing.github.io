---
layout:     post
title:      RESTful Web Services
subtitle:   RESTful Web Services 概念讲解
date:       2017-04-21
author:     huangqing
header-img: img/post-bg-restful.jpg
catalog: true
categories: [RESTful]
tags:
    - RESTful
    - Book    
---




# RESTful Web Services

## Programmable Web 及其分类

programmable web 是基于HTTP和XML技术。

+  虽然HTML、JSON、纯文本、二进制也在使用，但采用较多的还是XML。

+  基于HTTP

## HTTP：信封里的文档

HTTP是一中基于文档的协议。客户端把文档放在信封里，发给服务器；作为回应，服务器把响应文档放在信封里，发给客户端。

HTTP对信封格式有严格的标准，但它并不关心信封里的内容。

`HTTP请求`的各个主要部分：

+ HTTP方法( HTTP method)

+  路径(path)

+  请求报头(request headers)

+  实体主体(entity-body)，也称文档(document)或表示(representation)

`HTTP响应`代码

+  响应报头(response headers)

+  实体


## 作用域信息

Web技术基础：HTTP应用协议、URL命名标准、XML标记语言

Web设计原则(REST)：表示性状态转移(Representation State Transfer)

ROA：面向资源架构(Resource-Oriented Architecture)

RESTful web services:REST式Web服务



HTTP方法(HTTP method):GET 获取、HEAD、PUT 改写、DELETE 删除、POST

路径(path):URI里主机名(hostname)后面的部分

请求报头(request headers)：是一组键值对(key-value pairs)，起元数据的(metadata)作用。有8个报头：Host、User-Agent、Accept、Content-Type

实体主体(entity-body):

## 相互竞争的架构

`RPC`式架构：方法信息和作用域信息都在信封(envelop)或报头(headers)里。通常从客户端收到一个充满数据的信封，然后发回一个同样充满数据的信封。

`REST`式、面向资源的架构：REST架构意味，方法信息(method information)都在HTTP方法里；ROA意味，作用域(scoping information)都在URI里。



WS-*

这些标准定义各种特定用途的SOAP报头

WSDL(Web Service Description Language,Web 服务描述语言)：用于描述SOAP Web服务的XML词汇。

WADL(Web Application Description Language,Web 应用描述语言)：用于描述REST Web服务的XML词汇。


## 面向资源的架构

REST是一种设计原则。

REST式架构-面向资源的架构(Resource-Oriented Architecture,ROA)

ROA是一种把实际问题转换成REST式Web服务的方法：它令URI、HTTP和XML具有和其他Web应用一样的工作方式。

ROA特性：

+	可寻址性(addressability)

+	无状态性(statelessness)

+	连通性(connectedness)

+	统一接口(uniform interface)

## URIs

+  URI应具有描述性

+  URI跟资源的关系

  +	任何两个资源都不可能是同一个

  +	两个不同的资源在某一时刻可能指向同一个数据。

+  可寻址性

+  无状态性

  +  每个HTTP请求都是完全孤立的。当客户端发出一个HTTP请求时，请求里包含服务器实现该请求的全部信息。

  +  无状态的特性，在负载均衡(load-balanced)服务器上分配无状态的应用将容易许多：请求之间没有没有相互依赖，要提升规模只需往负载均衡系统中添置更多的服务器即可。

  +  对无状态的应用缓存也比较容易。

  +  改变无状态性，最常用的方法是利用HTTP会话(session)。当用户首次访问网站时，会得到一个唯一的字符串，用以标识在该网站上的会话。这个标识是服务器端某个数据结构中的key，通过这个key，可以在服务器端查到请求的状态信息。

  +  状态分为两种：应用状态和资源状态，前者应该保存在客户端，后者应该保存在服务端。

+  表示

  +  资源是表示的来源，表示只是资源当前状态的一些数据。

  +  当一个资源有多个表示时，推荐为一个资源的多个表示设置不同的URI。例如：同一个文章不同语言的表示。

  +  通过设置Accept可以指定表示的首先格式。例如：Accept-Language。但尽量将更多的信息放在URI里。

+  连接与连通性

  +	表示可以是系列化的数据结构
  +	大多数的REST式服务里表示是超媒体，也就是说文档中不仅包含数据(data)，还包含指向其他资源的链接。

+  统一接口

  HTTP四种基本方法：

  1.	获取资源的一个表示 *HTTP GET* 。
  2.	创建一个新资源：向新的 *URI* 发送 *HTTP PUT* ，或向一个 *URI* 发送 *HTTP POST* 。
  3.	修改已有的资源：向已有的 *URI* 发送 *HTTP PUT*
  4.	删除已有资源： *HTTP DELETE*

  其他：

  5.	获取一个只包含元数据的表示： *HTTP HEAD* 。HEAD请求就是比GET请求少返回实体主体而已。
  6.	查看一个资源支持哪些HTTP方法：  *HTTP OPTIONS*

## `POST`与`PUT`

在一个REST式设计中，POST通常被用于创建从属资源。

PUT与POST的区别在于：假如是客户端负责决定新资源采用什么URI，就用PUT；假如服务器负责新资源采用什么URI，就用POST。

PUT和POST动作

|					|	向新资源发PUT请求	|	向已有资源发PUT请求	|	POST			|
|-------			|-----------------		|-------------------	|----				|
|/weblogs			|N/A(资源已存在)		|无效果					|创建一个新的博客	|
|/weblogs/myweblog	|创建该博客				|修改该博客的设置		|往博客里添加一篇文章|
|/weblogs/myweblog/entries/|N/A(无法知道这个URI)|编辑该博客文章	|为该博客文章添加评论	|



## 安全性和等幂性

GET和HEAD请求应是安全的。

GET、HEAD、PUT、DELETE请求应是等幂的。等幂性实践要求是：不要允许客户端对资源做相对的改动。


## 设计只读的面向资源服务
 
1.  规划数据集

2.  把数据集划分为资源

3.  用URI为该资源命名

4.  设计发给客户端的表示

5.  用超链接和表单把该资源与已有的资源联系起来

6.  考虑有哪些典型的事件经过

7.  考虑可能出现哪些错误情况

把数据作为HTTP资源来发布：

+  一个资源就是任何值得作为超链接的目标事物。

+  成为REST式网站，其资源的命名是有意义的，其资源的表示是整齐有序、且可以通过HTTP GET 来访问的。

+  整齐有序的表示易于被解析和屏幕抓取。有意义的命名，令资源易于被程序引用。

+  一个资源是某个事物，用面向对象的方法来设计资源。

+   一个资源只暴露一个统一的接口，最多支持六中HTTP方法，这些方法只允许创建(PUT或POST)、修改(PUT)、读取(GET)和删除(DELETE)这些最基本的操作。如果需要，可以通过重载POST来扩展接口，把一个资源变成一个小型RPC式消息处理器

服务暴露的资源可分为三类：

1.  为特别目的专门预定义的一次性资源。包括可用资源的最上层目录。大多数服务几乎不暴露一次性资源。例如：桶列表

2.  服务暴露的每一个**对象**所对应的资源。

3.  代表在数据集上执行的**算法结果**的资源

命名资源：URI设计有三条基本原则

1.  用路径变量来表达层次结构：`/parent/child`。

2.  在路径变量里加上标点符号，消除误解：`/parent/child1;child2`。建议：当作用域信息的次序有关紧要时，用逗号；否则用分号。

3.  用查询变量来表达算法的输入：`/search?q=jellyfish&start=20`


总结:

REST式Web服务通过资源来暴露数据(data)和算法(algorithms)。有关数据的资源常常构成一个层次结构：由很少的资源开始，然后逐渐扩展为具有许多叶节点。

例如:S3桶列表(bucket list)包含各个桶，而各个桶又包含对象。

理解“把算法暴露为资源”，不要从动作方面来考虑，而是从该动作的结果方面来考虑。


## 设计可读写的面向资源的服务

1.	规划数据集

2.	把数据集划分为资源

3.	用URI为该资源命名

4.	暴露一个统一接口子集

5.	设计来自客户端的表示

6.	设计发给客户端的表示

7.	用超链接和表单把资源与已有资源联系起来

8.	考虑有哪些典型的事件经过

9.	可虑可能出现哪些错误情况

关键点：

+  认证：把请求跟用户关联起来。
    +  认证方案：
      1.	HTTP基本验证(HTTP Basic)。
      2.	HTTP摘要认证(HTTP Digest)。
      3.	WSSE认证。

+  授权：确定一个给定用户可以做哪些操作。

+  保密性：数据要经过一系列的计算机才能最终抵达客户端，需要防止数据不被窃取。通常使用SSL对HTTP进行加密。采用HTTPS可以防止其他计算机窃听客户端与服务器之间的对话。

+  表单编码：

  媒体类型(application/x-www-formurlencoded)。有时被称为"CGI转义"。
  +	URI中使用URI-encoding编码，例如：URI.escape
  +	表单中使用form-encoding编码，例如：CGI::escape


## REST和ROA最佳实践

+  安全性和等幂性：

  +	GET和HEAD请求应该是安全的。

  +	PUT或DELETE请求应该是等幂的。向一个URI发送多次PUT或DELETE请求，应该和只做过一次请求的效果一样。应避免使用PUT请求对资源状态作相对的改动。

+  新建资源：PUT vs POST

  +	只有当客户端能够自己构造出新的URI时，才能够使用PUT来创建资源。

+  重载POST

  +	用于为Web浏览器这种不支持PUT和DELETE的客户端模拟HTTP统一接口。

  +	绕过URI的最大长度限制。

+  需要考虑的问题：

  +	异步操作

  +	批量操作

  +	事物

  +	认证

  +	压缩

  +	缓存

  +	Cookies

## 服务的技术构件表示格式：

+	XHTML(application/xhtml+xml)

+	Atom(application/atom+xml)发布协议

+	SVG(image/svg+xml)

+	表单编码的 *关键字-值对* (application/x-www-form-urlencoded)

+	JSON (application/json)

+	特定框架的序列化格式(application/xml)

## REST式服务框架

+	Ruby on Rails

+	Restlet:建立REST概念的Java类之间的映射

+	Django：方便用Python来开发Web应用和Web服务