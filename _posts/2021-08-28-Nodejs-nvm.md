---
layout: post
title: nvm
subtitle: Node.js 版本管理工具
date: 2021-08-28
author: huangqing
header-img: img/post-bg-nodejs.png
catalog: true
categories: [Nodejs]
tags:
  - nodejs
---

## nvm 是什么

`nvm`全名 node.js version management，顾名思义是一个 nodejs 的版本管理工具。通过它可以安装和切换不同版本的 nodejs。下面列出下载、安装及使用方法。

## 下载

可在点此在[github](https://github.com/coreybutler/nvm-windows/releases)上下载最新版本,本次下载安装的是 windows 版本。打开网址我们可以看到有两个版本：

- nvm-noinstall.zip：绿色免安装版，但使用时需进行配置。
- nvm-setup.zip：安装版，推荐使用

## 安装

> 首先卸载系统中的 nodejs

window:

1. 双击安装文件 nvm-setup.exe
2. 选择 nvm 安装路径
3. 选择 nodejs 路径
4. 确认安装即可
5. 安装完确认

```shell
nvm

Running version 1.1.8.

Usage:

  nvm arch                     : Show if node is running in 32 or 64 bit mode.
                                 显示node是运行在32位还是64位。

  nvm current                  : Display active version.

  nvm install <version> [arch] : The version can be a specific version, "latest" for the latest current version, or "lts" for the
                                 most recent LTS version. Optionally specify whether to install the 32 or 64 bit version (defaults
                                 to system arch). Set [arch] to "all" to install 32 AND 64 bit versions.
                                 Add --insecure to the end of this command to bypass SSL validation of the remote download server.
                                 安装node， version是特定版本也可以是最新稳定版本latest。可选参数arch指定安装32位还是64位版本，默认是系统位数。可以添加--insecure绕过远程服务器的SSL。

  nvm list [available]         : List the node.js installations. Type "available" at the end to see what can be installed. Aliased as ls.
                                 显示已安装的列表。可选参数available，显示可安装的所有版本。list可简化为ls。

  nvm on                       : Enable node.js version management.
                                 开启node.js版本管理。

  nvm off                      : Disable node.js version management.
                                 关闭node.js版本管理。

  nvm proxy [url]              : Set a proxy to use for downloads. Leave [url] blank to see the current proxy.
                                 Set [url] to "none" to remove the proxy.
                                 设置下载代理。不加可选参数url，显示当前代理。将url设置为none则移除代理。

  nvm node_mirror [url]        : Set the node mirror. Defaults to https://nodejs.org/dist/. Leave [url] blank to use default url.
                                 设置node镜像。默认是https://nodejs.org/dist/。如果不写url，则使用默认url。设置后可至安装目录settings.txt文件查看，也可直接在该文件操作。

  nvm npm_mirror [url]         : Set the npm mirror. Defaults to https://github.com/npm/cli/archive/. Leave [url] blank to default url.
                                 设置npm镜像。https://github.com/npm/cli/archive/。如果不写url，则使用默认url。设置后可至安装目录settings.txt文件查看，也可直接在该文件操作。

  nvm uninstall <version>      : The version must be a specific version.
                                 卸载指定版本node。

  nvm use [version] [arch]     : Switch to use the specified version. Optionally use "latest", "lts", or "newest".
                                 "newest" is the latest installed version. Optionally specify 32/64bit architecture.
                                 nvm use <arch> will continue using the selected version, but switch to 32/64 bit mode.
                                 使用指定版本node。可指定32/64位。

  nvm root [path]              : Set the directory where nvm should store different versions of node.js.
                                 If <path> is not set, the current root will be displayed.
                                 设置存储不同版本node的目录。如果未设置，默认使用当前目录。

  nvm version                  : Displays the current running version of nvm for Windows. Aliased as v.
                                 显示nvm版本。version可简化为v。
```

## 安装/管理 nodejs

查看本地安装的所有版本；有可选参数 `available`，显示所有可下载的版本:

```shell
nvm list [available]
```

```shell
nvm list

  * 16.13.1 (Currently using 64-bit executable)
    12.20.2
```

```shell
nvm list available

|   CURRENT    |     LTS      |  OLD STABLE  | OLD UNSTABLE |
|--------------|--------------|--------------|--------------|
|    17.3.0    |   16.13.1    |   0.12.18    |   0.11.16    |
|    17.2.0    |   16.13.0    |   0.12.17    |   0.11.15    |
|    17.1.0    |   14.18.2    |   0.12.16    |   0.11.14    |
|    17.0.1    |   14.18.1    |   0.12.15    |   0.11.13    |
|    17.0.0    |   14.18.0    |   0.12.14    |   0.11.12    |
|   16.12.0    |   14.17.6    |   0.12.13    |   0.11.11    |
|   16.11.1    |   14.17.5    |   0.12.12    |   0.11.10    |
|   16.11.0    |   14.17.4    |   0.12.11    |    0.11.9    |
|   16.10.0    |   14.17.3    |   0.12.10    |    0.11.8    |
|    16.9.1    |   14.17.2    |    0.12.9    |    0.11.7    |
|    16.9.0    |   14.17.1    |    0.12.8    |    0.11.6    |
|    16.8.0    |   14.17.0    |    0.12.7    |    0.11.5    |
|    16.7.0    |   14.16.1    |    0.12.6    |    0.11.4    |
|    16.6.2    |   14.16.0    |    0.12.5    |    0.11.3    |
|    16.6.1    |   14.15.5    |    0.12.4    |    0.11.2    |
|    16.6.0    |   14.15.4    |    0.12.3    |    0.11.1    |
|    16.5.0    |   14.15.3    |    0.12.2    |    0.11.0    |
|    16.4.2    |   14.15.2    |    0.12.1    |    0.9.12    |
|    16.4.1    |   14.15.1    |    0.12.0    |    0.9.11    |
|    16.4.0    |   14.15.0    |   0.10.48    |    0.9.10    |
```

安装，命令中的版本号可自定义:

```shell
nvm install 16.13.1
```

使用特定版本

```shell
nvm use 16.13.1
```

卸载

```shell
nvm uninstall 16.13.1
```
