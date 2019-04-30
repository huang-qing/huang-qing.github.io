---
layout:     post
title:      RESTful Web APIs
subtitle:   
date:       2017-04-13
author:     huangqing
header-img: img/post-bg-restful.jpg
catalog: true
categories: [RESTful]
tags:
    - RESTful   
    - Book 
---




# RESTful Web APIs

## 分布式部署Web API： 
+ SOAP Web Service (Asp.net Web Service、Asp.net WCF)    
+ RESTful Web API （Asp.net Web Api）

----

## 演变：    

PC主导的Web时代，只需一套程序满足PC上浏览器浏览网站即可。移动Web快速发展，系统的后台需要同时支持PC和各种移动设备，
而又只写一套后台程序，这样就需要统一为同一套通用的接口，配合被JavaScript天然支持的JSON传输格式。Web API逐渐流行起来。   
基于HTTP协议的接口是最容易被复用的，基于原始的HTTP协议比SOAP更轻量。
URL风格开始美化，设计简短合理清晰（摒弃了URL中的`update?id=xxx&date=xxx`之类的长篇的&连接变量的风格，而是代之`PUT xxxx/articles/2015/3413`之类优美的URL）  
RESTful Web API 逐渐在竞争实践中占据优势。 

----
## SOAP
>SOAP（Simple Object Access protocol，简单对象访问协议）是微软 .net 架构的关键元素。  
 SOAP是基于 XML 的简易协议，可使应用程序在 HTTP 之上进行信息交换。更简单地说：SOAP 是用于访问网络服务的协议。       
 SOAP提供了一种标准的方法，使得运行在不同的操作系统并使用不同的技术和编程语言的应用程序可以互相进行通信。

### SOAP Web 服务架构
所有的 SOAP 消息发送都使用 HTTP POST 方法，并且所有 SOAP 消息的 URI 都是一样的，这是基于 SOAP 的 Web 服务的基本实践特征。

![SOAP Web 服务架构](/images/restful/SOAP%20Web%20Service%20Architecture.jpg)

----

## REST
>REST（Representational State Transfer，简称REST,表述性状态移交）描述了一个架构样式的网络系统，客户端应用程序随着每个资源表现状态的不同而发生状态转移。   
它是一系列设计约束的集合：无状态性、将超媒体作为应用状态的引擎等。
它首次出现在 2000 年 Roy Fielding 的博士论文中，他是 HTTP 规范的主要编写者之一。
REST倾向于用更加简单轻量的方法设计和实现,并没有一个明确的标准，而更像是一种设计的风格。


### Restful Web 服务架构:
使用 HTTP PUT 增加、修改用户资源，使用 HTTP GET 得到某一具体用户资源，使用 HTTP DELETE 删除用户资源，使用 HTTP GET 得到用户列表资源。

![Restful Web 服务架构](/images/restful/Restful%20Web%20Service%20Architecture.jpg)

----

## Web体系结构

Web有三个核心概念：资源（resource）、URI（Uniform Resource Identifier，统一资源标识符）和表示（representation）

![Restful Web 服务架构](/images/restful/uri-res-rep.png)


## 资源和表述

资源：一切皆可以成为资源，将一个事物赋以URL，就可以成为一个资源    
表述：表述描述资源的状态。可以通过多种的方式进行描述，如JSON、XML、文件。    
资源的多重表述：例如官方文档的多个语言版本。     
  + 通过HTTP报头值区分
  + 通过分配多个URL来区分

## REST基本的概念：
可寻址性：每个URL代表一个资源，每个资源。   

无状态性：HTTP会话只维持在一次请求过程中，客户端发送请求，服务器进行响应。服务器不关心客户端的状态。

自描述消息：HTML页面不仅仅包含请求的内容，还包含指向其他内容的链接。   
![Self-Descriptive Messages](/images/restful/Self-Descriptive%20Messages.png)   

应用状态：客户端保存着应用状态，通过点击链接或提交表单进行应用状态的转变。   
![Application State](/images/restful/Application%20State.png)   

