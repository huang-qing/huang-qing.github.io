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

# **工具**

+ Git Bash
+ [TortoiseGit](https://tortoisegit.org/)
+ [Visual Studio Code](https://code.visualstudio.com/)

# Setup and Config

## git

```Shell
git --version
```

## config

```Shell
git config --global user.email "[email address]"
git config --global user.name "[name]"
git config --global color.ui auto
```

```Shell
git config --global --list
git config --list
```

## help 

```Shell
git help
```

# Getting and Creating Projects

## init

```Shell
git init
```

## clone

```Shell
git clone [url]
```

# Basic Snapshotting

```Shell
git status
```

```Shell
git add [file]
git add .
```

```Shell
git diff
git diff --cache
git diff --staged
```

```Shell
git commit -m"[descriptive message]"
git commit -a -m "[descriptive message]"
```
`-a` 自动执行 `git add .`

```Shell
git reset --hard HEAD^
git reset --hard HEAD~3
```

```
git mv the-file-name the-file-rename
```

```
git rm the-file
```


# Branching and Merging

```
git branch
git branch -v
```

```
git branch [branch-name]
```

```
git branch -d [branch-name]
git branch -D [branch-name]
```

```
git branch [new-branch-name] e064ba4
```

```
git checkout    [branch-name]
git checkout -b [branch-name]
```

```
git checkout [branch-name]
git merge [branch]
```

`.git/config`

```
[branch "master"]
	remote = origin
	merge = refs/heads/master
```

# Sharing and Updating Projects

```
git fetch
```

```
git push
```

```
git pull
```

```
git remote add origin [url]
```

```
git remote origin set-url [url]
```

`.git/config`
```Shell
[remote "origin"]
        url =[url]
        fetch = +refs/heads/*:refs/remotes/origin/*
```

# Inspection and Comparison

```
git log
git log --oneline
git reflog

```
git log --follow [file]
```

```
git diff [first-branch]...[second-branch]
```

```
git show [commit]
```

# .gitignore

一般来说每个Git项目中都需要一个`.gitignore` 文件([A collection of useful .gitignore templates](https://github.com/github/gitignore))，这个文件的作用就是告诉Git哪些文件不需要添加到版本管理中。

>win7:创建一个文件，文件名为：`.gitignore.`，注意前后都有一个点。保存之后系统会自动重命名为“.gitignore”。

#### 常用规则

过滤规则：

+ `/mtk/` 过滤整个文件夹
+ `*.zip` 过滤所有.zip文件
+ `/mtk/do.c` 过滤某个具体文件

添加规则：

+ `!*.zip` 添加所有.zip文件
+ `!/mtk/one.txt` 添加所有某个具体文件

例如：node需要配置忽略node_modules文件夹：

```
node_modules
```