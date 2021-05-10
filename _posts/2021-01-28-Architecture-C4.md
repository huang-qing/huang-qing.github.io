---
layout:     post
title:      如何画出一张合格的技术架构图
subtitle:   
date:       2021-01-28
author:     huangqing
header-img: img/post-bg-technology-stack.jpg
catalog: true
categories: [Architecture]
tags:
    - architecture
    - C4
---


## 1、什么是架构


架构就是对系统中的实体以及实体之间的关系所进行的抽象描述，是一系列的决策。

架构是结构和愿景。

系统架构是概念的体现，是对物/信息的功能与形式元素之间的对应情况所做的分配，是对元素之间的关系以及元素同周边环境之间的关系所做的定义。

有了架构之后，就需要让干系人理解、遵循相关决策。


## 2、什么是架构图

系统架构图是为了抽象地表示软件系统的整体轮廓和各个组件之间的相互关系和约束边界，以及软件系统的物理部署和软件系统的演进方向的整体视图。

## 3、架构图的作用

一图胜千言。要让干系人理解、遵循架构决策，就需要把架构信息传递出去。架构图就是一个很好的载体。那么，画架构图是为了：

+ 解决沟通障碍
+ 达成共识
+ 减少歧义


## 4、架构图分类

有一种比较流行的是4+1视图，分别为场景视图、逻辑视图、物理视图、处理流程视图和开发视图。

**★ 场景视图**

场景视图用于描述系统的参与者与功能用例间的关系，反映系统的最终需求和交互设计，通常由用例图表示。

![场景视图](/images/architecture/use-case-view.jpg)

**★ 逻辑视图**

逻辑视图用于描述系统软件功能拆解后的组件关系，组件约束和边界，反映系统整体组成与系统如何构建的过程，通常由UML的组件图和类图来表示。

![逻辑视图](/images/architecture/logical-view.jpg)

**★ 物理视图**

物理视图用于描述系统软件到物理硬件的映射关系，反映出系统的组件是如何部署到一组可计算机器节点上，用于指导软件系统的部署实施过程。

![物理视图](/images/architecture/physical-view.jpg)

**★ 处理流程视图**

处理流程视图用于描述系统软件组件之间的通信时序，数据的输入输出，反映系统的功能流程与数据流程,通常由时序图和流程图表示

![处理流程视图](/images/architecture/process-view.jpg)


**★ 开发视图**

开发视图用于描述系统的模块划分和组成，以及细化到内部包的组成设计，服务于开发人员，反映系统开发实施过程。

![开发视图](/images/architecture/development-view.jpg)

以上 5 种架构视图从不同角度表示一个软件系统的不同特征，组合到一起作为架构蓝图描述系统架构。


## C4 模型

![C4](/images/architecture/c4.jpg)

C4 模型使用：System Context（上下文）、Container容器（应用程序、数据存储、微服务等）、Component组件和Code代码来描述一个软件系统的静态结构。这几种图比较容易画，也给出了画图要点，但最关键的是，我们认为，它明确指出了每种图可能的受众以及意义

### 1、语境图(System Context Diagram)

![C4](/images/architecture/c4-System-Context-Diagram.jpg)

★用途：

+ 构建的系统是什么
+ 谁会用它
+ 如何融入已有的IT环境

★ 怎么画：

中间是自己的系统，周围是用户和其它与之相互作用的系统。这个图的关键就是梳理清楚待建设系统的用户和高层次的依赖。

### 容器图(Container Diagram)

容器图是把语境图里待建设的系统做了一个展开。

![C4](/images/architecture/c4-Container-Diagram.jpg)

上图中，除了用户和外围系统，要建设的系统包括一个基于`java\spring mvc`的`web`应用提供系统的功能入口，基于`xamarin`架构的手机`app`提供手机端的功能入口，一个基于`java`的`api`应用提供服务，一个mysql数据库用于存储，各个应用之间的交互都在箭头线上写明了。

★ 用途

这个图的受众可以是团队内部或外部的开发人员，也可以是运维人员。用途可以罗列为：

+ 展现了软件系统的整体形态
+ 体现了高层次的技术决策
+ 系统中的职责是如何分布的，容器间的是如何交互的
+ 告诉开发者在哪里写代码

★ 怎么画

用一个框图来表示，内部可能包括名称、技术选择、职责，以及这些框图之间的交互，如果涉及外部系统，最好明确边界。

### 3、组件图(Component Diagram)

![C4](/images/architecture/c4-Component-Diagram.jpg)

组件图是把某个容器进行展开，描述其内部的模块。

★ 用途

这个图主要是给内部开发人员看的，怎么去做代码的组织和构建。其用途有：

+ 描述了系统由哪些组件/服务组成
+ 厘清了组件之间的关系和依赖
+ 为软件开发如何分解交付提供了框架

### 4、类图(Code/Class Diagram)

![C4](/images/architecture/c4-Class-Diagram.jpg)

### 符号

C4 模型没有预定义任何特定的符号，你在这些示例图中看到的是一个个简单的符号，适用于白板、纸张、便签、索引卡片和各种图表工具。你也可以使用 UML 作为符号，并适当使用包、组件和原型。无论你使用哪种符号，我都会建议让每个元素都包含名称、元素类型（即“人”、“软件系统”，“容器”或“组件”）、技术选型（如果有的话），以及一些描述性文字。在图表中包含如此多的文本可能看起来很不寻常，但这些附加文本有助于消除软件架构图中通常会出现的不明确的表示。

即使符号对你来说是显而易见的，仍然要确保为这些符号提供图例。图例中应该包括颜色、形状、首字母缩略词、线条样式、边框、尺寸等。理想情况下，符号应该在每个细节层次上保持一致。下面是前面显示的容器图的图例。

![C4](/images/architecture/c4-Class-Diagram.jpg)

## 工具

+ [C4](https://c4model.com/)
+ [C4中文](https://www.infoq.cn/article/C4-architecture-model/)
+ [structurizr](https://structurizr.com/)
+ [plantuml](https://plantuml.com/zh/)
+ [vscode-drawio](https://marketplace.visualstudio.com/items?itemName=hediet.vscode-drawio)

![c4-Symbol](/images/architecture/c4-Symbol.png)

## 原文

[如何画出一张合格的技术架构图？](https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247490075&idx=1&sn=18b8f093352c34e1b7239eee3bfeb93c&chksm=e9292714de5eae02cd70e1ac03217fdf1e3ccfc3d0adce7f15b5366f6c498b2fdc21c1a0b2c8&mpshare=1&scene=24&srcid=#rd)