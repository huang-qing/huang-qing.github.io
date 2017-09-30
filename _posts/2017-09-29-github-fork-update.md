---
layout:     post
title:      更新fork的repo
subtitle:   如何直接在github网站上更新你fork的repo
date:       2017-09-29
author:     huangqing
header-img: img/post-bg-github-cup.jpg
catalog: true
categories: [GitHub]
tags:
    - GitHub
---

## fork

玩过github的人一定会在你自己的账号上`fork`了一些`github`开源项目。这些开源项目往往更新比较活跃，你今天`fork`用到你自己的项目中去了，过几个星期这个`fork`的`origin`可能有一些`bugfix`了，你怎么办呢？

当然直接到`Origin repo`中去`clone`是一个方法，但是`github`的`public repo`有可能过一段时间就被作者删除了，你是否希望在`origin`即使已经被删除的情况下，你的账号下依然有你钟情的`repo`？

解决上面的问题，最好的方法就是不定时地将`origin`的`commit sync`到你自己的`fork repo`中，一方面能够保持鲜活，另一方面有备无患。

那么如何`sync`呢？有以下几种方案:

## clone

一种是你直接在本地`clone`的repo中，`pull upstrame`,做好`merge`，随后`push`到你自己的`fork repo`中。

## github

另外还有一种更加简便聪明的方法：只需在github网站上点几个鼠标，不用本地开发环境轻松搞定：

1. 打开你的`github fork repo`;

2. 点击`Pull request`;

3. 点击`new pull request`.默认情况下，`github`会比较`original/your fork`，这时应该不会有任何输出，因为你并没有做过任何变更；

4. 点击`switching the base`.这时`github`将反过来比较`yourfork/original`，这时你将看到`original`相对你`fork`时的所有`commit`;

![](/images/github/Comparing changes.png)

5. 点击`create a pull request for this comparison`，这时将会反过来向你的repo提交一个`pull request`;

6. 这时你作为你自己fork的repo的owner，你就可以点击`confirm the merge`，合并同步完成！👻

![](/images/github/confirm-merge.png)
 
## 使用git命令

```
git remote add upstream <pathtooriginalrepo>

git fetch upstream

git merge upstream/master master

git push origin master
```