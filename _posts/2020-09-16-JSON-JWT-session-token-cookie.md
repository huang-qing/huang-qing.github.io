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


## 什么是认证（Authentication）

通俗地讲就是**验证当前用户的身份**，证明“你是你自己”

互联网中的认证：

+ 用户名密码登录
+ 邮箱发送登录链接
+ 手机号接收验证码

只要你能收到邮箱/验证码，就默认你是账号的主人

## 什么是授权（Authorization）

用户授予第三方应用访问该用户某些资源的权限

你在安装手机应用的时候，APP 会询问是否允许授予权限（访问相册、地理位置等权限）

你在访问微信小程序时，当登录时，小程序会询问是否允许授予权限（获取昵称、头像、地区、性别等个人信息）

实现授权的方式有：`cookie`、`session`、`token`、`OAuth`






## 参考

[还分不清 Cookie、Session、Token、JWT](https://juejin.im/post/5e055d9ef265da33997a42cc)