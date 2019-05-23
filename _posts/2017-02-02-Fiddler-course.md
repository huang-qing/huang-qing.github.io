---
layout:     post
title:      Fiddler 教程
subtitle:   
date:       2017-02-02
author:     huangqing
header-img: img/post-bg-fiddler.jpg
catalog: true
categories: [Web]
tags:
    - fiddler
---

# Fiddler教程

## 简介

Fiddler（中文名称：小提琴）是一个HTTP的调试代理，以代理服务器的方式，监听系统的Http网络数据流动，Fiddler可以也可以让你检查所有的HTTP通讯，设置断点，以及Fiddle所有的“进出”的数据（我一般用来抓包）,Fiddler还包含一个简单却功能强大的基于JScript .NET事件脚本子系统，它可以支持众多的HTTP调试任务。

Fiddler官方网站提供了大量的帮助文档和视频教程,这是学习Fiddler的最好资料

+  [Fiddler_官方网站](http://www.telerik.com/fiddler)

+  [Fiddler_官方文档](http://docs.telerik.com/fiddler/configure-fiddler/tasks/configurefiddler)

+  [Fiddler_官方视频](https://www.youtube.com/playlist?list=PLvmaC-XMqeBbw72l2G7FG7CntDTErjbHc)

+  [Fiddler_官方插件](http://www.telerik.com/fiddler/add-ons)

## 工作原理

![工作原理](/images/fiddler/947566-f51654e6f0018748.jpg)

Fiddler是以代理WEB服务器的形式工作的,浏览器与服务器之间通过建立TCP连接以HTTP协议进行通信，浏览器默认通过自己发送HTTP请求到服务器，它使用代理地址:127.0.0.1, 端口:8888. 当Fiddler开启会自动设置代理， 退出的时候它会自动注销代理，这样就不会影响别的程序。不过如果Fiddler非正常退出，这时候因为Fiddler没有自动注销，会造成网页无法访问。解决的办法是重新启动下Fiddler。

## 主界面

![主界面](/images/fiddler/947566-66f000394932159c.jpg)

Fiddler的主界面分为 工具面板、会话面板、监控面板、状态面板

## 工具面板

![工具面板](/images/fiddler/947566-7391db3bfd1b3de5.jpg)

说明注释、重新请求、删除会话、继续执行、流模式/缓冲模式、解码、保留会话、监控指定进程、寻找、保存会话、切图、计时、打开浏览器、清除IE缓存、编码/解码工具、弹出控制监控面板、MSDN、帮助

两种模式

+  缓冲模式（Buffering Mode）Fiddler直到HTTP响应完成时才将数据返回给应用程序。可以控制响应，修改响应数据。但是时序图有时候会出现异常

+  流模式（Streaming Mode）Fiddler会即时将HTTP响应的数据返回给应用程序。更接近真实浏览器的性能。时序图更准确,但是不能控制响应。

## 会话面板

![会话面板](/images/fiddler/947566-88573a5260342401.jpg)

![会话面板图标](/images/fiddler/947566-5fbf69350a526432.jpg)

## 监控面板

![监控面板](/images/fiddler/947566-2a17b3b31567aae5.jpg)

### 统计报表

1.   请求总数、请求包大小、响应包大小。

2.  请求起始时间、响应结束时间、握手时间、等待时间、路由时间、TCP/IP、传输时间。

3.  HTTP状态码统计。

4.  返回的各种类型数据的大小统计以及饼图展现。

![统计报表](/images/fiddler/947566-8a49bfdfd17d7ef5.jpg)

### 时间轴

每个网络请求都会经历域名解析、建立连接、发送请求、接受数据等阶段。把多个请求以时间作为 X 轴，用图表的形式展现出来，就形成了瀑布图。在Fiddler中，只要在左侧选中一些请求，右侧选择Timeline标签，就可以看到这些请求的瀑布图

![时间轴](/images/fiddler/947566-9f85bc1a0bfaf113.jpg)

+  绿色的请求表示这是一个“有条件的请求”。HTTP 协议定义了 5 个条件请求头部，最常见的两个是“If-Modified-Since”和“If-None-Match”。服务器根据这两个头部来验证本地缓存是否过期，如果过期则正常返回资源的最新版本；否则仅返回 304 Not Modified，浏览器继续使用本地缓存。包含条件请求头部的请求用绿色显示，否则用黑色。

+  有阴影线的请求是缓冲模式下的请求，实心的是流模式下的请求。Fiddler 提供了缓冲（Buffering）和流（Streaming）两种抓包模式：缓冲模式下，Fiddler 会在响应完成时才将数据返回给应用程序（通常是浏览器），这种模式下可以控制响应，方便地修改响应内容；流模式下，Fiddler 会实时返回响应数据给浏览器，但没办法控制响应。一般使用流模式，瀑布图会更真实一些。这两种模式可以通过 Fiddler 的工具栏选择。特别的，通过 Fiddler 的“AutoResponder”功能返回的响应，只能是缓冲模式。

+  请求条的不同颜色对应着不同类型的响应，根据响应头的 MIME Type 来归类。如浅绿色表示图片类型的响应；深绿色是 JavaScript；紫色是 CSS；其它都是蓝色。

+  请求中的黑色竖线，表示的是浏览器收到服务端响应的第一个字节这一时刻。这个时间受 DNS 解析、建立连接、发送请求、等待服务端响应等步骤的影响。

+  请求条后面的图标表示响应的某些特征。如软盘图标表示这个响应正文从本地获得，也就是说服务端返回了 304；闪电表示这是 Fiddler 的“AutoResponder”的响应；向下的箭头表示响应是 302，需要重定向；红色感叹号说明这个请求有错误发生（状态码是 4XX 或 5XX）。特别的，如果请求条后面有一个红色的X，说明服务端响应完这个请求之后，断开了连接。出现这种情况一般有两种可能：HTTP/1.0 的响应中没有 Connection: Keep-Alive；或者是 HTTP/1.1 的响应中包含了 Connection: close。使用持久连接可以省去建立连接的开销，也可以减小 TCP 慢启动和其它拥塞控制机制带来的影响，总之是好处多多。

+  请求前面的红色圆圈表示这个连接是新建的，绿色表示是复用的。上面的圆圈表示的是浏览器到 Fiddler 的连接，下面的圆圈是 Fiddler 到服务端的连接。

## 状态面板

![状态面板](/images/fiddler/947566-7ecf91e24cd75e9a.jpg)

控制台Fiddler的左下角有一个命令行工具叫做QuickExec,允许你直接输入命令。

常见得命令有：

|  命令    |  解释    |
|  ----   |----    |
|help        |   打开官方的使用页面介绍，所有的命令都会列出来         |
|cls          |   清屏 (Ctrl+x 也可以清屏)         |
|select     |   选择会话的命令         |
|?.png      |   用来选择png后缀的图片         |
|bpu         |   截获request         |
|bpafter    |   截获response         |

## Request消息的结构

![Request消息的结构](/images/fiddler/947566-1733be471914d8af.png)

## Response消息的结构

![Response消息的结构](/images/fiddler/947566-f94b5d9daa376a92.png)


## 常用功能

### 监听HTTPS

  Fiddler不仅能监听HTTP请求而且默认情况下也能捕获到HTTPS请求，Tool -> Fiddler Option -> HTTPS下面进行设置，勾选上“Decrypt HTTPS traffic”，如果不必监听服务器端得证书错误可以勾上“Ignore server certification errors”，也可以跳过几个指定的HOST来缩小或者扩大监听范围。

  ![监听HTTPS](/images/fiddler/947566-e027d3b3aa81481a.jpg)

### HOST切换

  ![HOST](/images/fiddler/947566-bddcc793315929c4.jpg)

### 模拟各类场景

+  通过GZIP压缩，测试性能

+  模拟Agent测试，查看服务端是否对不同客户端定制响应

+  模拟慢速网络，测试页面的容错性

+  禁用缓存，方便调试一些静态文件或测试服务端响应情况

+  根据一些场景自定义规则

![自定义规则](/images/fiddler/947566-9cf37f9065825855.png)

低网速模拟有时出于兼容性考虑或者对某处进行性能优化，在低网速下往往能较快发现问题所在也容易发现性能瓶颈，可惜其他调试工具没能提供低网速环境，而强大的Fiddler考虑到了这一点，能够进行低网速模拟设置Rules > Performance > Stimulate Modem Speeds。

### Compare（对比文本）

![对比文本](/images/fiddler/947566-6d65871b7b731fe6.jpg)

### Composer（构造器）

请求构造顾名思义就是我们可以模拟请求，也就是说我们可以借助Fiddler的Composer 在不改动开发环境实际代码的情况下修改请求中的参数值并且方便的重新调用一次该请求，然后相比较2次请求响应有何具体不同。任何一个请求参数只要是合法的取值再次调用后都会有相应的响应，那么你想要的任意一个合法请求组合自然也能够按照你的意愿构造出来，然后再次调用以及查看返回数据。
  
![Paste_Image.png](/images/fiddler/947566-c128f0a0ede64953.png)

将该请求鼠标左键单击拖入Fiddler右侧Request Builder标签内并修改原请求参数OutPutType=JSON为OutPu tType=XML，然后点击Execute按钮再次触发调用请求
  
![Paste_Image.png](/images/fiddler/947566-5aaac0b84a397c58.png)

双击这次请求包在Inspectors标签下查看返回数据为XML格式，而JSON格式一栏为空：
    
![Paste_Image.png](/images/fiddler/947566-101fc07c0290469f.png)

### Filters(过滤监控)

对一个重新载入的页面进行抓包，如果包的条目过多而你需要关注的就那么几项的话，可以使用Fiddler的过滤器Filters进行抓包，那么抓包时只会抓取你希望抓到的那些包。切换到Filters标签勾选Use filter，以便激活过滤器，这样下面的各种过滤方式就可以进行选择了。

![Filter_1](/images/fiddler/947566-267b4814eec8801d.jpg)

![Filter_2](/images/fiddler/947566-b6ed61685e13315e.jpg)

### AutoResponder(请求重定向)

所谓请求无非就是需要调用到的一些资源(包括JS、CSS和图片等)，所谓重定向就是将页面原本需要调用的资源指向其他资源(你能够控制的资源或者可以引用到的资源)。

1.  你可以将前台服务器的诸多或者某个资源在本地做个副本，如果正常网络访问环境下该资源出现了BUG而导致开发环境崩溃时，可以先将这个资源的请求重定向到本地副本，这样就可以继续进行开发调试你的页面，从而大量节省资源维护的等待时间。

2.  你也可以将多人同时维护的某个JS文件复制一份出来在本地，当你的开发调试收到他人调试代码干扰时，可以将这个JS的调用重定向到本地无干扰的JS文件，进行无干扰开发，功能开发完成并调试OK之后再将你的代码小心合入到开发环境中，这样就可以避免受到他人干扰专心搞你的模块开发，也就是说能够将JS文件脱离开发环境却不影响线上调试。

3.  你还可以将样式文件或者图片指向本地。

![重定向](/images/fiddler/947566-18f9c105596ef543.jpg)

## 移动端抓包

Fiddler不但能截获各种浏览器发出的HTTP请求, 也可以截获各种智能手机发出的HTTP/HTTPS请求。

Fiddler能捕获IOS,Andriod,WinPhone,设备发出的请求，同理，也可以截获IPad, MacBook的等设备发出的HTTP/HTTPS。

前提条件是：安装Fiddler的机器，跟Iphone 在同一个网络里， 否则IPhone不能把HTTP发送到Fiddler的机器上来。

具体操作步骤如下：

+  Fiddler设置打开Fiddler, Tools-> Fiddler Options。（配置完后记得要重启Fiddler）

+  选中"Allow remote computers to connect". 是允许别的机器把HTTP/HTTPS请求发送到Fiddler上来!
[APP](/images/fiddler/947566-234095a4e258e508.jpg)

+  获取Fiddler所在机器的IP

+  安装Fiddler证书这一步是为了让Fiddler能捕获HTTPS请求。 如果你只需要截获HTTP请求， 可以忽略这一步

+  首先要知道Fiddler所在的机器的IP地址：　假如我安装了Fiddler的机器的IP地址是:192.168.1.104打开IPhone 的Safari, 访问 [http://192.168.1.104:8888](http://192.168.1.104:8888/)， 点"FiddlerRoot certificate" 然后安装证书

![APP_1](/images/fiddler/947566-aefb93710584c743.jpg)

![APP_2](/images/fiddler/947566-c0347d14cf821503.jpg)

+  打开IPhone, 找到你的网络连接， 打开HTTP代理， 输入Fiddler所在机器的IP地址(比如:192.168.1.104) 以及Fiddler的端口号8888!
[APP_3](/images/fiddler/947566-08322c2d57e9392a.jpg)

## 参考资料

+  [Fiddler 教程](http://www.cnblogs.com/FounderBox/p/4653588.html?utm_source=tuicool&utm_medium=referral)