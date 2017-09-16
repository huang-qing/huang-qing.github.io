---
layout:     post
title:      WebSocket
subtitle:   Web API - WebSocket
date:       2017-08-10
author:     huangqing
header-img: img/post-bg-html.jpg
catalog: true
categories: [HTML]
tags:
    - WebSocket
    - HTML
---

# WebSocket

[websocket](http://www.websocket.org/ )  \  [webplatform](http://webplatform.org )  \ 《The Definitive Guide to HTML5 WebSocket》

WebSocket 发起单个请求，服务端不需要等待客服端，客户端在任何时候也能发消息到服务端，减少了轮询时候的延迟.经历一次连接后，服务器能给客户端发多次。下图是轮询与WebSocket的区别。

![websocket](/images/html5/417688-20160405225318406-2127282161.jpg)

基于http的实时消息是相当的复杂，在无状态的请求中维持回话的状态增加了复杂度，跨域也很麻烦，使用ajax处理请求有序请求需要考虑更多。通过ajax进行交流也不简单。每一个延伸http功能的目的不是增加他的复杂度。websocket 可以大大简化实时通信应用中的链接。
 
## Websocket标准

Websocket是一种底层网络协议，可以让你在这个基础上建立别的标准协议。比如在WebSocket的客户端的基础上使用XMPP登录不同的聊天服务器，因为所有的XMPP服务理解相同的标准协议。WebSocket是web应用的一种创新。
 
为了与其他平台竞争，WebSocket是H5应用提供的一部分先进功能。每个操作系统都需要网络功能，能够让应用使用Sockets与别的主机进行通信，是每个大平台的核心功能。在很多方面，让Web应用表现的像操作系统平台是html5的趋势。像socket这样底层的网络协议APIs不会符合原始的安全模型，也不会有web api那样的设计风格。WebSocket给H5应用提供TCP的方式不会消弱网络安全且有现代的Api。    
 
WebSocket是Html5平台的一个重要组件也是开发者强有力的工具。简单的说，你需要WebSocket创建世界级的web应用。它弥补了http不适合实时通信的重大缺陷。异步、双向通信模式，通过传输层协议使WebSocket具有普遍灵活性。想象一下你能用WebSocket创建正真实实时应用的所有方式。比如聊天、协作文档编辑、大规模多人在线游戏（MMO）,股票交易应用等等。
 
WebSocket是一个协议，但也有一个WebSocket API，这让你的应用去控制WebSocket的协议去响应被服务端触发的事件。API是W3C开发，协议是IETE制定。现代浏览器支持WebSocket API，这包括使用全双工和双向链接的方法和特性。让你执行像打开关闭链接、发送接收消息、监听服务端事件等必要操作。
 
WebSocket协议能够让客户端和远程服务端通过web建立全双工通信。支持传输二进制字符串和文本字符串，协议包含打开握手之后的基本消息框架。
 
大量的可选的WebSocket服务器、开发者社区以及无数正在使用WebSocket的应用体现了WebSocket的火热，已有大量已经实现的WebSocket服务器，例如Apache mod_pywebsocket, Jetty, Socket.IO 和 Kaazing’s WebSocket Gateway. 这本书的灵感来自于分享我们多年在Kaazing项目中处理WebSocket及相关技术的知识和经验。Kaazing被作为企业级WebSocket网关服务器和它的客户端库已经超过5年。

## WebSocket API

[websocket api cnblogs](http://www.cnblogs.com/stoneniqiu/p/5373993.html)