资源状态：服务器保存着资源状态。客户端可以通过HTML表单的方式改变资源状态。  
![Resource State](/images/restful/Resource%20State.png)  

连通性：当前的网页可以获取相邻的网页。  

设计思想的转变：REST是无状态的，而COOKIE，SESSIONID是为会话服务的，在设计一套REST风格的应用时应特别注意。

##REST应用的三级成熟度模型
+ 第一级：REST风格设计，即有清晰的资源对象，每个资源有对应的标识符和表达。  
+ 第二级：对资源的操作使用合适的HTTP谓词。例如：一个订单的URL就是http://xxxx/xxx/orders/201603019870，这个URL就是这个订单的唯一标识；
从无到有创建出来这个订单，POST这个URL；修改这个订单的内容，先GET下来，编辑内容后再PUT回去；删除订单就去DELETE；查询订单的操作权限就去用OPTIONS去访问它。
+ 第三级：REST接口的业务含义变成自我表述，摒弃接口文档。

## HTTP

Request:   
![HTTP Request](/images/restful/HTTP_Request.png)    

Response:   
![HTTP Response](/images/restful/HTTP_Response.png)  

常用的HTTP方法和响应码：     
  1. GET：获取资源的某个表述。响应码：200(OK)成功、300(Moved Permanently)重定向、404(Not Found)、410(Gone)。
  2. DELETE:销毁一个资源。响应码：200(OK)删除成功并返回一条消息、204(No Content)删除成功且没有相关的资源、202(Accepted)稍后删除、404(Not Found)。  
  3. POST:基于给定的表述信息，在当前资源的下一级创建新的资源。响应码：201(Created)、202(Accepted)。
  4. PUT:用给定的表述信息替换资源的当前状态。响应码：200(OK)、204(No Content)。
  5. HEAD:获取服务器发送过来的报头信息。
  6. OPTIONS:获取资源所能响应的HTTP方法列表。

等幂性：每个安全的运算等幂的。具体说就是多次的操作请求不会造成资源状态的多次改变。
  + 等幂：GET、DELETE、PUT
  + 非等幂：POST

## JSON

+ 使用双引号描述字符串：`"this is a string"`    
+ 使用方括号描述数组：`[1,2,3]`    
+ 使用花括号描述对象(键值对的集合)：`{"key":"value"}`  

##　媒体类型：

Content-Type: application/json    
Content-Type: application/vnd.collection+json   

``` javascript
//a JSON document:
[{
    text: "Test.",
    date_posted: "2013-04-22T05:33:58.930Z"
}, {
    text: "Hello.",
    date_posted: "2013-04-20T12:55:59.685Z"
}];

```

``` javascript
//a Collection+JSON document: 集合模式规范示例
{
    "collection": {
        "version": "1.0",
        //一个指向集合本身的永久链接
        "href": "http://www.youtypeitwepostit.com/api/",
        //包含指向集合成员的链接和表述
        "items": [{
            //指向子项的永久链接
            "href": "/api/messages/21818525390699506",
            //任何其他的信息
            "data": [{
                "name": "text",
                "value": "Test."
            }, {
                "name": "date_posted",
                "value": "2013-04-22T05:33:58.930Z"
            }],
            //指向子项相关的其他资源的超媒体链接
            "links": []
        }, {
            "href": "/api/messages/3689331521745771",
            "data": [{
                "name": "text",
                "value": "Hello."
            }, {
                "name": "date_posted",
                "value": "2013-04-20T12:55:59.685Z"
            }],
            "links": []
        }],
        "links": [{
            "href": "/logo.png",
            "rel": "icon",
            "render": "image"
        }],
        //用于搜索集合的超媒体控件
        "queries": [{
            "href": "/api/search",
            "rel": "search",
            "prompt": "Search the microblog archives",
            "data": [{
                "name": "query",
                "value": ""
            }]
        }],
        //用于向集合添加子项的超媒体控件
        "template": {
            "data": [{
                "prompt": "Text of message",
                "name": "text",
                "value": ""
            }]
        }
    }
}
```

