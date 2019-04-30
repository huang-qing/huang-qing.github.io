---
layout:     post
title:      使用RESTful创建多媒体APIS
subtitle:   RESTful Building Hypermedia APIS-Building Hypermedia APIs width HTML and Node
date:       2017-04-14
author:     huangqing
header-img: img/post-bg-restful.jpg
catalog: true
categories: [RESTful]
tags:
    - RESTful   
    - Book 
---


# Building Hypermedia APIs width HTML and Node

## 理解超媒体

`HTTP`：`HTTP`是传输协议。从最初的基于文件的传输协议扩展为更为通用的协议，使用了`MIME`(Multipurpose Internet Mail Extensions,多用途互联网邮件扩展)
媒体类型，除了可以建立文件镜像，还能表示数据。`MIME`类型标准使`HTTP`传输能支持一系列的数据格式，并提供了支持新媒体类型的能力。

`MIME`：`MIME`是媒体类型标准。例如：`Content Type:application/xml`

----

## 解决的问题：
+ 更新服务端Web API 后，需要要更新客户端代码，否则客户端能按预期方式工作。
+ 将长期使用的服务端应用迁移的新的`DNS`名称时，需要重写所以得API文档并更新现有客户端代码。
+ 在服务器端应用程序内实现了新的处理流程或工作流程，客户端遇到新规则时出现问题，忽略了新规则，执行的代码在服务器上创建出无效的结果。

----

## 类型封送：    
将内部对象(如"customer"、"order"和"product")，序列化到一种常见数据格式中，然后在客户端和服务器之间来回传递。
它们是以服务器为中心的，可以满足服务器的需求，但客户端程序通常需要自己解决各种问题；
以保存在服务器上的内部类型为中心，给独立于服务器的代码构建的客户端代码带来了风险，任何对内部对象的修改都很容易改变公共接口。

共享模式：发布详细列出参数和交互细节的模式文档。SOAP设计采用的此模式。
+ 优点：清楚的描述了所有的参与方。
+ 缺点：对象发生变化后，客户端必须重新构建。

URI构建：让URI携带类型信息。
+ 优点：大部分框架可以从源代码内的元数据生成这些URI
+ 缺点：失去了对公共URI空间的控制。利用URI携带信息，意味URI是由私有的源代码决定。源代码的任何修改都会威胁到之前发布URI的有效性。例如:`http://www.example.org/order/123`

----

## 超媒体解决方案
超媒体不仅包括保存在服务器上的数据，还包括关于数据本身的元数据和当前时刻修改应用状态可能选项的元数据。

### 超媒因子（H-Factors）：

![Hypermedia_Factors](/images/restful/Hypermedia_Factors.png) 

+ 链接因子：客户端与服务器之间的具体的链接交换，表示的是客户端推进应用状态的机会。包括导出、嵌入、模板、幂等和非幂等。
+ 控制因子：定制超媒体元数据的详细信息（如HTTP部首），包括读、更新、方法、和链接注释。


### 链接因子：

嵌入链接（LE）：该因子伴随的URI应该使用应用级协议的读操作（如 `HTTP GET`）来解析并引用，将响应结果显示在当前输出窗口之上。

```` html
<img src="..."/>
````

导出链接（LO）：该因子伴随的URI应该使用应用级协议的读操作来解析并引用,且响应的结果是一个完整的显示。

``` html
<a href="...">...</a>
```
``` html
<a href="..." target="_blank">...</a>
```

模板链接（LT）：该因子提供了一种方式，可以指定在执行读操作时提供的一个或多个参数。

``` html
//构建的URI：http://www.example.org/?search=hypermedia
<form method="get" action="http://www.example.org/">
    <input type="text" name="search" value="" />
    <input type="submit" />
</form>
```
``` html
<link href="http://www.example.org/?search={search}" />

```

幂等链接（LI）：该因子为媒体类型提供了一种定义向服务器进行幂等提交的方式。

``` javascript
<script type="text/javascript">
    function delete(id)
    {
        var client = new XMLHttpRequest();
        client.open("DELETE", "http://example.org/comments/"+id);
    }
</script>

```

非幂等链接（LN）：该因子提供了一种使用非幂等“提交”，向服务器发送数据的方法。

``` html 
//提交的表单
<form method="post" action="http://example.org/comments/">
    <textarea name="comment"></textarea>
    <input type="submit" />
</form>
```
``` HTTP Request
POST /comments/ HTTP/1.1
Host: example.org
Content-Type: application/x-www-form-urlencoded
Length: XX
comment=this+is+my+comment
```

### 控制因子

读控制(CR):媒体类型可以控制信息暴露给客户端，方式之一就是支持操纵CR的控制数据。

```HTTP Request Headers
Accept-Language:zh-CN,zh;q=0.8
```

更新控制（CU）：在发送/更新操作期间支持控制数据。

`enctype`
```html
<form method="post" action="http://example.org/comments/" enctype="text/plain">
    <textarea name="comment"></textarea>
    <input type="submit" />
</form>
```
```HTTP
POST /comments/ HTTP/1.1
Host: example.org
Content-Type: text/plain
Length: XX
this+is+my+comment
```

链接注释控制（CL）：提供链接本身内联的元数据。

```html
<link rel="stylesheet" href="..." />
```
----

## 超媒体设计元素

基本的设计元素包括：基本格式、状态转移、领域风格、应用流程。

### 基本格式
任何超媒体设计中都有一个关键元素，基本消息格式。HTTP协议上最常见的的格式是`XML`、`JSON`、`HTML`。

### 状态转移
支持信息（状态）从客户端向服务器的转移。由客户端发起的状态转移是超媒体消息传送的真正核心。
+ 只读转移（`SVG`）
+ 预定义转移
+ 即席转移。这种是最熟悉的，HTML媒体类型依赖于这种设计模式，使用`form`、`input`等元素提供支持。

----

## 领域风格

领域风格包括特定领域风格、一般领域风格、领域无关风格。

### 特定领域风格

```XML
<!-- domain-specific design -->
<order>
<id>...</id>
<shipping-address>...</shipping-address>
<billing-address>...</billing-address>
...
</order>
```

### 一般领域风格

```JSON
/* domain-general design */
{
    "order": {
        "id": "...",
        "address": {
            "type": "shipping",
            "street-address": "..."
        },
        "address": {
            "type": "billing",
            "street-address": "..."
        }
    }
}
```

### 领域无关风格

```html
<!-- domain-agnostic design -->
<ul class="order">
    <li class="id">...</li>
    <ul class="shipping-address">
    <li class="street-address">...</li>
    ...
</ul>
<ul class="billing-address">
    <li class="street-address">...</li>
    ...
</ul>
...
</ul>
```