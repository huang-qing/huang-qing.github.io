---
layout:     post
title:      浏览器缓存简明教程
subtitle:   
date:       2017-05-11
author:     huangqing
header-img: img/post-bg-chrome.jpg
catalog: true
categories: [chrome]
tags:
    - chrome
---

# 浏览器缓存简明教程

## 简单说明

浏览器缓存就是当你打开一个网页，浏览器会自动下载副本到你电脑上，就相当于你另存为网页到某个地方而已，只不过这里是自动而已。
当然不是浏览器能把各种 网页都能下载到本地电脑上，它是有特殊情况。一般html，后者request是`get`请求，而`post`一般不缓存。

当然客户端缓存是否需要是可以在服务端代码上控制的。那就是响应头。

响应头告诉缓存器不要保留缓存，缓存器就不会缓存相应内容；

如果请求信息是需要认证或者安全加密的，相应内容也不会被缓存；

校验参数非常重要，如果回应中1个参数都不存在，并且没有任何信息说明保鲜期（`Expires`或`Cache-Control`）的情况下，缓存将不会存储任何副本；

最常见的校验参数是文档的最后修改时间，通过最后Last-Modified头信息可以，当一份缓存包含`Last-Modified`信息，他基于此信息，通过添加一个`If-Modified-Since`请求参数，向服务器查询：这个副本从上次查看后是否被修改了。 

HTTP 1.1介绍了另外一个校验参数： `ETag`，服务器是服务器生成的唯一标识符`ETag`，每次副本的标签都会变化。
由于服务器控制了`ETag`如何生成，缓存服务器可以通过`If-None-Match`请求的返回没变则当前副本和原件完全一致。 

所有的缓存服务器都使用`Last-Modified`时间来确定副本是否够新，而`ETag`校验正变得越来越流行。

响应头如果是POST模式递交数据，则返回的页面大部分不会被浏览器缓存，如果你发送内容通过URL和查询（通过GET模式），则返回的内容可以缓存下来供以后使用。

HTTP协议中关于缓存的信息头关键字包括`Cache-Control(HTTP1.1)`，`Pragma(HTTP1.0)`，`last-Modified`，`Expires`等。


![浏览器缓存](/images/http/13170445_SwOF.jpg)

## 缓存控制头 Cache-Control

Cache-Control 是最重要的规则。这个字段用于指定所有缓存机制在整个请求/响应链中必须服从的指令。这些指令指定用于阻止缓存对请求或响应造成不利干扰的行为。这些指令 通常覆盖默认缓存算法。缓存指令是单向的，即请求中存在一个指令并不意味着响应中将存在同一个指令。

表 1. 常用 cache-directive 值

|Cache-directive	| 说明|
|----|----|
|public	|所有内容都将被缓存|
|private	|内容只缓存到私有缓存中|
|no-cache	|所有内容都不会被缓存|
|no-store	|所有内容都不会被缓存到缓存或 Internet 临时文件中|
|must-revalidation/proxy-revalidation	|如果缓存的内容失效，请求必须发送到服务器/代理以进行重新验证|
|max-age=xxx (xxx is numeric)	|缓存的内容将在 xxx 秒后失效, 这个选项只在HTTP 1.1可用, 并如果和Last-Modified一起使用时, 优先级较高|


表 2. 对 cache-directive 值的浏览器响应

|Cache-directive	|打开一个新的浏览器窗口	|在原窗口中单击`Enter`按钮	|刷新	|单击`Back`按钮|
|----|----|----|-----|-----|-----|
|public	|浏览器呈现来自缓存的页面	|浏览器呈现来自缓存的页面	|浏览器重新发送请求到服务器	|浏览器呈现来自缓存的页面|
|private	|浏览器重新发送请求到服务器	|第一次，浏览器重新发送请求到服务器；此后，浏览器呈现来自缓存的页面	|浏览器重新发送请求到服务器	|浏览器呈现来自缓存的页面|
|no-cache/no-store	|浏览器重新发送请求到服务器	|浏览器重新发送请求到服务器	|浏览器重新发送请求到服务器	|浏览器重新发送请求到服务器
|must-revalidation/proxy-revalidation	|浏览器重新发送请求到服务器	|第一次，浏览器重新发送请求到服务器；此后，浏览器呈现来自缓存的页面	|浏览器重新发送请求到服务器	|浏览器呈现来自缓存的页面|
|max-age=xxx (xxx is numeric)	|在 xxx 秒后，浏览器重新发送请求到服务器	在 xxx 秒后，浏览器重新发送请求到服务器	|浏览器重新发送请求到服务器	|在 xxx 秒后，浏览器重新发送请求到服务器|

