---
layout:     post
title:      Git
subtitle:   Git 简明教程
date:       2017-04-11
author:     huangqing
header-img: img/post-bg-git.jpg
catalog: true
categories: [GitHub]
tags:
    - Git
    - vscode    
---



# Git 简明使用教程

## 工具

+ [TortoiseGit](https://tortoisegit.org/)
+ [Visual Studio Code](https://code.visualstudio.com/)

## 指定远程仓库：

#### 直接修改.git/config文件

~~~
[remote "origin"]
        url = https://github.com/huang-qing/github-huangqing.github.io.git
        fetch = +refs/heads/*:refs/remotes/origin/*
~~~

#### 修改命令 

~~~
git remte origin set-url https://github.com/huang-qing/github-huangqing.github.io.git
~~~

#### 删除后重新添加

~~~
git remote rm origin 
git remote add origin https://github.com/huang-qing/github-huangqing.github.io.git
~~~

## 指定分支节点：.git/config

~~~
[branch "master"]
	remote = origin
	merge = refs/heads/master
~~~

`fatal`: refusing to merge unrelated histories

~~~
git pull origin master --allow-unrelated-histories
~~~

## .gitignore

一般来说每个Git项目中都需要一个`.gitignore` 文件([A collection of useful .gitignore templates](https://github.com/github/gitignore))，这个文件的作用就是告诉Git哪些文件不需要添加到版本管理中。

>win7:创建一个文件，文件名为：`.gitignore.`，注意前后都有一个点。保存之后系统会自动重命名为“.gitignore”。

## 常用规则

#### 过滤规则：

+ `/mtk/` 过滤整个文件夹
+ `*.zip` 过滤所有.zip文件
+ `/mtk/do.c` 过滤某个具体文件

#### 添加规则：

+ `!*.zip` 添加所有.zip文件
+ `!/mtk/one.txt` 添加所有某个具体文件

例如：node需要配置忽略node_modules文件夹：

~~~
node_modules
~~~