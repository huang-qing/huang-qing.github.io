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

>Cookie，有时也用其复数形式 Cookies。类型为“小型文本文件”，是某些网站为了辨别用户身份，进行 Session 跟踪而储存在用户本地终端上的数据（通常经过加密），由用户客户端计算机暂时或永久保存的信息 

![](/images/web/cookie-client-server-1.jpeg)

![](/images/web/cookie-client-server-2.jpeg)

### 什么是 Session

session 是另一种记录服务器和客户端会话状态的机制

session 是基于 cookie 实现的，session 存储在服务器端，sessionId 会被存储到客户端的cookie 中

![](/images/web/session.jpg)

![](/images/web/session-cookie.jpeg)

session 认证流程：

1. 用户第一次请求服务器的时候，服务器根据用户提交的相关信息，创建对应的 Session
2. 请求返回时将此 Session 的唯一标识信息 SessionID 返回给浏览器
3. 浏览器接收到服务器返回的 SessionID 信息后，会将此信息存入到 Cookie 中，同时 Cookie 记录此 SessionID 属于哪个域名
4. 当用户第二次访问服务器的时候，请求会自动判断此域名下是否存在 Cookie 信息，如果存在自动将 Cookie 信息也发送给服务端，服务端会从 Cookie 中获取 SessionID，再根据 SessionID 查找对应的 Session 信息，如果没有找到说明用户没有登录或者登录失效，如果找到 Session 证明用户已经登录可执行后面操作。

`SessionID` 是连接 `Cookie` 和 `Session` 的一道桥梁，大部分系统也是根据此原理来验证用户登录状态。

### 什么是 Token（令牌） : no session!

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

你会发现这种方式确实很妙，只要 server 保证密钥不泄露，那么生成的 token 就是安全的，因为如果伪造 token 的话在签名验证环节是无法通过的，就此即可判定 token 非法。

可以看到通过这种方式有效地避免了 token 必须保存在 server 的弊端，实现了分布式存储，不过需要注意的是，token 一旦由 server 生成，它就是有效的，直到过期，无法让 token 失效，除非在 server 为 token 设立一个黑名单，在校验 token 前先过一遍此黑名单，如果在黑名单里则此 token 失效，但一旦这样做的话，那就意味着黑名单就必须保存在 server，这又回到了 session 的模式，那直接用 session 不香吗。所以一般的做法是当客户端登出要让 token 失效时，直接在本地移除 token 即可，下次登录重新生成 token 就好。

另外需要注意的是 Token 一般是放在 header 的 Authorization 自定义头里，不是放在 Cookie 里的，这主要是为了解决跨域不能共享 Cookie 的问题 （下文详述）


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

### Token 与 Cookie

Cookie 的局限性：

1. Cookie 跨站是不能共享的，这样的话如果你要实现多应用（多系统）的单点登录（SSO），使用 Cookie 来做需要的话就很困难了（要用比较复杂的 trick 来实现）

>所谓单点登录，是指在多个应用系统中，用户只需要登录一次就可以访问所有相互信任的应用系统。

但如果用 token 来实现 SSO 会非常简单，如下:

![](/images/web/token-sso.jpeg)

只要在 header 中的 authorize 字段（或其他自定义）加上 token 即可完成所有跨域站点的认证。

2. 在移动端原生请求是没有 cookie 的

token 有哪些缺点：

1. token 太长了：token 是 header, payload 编码后的样式，所以一般要比 sessionId 长很多，很有可能超出 cookie 的大小限制（cookie 一般有大小限制的，如 4kb）
2. 不太安全：网上很多文章说 token 更安全，其实不然，细心的你可能发现了，我们说 token 是存在浏览器的，再细问，存在浏览器的哪里？既然它太长放在 cookie 里可能导致 cookie 超限，那就只好放在 local storage 里，这样会造成严重的安全问题，因为 local storage 这类的本地存储是可以被 JS 直接读取的，另外由上文也提到，token 一旦生成无法让其失效，必须等到其过期才行，这样的话如果服务端检测到了一个安全威胁，也无法使相关的 token 失效。**所以 token 更适合一次性的命令认证，设置一个比较短的有效期**


