---
layout:     post
title:      Git Flow
subtitle:   别再推荐Git Flow了
date:       2020-10-05
author:     huangqing
header-img: img/post-bg-git.jpg
catalog: true
categories: [Git]
tags:
    - Git
---


![](/images/git/git-flow.jpg)

功能分支(feature branches)、发布分支(release branches)、主干(master)、开发分支(develop)、紧急修复分支(hotfixes)和标签(tag)。

Git Flow 太复杂

Git Flow 违背了分支的“短命”原则:在使用 Git 时，在同一个分支上开发代码的人越多，出现合并冲突的几率就越高。在使用 Git Flow 后，冲突几率会变得更高，因为还有三个其他的分支（具有不同的生命周期）会合并到开发分支上：功能分支、发布分支和紧急修复分支。如果你所在的企业提交代码的速度比较慢，或许没什么问题，但对于需要快速开发的企业或初创公司来说，情况就不一样了。

Git Flow 抛弃了 rebase:由于 Git Flow 的复杂性，你需要可视化跟踪分支，这意味着如果你想要看到来龙去脉，就不能使用 rebase。

Git Flow 阻碍了持续交付:Git Flow 分支模型是基于可预测的长期发布周期，而不是基于每隔几分钟或几小时就要发布新代码的场景

Git Flow 无法应对多代码库

Git Flow 无法应对大型单代码库

谁应该（或不应该）使用 Git Flow？

如果你所在的公司采用了月度或季度发布周期，并且由一个团队负责并行开发多个项目，那么 Git Flow 可能是一个不错的选择。如果你所在的公司是一个初创公司，或者开发的是一个网站或 Web 应用程序，在一天内可能需要发布多个版本，那么使用 Git Flow 对你来说没有什么好处。如果你的团队规模很小（10 人以下），Git Flow 会给你的带来太多冗繁的工作。