`Cache-Control`是关于浏览器缓存的最重要的设置，因为它覆盖其他设置，比如 `Expires` 和 `Last-Modified`。另外，由于浏览器的行为基本相同，这个属性是处理跨浏览器缓存问题的最有效的方法。


## Last-Modified

在浏览器第一次请求某一个URL时，服务器端的返回状态会是200，内容是你请求的资源，同时有一个Last-Modified的属性标记(HttpReponse Header)此文件在服务期端最后被修改的时间，格式类似这样：

~~~http
Last-Modified:Tue, 24 Feb 2009 08:01:04 GMT
~~~

客户端第二次请求此URL时，根据HTTP协议的规定，浏览器会向服务器传送If-Modified-Since报头(HttpRequest Header)，询问该时间之后文件是否有被修改过：
~~~http
If-Modified-Since:Tue, 24 Feb 2009 08:01:04 GMT
~~~

如果服务器端的资源没有变化，则自动返回HTTP304（NotChanged.）状态码，内容为空，这样就节省了传输数据量。当服务器端代码发生改变或者重启服务器时，则重新发出资源，返回和第一次请求时类似。从而保证不向客户端重复发出资源，也保证当服务器有变化时，客户端能够得到最新的资源。
注：如果If-Modified-Since的时间比服务器当前时间(当前的请求时间request_time)还晚，会认为是个非法请求。


## Etag工作原理

HTTP协议规格说明定义ETag为"被请求变量的实体标记”。简单点即服务器响应时给请求URL标记，并在HTTP响应头中将其传送到客户端，类似服务器端返回的格式：

```http response
Etag:"5d8c72a5edda8d6a:3239"
```
客户端的查询更新格式是这样的：

```http
If-None-Match:"5d8c72a5edda8d6a:3239"
```

如果ETag没改变，则返回状态304。

即:在客户端发出请求后，HttpReponse Header中包含`Etag:"5d8c72a5edda8d6a:3239"`标识，等于告诉Client端，你拿到的这个的资源有表示`ID：5d8c72a5edda8d6a:3239`。

当下次需要发Request索要同一个URI的时候，浏览器同时发出一个`If-None-Match`报头(Http RequestHeader)此时包头中信息包含上次访问得到的`Etag:"5d8c72a5edda8d6a:3239"`标识。

```http
If-None-Match:"5d8c72a5edda8d6a:3239"
```

这样，Client端等于Cache了两份，服务器端就会比对2者的etag。如果`If-None-Matc`h为`False`，不返回200，返回`304(Not Modified)` Response。

## 过期头 (`Expires`)

Expires 头部字段提供一个日期和时间，响应在该日期和时间后被认为失效。
失效的缓存条目通常不会被缓存（无论是代理缓存还是用户代理缓存）返回，除非首先通过原始 服务器（或者拥有该实体的最新副本的中介缓存）验证。
（注意：`cache-control` `max-age` 和 `s-maxage` 将覆盖 `Expires` 头部。）

需和Last-Modified结合使用。用于控制请求文件的有效时间，当请求数据在有效期内时客户端浏览器从缓存请求数据而不是服务器端.
当缓存中数据失效或过期，才决定从服务器更新数据。

Expires 字段接收以下格式的值：

~~~http
Expires: Sun, 08 Nov 2009 03:37:26 GMT
~~~

如果查看内容时的日期在给定的日期之前，则认为该内容没有失效并从缓存中提取出来。反之，则认为该内容失效，缓存将采取一些措施。

## Last-Modified和Expires

Last-Modified标识能够节省一点带宽，但是还是逃不掉发一个HTTP请求出去，而且要和Expires一起用。