## 常见的前后端鉴权方式

+ Session-Cookie
+ Token 验证（包括 JWT，SSO）
+ OAuth2.0（开放授权）

## 分布式架构下 session 共享方案

### session 的痛点

看起来通过 cookie + session 的方式是解决了问题， 但是我们忽略了一个问题，上述情况能正常工作是因为我们假设 server 是单机工作的，但实际在生产上，为了保障高可用，一般服务器至少需要两台机器，通过负载均衡的方式来决定到底请求该打到哪台机器上。

假设登录请求打到了 A 机器，A 机器生成了 session 并在 cookie 里添加 sessionId 返回给了浏览器，那么问题来了：如果请求打到了 B 或者 C，由于 session 是在 A 机器生成的，此时的 B,C 是找不到 session 的.

![](/images/web/session-nginx-balance.jpeg)

###  session 复制

任何一个服务器上的 session 发生改变（增删改），该节点会把这个 session 的所有内容序列化，然后广播给所有其它节点，不管其他服务器需不需要 session ，以此来保证 session 同步

![](/images/web/nginx-session-copy.jpeg)

A 生成 session 后复制到 B, C，这样每台机器都有一份 session，无论添加购物车的请求打到哪台机器，由于 session 都能找到，故不会有问题

这种方式虽然可行，但缺点也很明显：

1. 同一样的一份 session 保存了多份，数据冗余
2. 如果节点少还好，但如果节点多的话，特别是像阿里，微信这种由于 DAU 上亿，可能需要部署成千上万台机器，这样节点增多复制造成的性能消耗也会很大。

###  粘性 session /IP 绑定策略

这种方式是让每个客户端请求只打到固定的一台机器上，比如浏览器登录请求打到 A 机器后，后续所有的添加购物车请求也都打到 A 机器上，Nginx 的 sticky 模块可以支持这种方式，支持按 ip 或 cookie 粘连等等，如按 ip 粘连方式如下

```
upstream tomcats {　　ip_hash;　　server 10.1.1.107:88;　　server 10.1.1.132:80;}
```
![](/images/web/nginx-session-sticky.jpeg)

这样的话每个 client 请求到达 Nginx 后，只要它的 ip 不变，根据 ip hash 算出来的值会打到固定的机器上，也就不存在 session 找不到的问题了，当然不难看出这种方式缺点也是很明显，对应的机器挂了怎么办？

###  session 共享（常用）

+ 使用分布式缓存方案比如 Memcached 、Redis 来缓存 session，但是要求 Memcached 或 Redis 必须是集群

+ 把 session 放到 Redis 中存储，虽然架构上变得复杂，并且需要多访问一次 Redis ，但是这种方案带来的好处也是很大的：

+ 实现了 session 共享；

+ 可以水平扩展（增加 Redis 服务器）；

+ 服务器重启 session 不丢失（不过也要注意 session 在 Redis 中的刷新/失效机制）；

+ 不仅可以跨服务器 session 共享，甚至可以跨平台（例如网页端和 APP 端）

![](/images/web/session-share.jpg)

缺点其实也不难发现，就是每个请求都要去 redis 取一下 session，多了一次内部连接，消耗了一点性能，另外为了保证 redis 的高可用，必须做集群，当然了对于大公司来说, redis 集群基本都会部署，所以这方案可以说是大公司的首选了.

### session 持久化

将 session 存储到数据库中，保证 session 的持久化

## 参考

+ [还分不清 Cookie、Session、Token、JWT](https://juejin.im/post/5e055d9ef265da33997a42cc)
+ [你管这破玩意儿叫 token](https://blog.51cto.com/u_15175267/2833212)
+ [Cookie Session跨站无法共享问题(单点登录解决方案)](https://blog.csdn.net/wtopps/article/details/75040224)



