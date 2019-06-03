---
layout:     post
title:      前端目录结构说明
subtitle:   
date:       2017-04-14
author:     huangqing
header-img: img/post-bg-technology-stack.jpg
catalog: true
categories: [TechnologyStack]
tags:
    - 部署
    - 前端
---

# 前端目录结构说明

## assets/：

一般存放开发过程中自己写的静态资源（image, css, js等，如：shop.css, car.png, roomListUtil.js）

## static/：

存放第三方静态资源（jquery.js, bootstrap.css等），这里的资源一般是直接引用。

当打包编译后assets中的静态资源也会编译到static目录下，这样原来引用static资源的地址也不用改变，最后编译发布的时候会将所有的静态资源整合到 /dist/static/ 目录下。

## build/:

项目编译目录，各种编译的临时文件和最终的目标文件皆存于此，分为debug/和release/子目录

## dist/:

分发目录，最终发布的可执行程序和各种运行支持文件存放在此目录，打包此目录即可完成项目分发

## doc/:

保存项目各种文档

## lib/:

外部依赖库目录

## samples/:

样例程序目录

## tools/:

项目支撑工具目录

## README:

项目的总体说明文件