而Expires标识却使得浏览器干脆连HTTP请求都不用发。
比如当用户F5或者点击Refresh按钮的时候就算对于有Expires的URI，一样也会发一个HTTP请求出去，所以，Last-Modified还是要用的，而且要和Expires一起用。


## Etag和Expires

如果服务器端同时设置了Etag和Expires时，Etag原理同样，即与Last-Modified/Etag对应的HttpRequestHeader:If-Modified-Since和If-None-Match。

我们可以看到这两个Header的值和WebServer发出的Last-Modified,Etag值完全一样；
在完全匹配If-Modified-Since和If-None-Match即检查完修改时间和Etag之后，服务器才能返回304.


## Last-Modified和Etag

**分布式系统里多台机器间文件的last-modified必须保持一致，以免负载均衡到不同机器导致比对失败。**

**分布式系统尽量关闭掉Etag(每台机器生成的etag都会不一样)。**

Last-Modified和ETags请求的http报头一起使用，服务器首先产生Last-Modified/Etag标记，服务器可在稍后使用它来判断页面是否已经被修改，来决定文件是否继续缓存

过程如下:

1. 客户端请求一个页面（A）。
2. 服务器返回页面A，并在给A加上一个Last-Modified/ETag。
3. 客户端展现该页面，并将页面连同Last-Modified/ETag一起缓存。
4. 客户再次请求页面A，并将上次请求时服务器返回的Last-Modified/ETag一起传递给服务器。
5. 服务器检查该Last-Modified或ETag，并判断出该页面自上次客户端请求之后还未被修改，直接返回响应304和一个空的响应体。

注：

1. Last-Modified和Etag头都是由WebServer发出的HttpReponse Header，WebServer应该同时支持这两种头。
2. WebServer发送完Last-Modified/Etag头给客户端后，客户端会缓存这些头；
3. 客户端再次发起相同页面的请求时，将分别发送与Last-Modified/Etag对应的HttpRequestHeader:If-Modified-Since和If-None-Match。我们可以看到这两个Header的值和WebServer发出的Last-Modified,Etag值完全一样；
4. 通过上述值到服务器端检查，判断文件是否继续缓存；


## Cache-Control: max-age=秒 和 Expires

Expires = 时间，HTTP 1.0 版本，缓存的载止时间，允许客户端在这个时间之前不去检查（发请求）

max-age = 秒，HTTP 1.1版本，资源在本地缓存多少秒。

如果max-age和Expires同时存在，则被Cache-Control的max-age覆盖。

Expires 的一个缺点就是，返回的到期时间是服务器端的时间，这样存在一个问题，如果客户端的时间与服务器的时间相差很大，那么误差就很大，所以在HTTP 1.1版开始，使用Cache-Control: max-age=秒替代。

Expires =max-age +   “每次下载时的当前的request时间”

所以一旦重新下载的页面后，expires就重新计算一次，但last-modified不会变化


## 不进行任何缓存相关设置

如果您不定义任何缓存相关设置，则不同的浏览器有不同的行为。有时，同一个浏览器在相同的情形下每次运行时的行为都是不同的。
情况可能很复杂。另外，有些不该缓存的内容如果被缓存，将会导致安全问题。 



## 关键结论

|操作	|行为|
|----|----|
|打开新窗口	|如果指定`cache-control`的值为`private`、`no-cache`、`must-revalidate`,那么打开新窗口访问时都会重新访问服务器。而如果指定了 `max-age`值,那么在此值内的时间里就不会重新访问服务器,例如：`Cache-control: max-age=5` 表示当访问此网页后的5秒内再次访问不会去服务器.|
|在地址栏回车	|如果值为`private`或`must-revalidate`,则只有第一次访问时会访问服务器,以后就不再访问。如果值为`no-cache`,那么每次都会访问。如果值为`max-age`,则在过期之前不会重复访问。|
|按后退按扭	|如果值为`private`、`must-revalidate`、`max-age`,则不会重访问,而如果为`no-cache`,则每次都重复访问.|
|按刷新按扭	|无论为何值,都会重复访问.|