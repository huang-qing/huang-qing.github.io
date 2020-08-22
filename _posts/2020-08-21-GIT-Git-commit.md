---
layout:     post
title:      Git
subtitle:   规范 Git commit
date:       2020-08-21
author:     huangqing
header-img: img/post-bg-git.jpg
catalog: true
categories: [Git]
tags:
    - Git
---

## 规范建设

commit message格式

```
<type>(<scope>): <subject>
```

### type(必须)

用于说明git commit的类别，只允许使用下面的标识。

+ `feat`：新功能（feature）。

+ `fix/to`：修复bug，可以是QA发现的BUG，也可以是研发自己发现的BUG。
  + `fix`：产生diff并自动修复此问题。适合于一次提交直接修复问题
  + `to`：只产生diff不自动修复此问题。适合于多次提交。最终修复问题提交时使用fix

+ docs：文档（documentation）。

+ style：格式（不影响代码运行的变动）。

+ refactor：重构（即不是新增功能，也不是修改bug的代码变动）。

+ perf：优化相关，比如提升性能、体验。

+ test：增加测试。

+ chore：构建过程或辅助工具的变动。

+ revert：回滚到上一个版本。

+ merge：代码合并。

+ sync：同步主线或分支的Bug。

### scope(可选)

scope用于说明 commit 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同。

例如在Angular，可以是location，browser，compile，compile，rootScope， ngHref，ngClick，ngView等。如果你的修改影响了不止一个scope，你可以使用*代替。

## subject(必须)

subject是commit目的的简短描述，不超过50个字符。

+ 建议使用中文。
+ 结尾不加句号或其他标点符号。


根据以上规范git commit message将是如下的格式：

```
fix(DAO):用户查询缺少username属性
```

```
feat(Controller):用户查询接口开发
```

## 监控服务

git commit规范，考虑从源头进行强制拦截，只要大家提交代码的commit message不符合规范，直接不能提交。但由于代码仓库操作权限的问题，最终选择了使用webhook通过发送警告的形式进行监控，督促大家按照规范执行代码提交。除了监控git commit message的规范外，我们还加入了大代码量提交监控和删除文件监控，减少研发的代码误操作。

![](/images/git/git-comment.jpg)

+ 服务注册：服务注册主要完成代码库相关信息的添加。
+ 重复校验：防止merge request再走一遍验证流程。
+ 消息告警：对不符合规范以及大代码量提交、删除文件等操作发送告警消息。
+ DB：存项目信息和git commit信息便于后续统计commit message规范率。

`webhook`是作用于代码库上的，用户提交git commit，push到仓库的时候就会触发webhook，webhook从用户的commit信息里面获取到commit message，校验其是否满足git commit规范，如果不满足就发送告警消息；如果满足规范，调用gitlab API获取提交的diff信息，验证提交代码量，验证是否有重命名文件和删除文件操作，如果存在以上操作还会发送告警消息，最后把所有记录都入库保存。

## 改进

git hooks分为客户端hook和服务端hook。

客户端hook又分为`pre-commit`、`prepare-commit-msg`、`commit-msg`、`post-commit`等，主要用于控制客户端git的提交工作流。用户可以在项目根目录的.git目录下面配置使用，也可以配置全局git template用于个人pc上的所有git项目使用。

服务端hook又分为`pre-receive`、`post-receive`、`update`，主要在服务端接受提交对象时进行调用。

以上这种采用webhook的形式对git commit进行监控就是一种server端的hook，相当于post-receive。这种方式并不能阻止代码的提交，它只是通过告警的形式来约束用户的行为，但最终不规范的commit message还是被提交到了服务器，不利于后面change log的生成。由于公司代码库权限问题，我们目前只能添加这种post-receive类型的webhook。

如大家有更高的代码库权限，可以采用server端`pre-receive`类型的webhook，直接拒绝不规范的git commit message。只要git commit规范了，我们甚至可以考虑把代码和bug、需求关联等等。

当然这块我们也可以考虑客户端的`pre-commit`，pre-commit在git add提交之后，然后执行git commit时执行，脚本执行没错就继续提交，反之就会驳回。客户端git hooks位于每个git项目下的隐藏文件.git中的hooks文件夹里。我们可以通过修改这块的配置文件添加我们的规则校验，直接阻止不规范message的提交，也可以通过客户端commit-msg类型的hook进行拦截，把不规范扼杀在萌芽之中。修改每个git项目下面.git目录中的hooks文件大家肯定觉得浪费时间，其实这里可以采用配置全局git template来完成。但是这又会涉及到hooks配置文件同步的问题。hooks配置文件在本地，如何让hooks配置文件的修改能同步到所有使用的项目又成为一个问题。所以使用服务端hook还是客户端hook需要根据具体需求做适当的权衡。

git hook不光可以用来做规范限制，它还可以做更多有意义的事情。

一次git commit提交的信息量很大，有作者信息、代码库信息、commit等信息。我们的监控服务就根据作者信息做了git commit的统计，这样不仅可以用来监控commit message的规范性，也可以用来监控大家的工作情况。我们也可以把git commit和相关的bug关联起来，我们查看bug时就可以查看解决这个bug的代码修改，很有利于相关问题的追溯。当然我们用同样的方法也可以把git commit和相关的需求关联起来，比如我们定义一种格式feat *786990（需求的ID），然后在git commit的时候按照这种格式提交，webhook就可以根据这种格式把需求和git commit进行关联，也可以用来追溯某个需求的代码量，当然这个例子不一定合适，但足以证明git hook功能之强大，可以给我们的流程规范带来很大的便利。
