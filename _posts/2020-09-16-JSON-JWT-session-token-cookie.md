---
layout:     post
title:      Cookie、Session、Token、JWT
subtitle:   
date:       2020-09-16
author:     huangqing
header-img: img/post-bg-javascript.jpg
catalog: true
categories: [JSON]
tags:
    - json 
---

## 概念

### 什么是认证（Authentication）

通俗地讲就是**验证当前用户的身份**，证明“你是你自己”

互联网中的认证：

+ 用户名密码登录
+ 邮箱发送登录链接
+ 手机号接收验证码

只要你能收到邮箱/验证码，就默认你是账号的主人

### 什么是授权（Authorization）

用户授予第三方应用访问该用户某些资源的权限

你在安装手机应用的时候，APP 会询问是否允许授予权限（访问相册、地理位置等权限）

实现授权的方式有：`cookie`、`session`、`token`、`OAuth`

### 什么是凭证（Credentials）

实现认证和授权的前提是需要一种媒介（证书） 来标记访问者的身份

### 什么是 Cookie

HTTP 是无状态的协议，对于事务处理没有记忆能力，每次客户端和服务端会话完成时，服务端不会保存任何会话信息。

服务器与浏览器为了进行会话跟踪，就必须主动的去维护一个状态，这个状态用于告知服务端前后两个请求是否来自同一浏览器。而这个状态需要通过 `cookie` 或者 `session` 去实现。

`cookie` 存储在客户端： `cookie` 是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上。

`cookie` 是不可跨域的： 每个 `cookie` 都会绑定单一的域名，无法在别的域名下获取使用，一级域名和二级域名之间是允许共享使用的（`domain`）

### 什么是 Session

session 是另一种记录服务器和客户端会话状态的机制

session 是基于 cookie 实现的，session 存储在服务器端，sessionId 会被存储到客户端的cookie 中

![](/images/web/session.jpg)


session 认证流程：

1. 用户第一次请求服务器的时候，服务器根据用户提交的相关信息，创建对应的 Session
2. 请求返回时将此 Session 的唯一标识信息 SessionID 返回给浏览器
3. 浏览器接收到服务器返回的 SessionID 信息后，会将此信息存入到 Cookie 中，同时 Cookie 记录此 SessionID 属于哪个域名
4. 当用户第二次访问服务器的时候，请求会自动判断此域名下是否存在 Cookie 信息，如果存在自动将 Cookie 信息也发送给服务端，服务端会从 Cookie 中获取 SessionID，再根据 SessionID 查找对应的 Session 信息，如果没有找到说明用户没有登录或者登录失效，如果找到 Session 证明用户已经登录可执行后面操作。

`SessionID` 是连接 `Cookie` 和 `Session` 的一道桥梁，大部分系统也是根据此原理来验证用户登录状态。

### 什么是 Token（令牌）

访问资源接口（API）时所需要的资源凭证

简单 token 的组成： uid(用户唯一的身份标识)、time(当前时间的时间戳)、sign（签名，token 的前几位以哈希算法压缩成的一定长度的十六进制字符串）

特点：

+ 服务端无状态化、可扩展性好
+ 支持移动端设备
+ 安全
+ 支持跨程序调用

![](/images/web/token.jpg)

1. 客户端使用用户名跟密码请求登录
2. 服务端收到请求，去验证用户名与密码
3. 验证成功后，服务端会签发一个 token 并把这个 token 发送给客户端
4. 客户端收到 token 以后，会把它存储起来，比如放在 cookie 里或者 localStorage 里
5. 客户端每次向服务端请求资源的时候需要带着服务端签发的 token
6. 服务端收到请求，然后去验证客户端请求里面带着的 token ，如果验证成功，就向客户端返回请求的数据
7. 每一次请求都需要携带 token，需要把 token 放到 HTTP 的 Header 里
8. 基于 token 的用户认证是一种服务端无状态的认证方式，服务端不用存放 token 数据。用解析 token 的计算时间换取 session 的存储空间，从而减轻服务器的压力，减少频繁的查询数据库
9. token 完全由应用管理，所以它可以避开同源策略

**Refresh Token**

refresh token 是专用于刷新 access token 的 token。如果没有 refresh token，也可以刷新 access token，但每次刷新都要用户输入登录用户名与密码，会很麻烦。有了 refresh token，可以减少这个麻烦，客户端直接用 refresh token 去更新 access token，无需用户进行额外的操作。

![](/images/web/refresh-token.jpg)

### 什么是 JWT

JSON Web Token（简称 JWT）是目前最流行的跨域认证解决方案。是一种认证授权机制。

## 区别

### Cookie 和 Session 的区别

+ 安全性： Session 比 Cookie 安全，Session 是存储在服务器端的，Cookie 是存储在客户端的。
+ 存取值的类型不同：Cookie 只支持存字符串数据，想要设置其他类型的数据，需要将其转换成字符串，Session 可以存任意数据类型。
+ 有效期不同： Cookie 可设置为长时间保持，比如我们经常使用的默认登录功能，Session 一般失效时间较短，客户端关闭（默认情况下）或者 Session 超时都会失效。
+ 存储大小不同： 单个 Cookie 保存的数据不能超过 4K，Session 可存储数据远高于 Cookie，但是当访问量过多，会占用过多的服务器资源。

### Token 和 Session 的区别

+ Session 是一种记录服务器和客户端会话状态的机制，使服务端有状态化，可以记录会话信息。而 Token 是令牌，访问资源接口（API）时所需要的资源凭证。Token 使服务端无状态化，不会存储会话信息。
+ 如果你的用户数据可能需要和第三方共享，或者允许第三方调用 API 接口，用 Token 

### Token 和 JWT 的区别

+ Token：服务端验证客户端发送过来的 Token 时，还需要查询数据库获取用户信息，然后验证 Token 是否有效。
+ JWT：将 Token 和 Payload 加密后存储于客户端，服务端只需要使用密钥解密进行校验（校验也是 JWT 自己实现的）即可，不需要查询或者减少查询数据库，因为 JWT 自包含了用户信息和加密的数据。


## 常见的前后端鉴权方式

+ Session-Cookie
+ Token 验证（包括 JWT，SSO）
+ OAuth2.0（开放授权）

## 分布式架构下 session 共享方案

1. session 复制

任何一个服务器上的 session 发生改变（增删改），该节点会把这个 session 的所有内容序列化，然后广播给所有其它节点，不管其他服务器需不需要 session ，以此来保证 session 同步

2. 粘性 session /IP 绑定策略

采用 Ngnix 中的 ip_hash 机制，将某个 ip的所有请求都定向到同一台服务器上，即将用户与服务器绑定。

3. session 共享（常用）

+ 使用分布式缓存方案比如 Memcached 、Redis 来缓存 session，但是要求 Memcached 或 Redis 必须是集群

+ 把 session 放到 Redis 中存储，虽然架构上变得复杂，并且需要多访问一次 Redis ，但是这种方案带来的好处也是很大的：

+ 实现了 session 共享；

+ 可以水平扩展（增加 Redis 服务器）；

+ 服务器重启 session 不丢失（不过也要注意 session 在 Redis 中的刷新/失效机制）；

+ 不仅可以跨服务器 session 共享，甚至可以跨平台（例如网页端和 APP 端）

![](/images/web/session-share.jpg)

4. session 持久化

将 session 存储到数据库中，保证 session 的持久化

## 参考

[还分不清 Cookie、Session、Token、JWT](https://juejin.im/post/5e055d9ef265da33997a42cc)