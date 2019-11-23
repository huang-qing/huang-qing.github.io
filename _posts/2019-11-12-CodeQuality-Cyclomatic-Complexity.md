---
layout: post
title: 圈复杂度
subtitle: 前端代码质量 ConardLi
date: 2019-11-11
author: huangqing
header-img: img/home-bg-codequality.jpg
catalog: true
categories: [CodeQuality]
tags:
  - javascript
  - CodeQuality
---

## 定义

圈复杂度 (Cyclomatic complexity) 是一种代码复杂度的衡量标准，也称为条件复杂度或循环复杂度，它可以用来衡量一个模块判定结构的复杂程度，数量上表现为独立现行路径条数，也可理解为覆盖所有的可能情况最少使用的测试用例数。简称 CC 。其符号为 VG 或是 M 。

>圈复杂度 在 1976 年由 Thomas J. McCabe, Sr. 提出。

## 衡量标准

>代码复杂度低，代码不一定好，但代码复杂度高，代码一定不好。

|圈复杂度|代码状况|可测性|维护成本|
| ---- | ---- | ---- | ---- |
|1 - 10|清晰、结构化|高|低|
|10 - 20|复杂|中|中|
|20 - 30|非常复杂|低|高|
|>30|不可读	|不可测	|非常高|

## 计算方法

#### 3.1 控制流程图

控制流程图，是一个过程或程序的抽象表现，是用在编译器中的一个抽象数据结构，由编译器在内部维护，**代表了一个程序执行过程中会遍历到的所有路径**。它用图的形式表示一个过程内所有基本块执行的可能流向, 也能反映一个过程的实时执行过程。

![](/images/code-quality/3253245866-5da3b33b5f53f_articlex.png)


##### 3.2 节点判定法

有一个简单的计算方法，**圈复杂度实际上就是等于判定节点的数量再加上1**。向上面提到的：`if else` 、`switch case` 、 `for`循环、三元运算符等等，都属于一个判定节点，例如下面的代码：

```js
function testComplexity(*param*) {
    let result = 1;
    if (param > 0) {
        result--;
    }
    for (let i = 0; i < 10; i++) {
        result += Math.random();
    }
    switch (parseInt(result)) {
        case 1:
            result += 20;
            break;
        case 2:
            result += 30;
            break;
        default:
            result += 10;
            break;
    }
    return result > 20 ? result : result;
}
```

上面的代码中一共有一个`if`语句，一个`for`循环，两个`case`语句，一个三元运算符,所以代码复杂度为 `4+1+1=6`。另外，需要注意的是 `||` 和 `&&` 语句也会被算作一个判定节点，例如下面代码的代码复杂为`3`：
```js
function testComplexity(*param*) {
    let result = 1;
    if (param > 0 && param < 10) {
        result--;
    }
    return result;
}
```

#### 3.3 点边计算法

```
M = E − N + 2P
```

+ E：控制流图中边的数量
+ N：控制流图中的节点数量
+ P：独立组件的数目

前两个，边和节点都是数据结构图中最基本的概念：

![](/images/code-quality/1097369189-5da3b33bc1952_articlex.png)

P代表图中独立组件的数目，独立组件是什么意思呢？来看看下面两个图，左侧为连通图，右侧为非连通图：

+ 连通图：对于图中任意两个顶点都是连通的

![](/images/code-quality/1698582206-5da3b33bcd181_articlex.png)

一个连通图即为图中的一个独立组件，所以左侧图中独立组件的数目为1，右侧则有两个独立组件。

对于我们的代码转化而来的控制流程图，正常情况下所有节点都应该是连通的，除非你在某些节点之前执行了 `return`，显然这样的代码是错误的。所以每个程序流程图的独立组件的数目都为1，所以上面的公式还可以简化为 `M = E − N + 2` 。

## 4. 降低代码的圈复杂度

我们可以通过一些代码重构手段来降低代码的圈复杂度。

>重构需谨慎，示例代码仅仅代表一种思想，实际代码要远远比示例代码复杂的多。

4.1 抽象配置

通过抽象配置将复杂的逻辑判断进行简化。

4.2 单一职责 - 提炼函数

单一职责原则（SRP）：每个类都应该有一个单一的功能，一个类应该只有一个发生变化的原因。

4.3 使用 break 和 return 代替控制标记

我们经常会使用一个控制标记来标示当前程序运行到某一状态，很多场景下，使用 break 和 return 可以代替这些标记并降低代码复杂度。

4.4 用函数取代参数

4.5 简化条件判断 - 逆向条件

某些复杂的条件判断可能逆向思考后会变的更简单

4.6 简化条件判断 -合并条件

将复杂冗余的条件判断进行合并。

4.7 简化条件判断 - 提取条件

将复杂难懂的条件进行语义化提取,提炼成函数。

## 5. 圈复杂度检测方法

#### 5.1 eslint规则

`eslint`提供了检测代码圈复杂度的`rules`:

我们将开启 `rules` 中的 `complexity` 规则，并将圈复杂度大于 `0` 的代码的 `rule severity` 设置为 `warn` 或 `error` 。

```js
    rules: {
        complexity: [
            'warn',
            { max: 0 }
        ]
    }
```

这样 `eslint` 就会自动检测出所有函数的代码复杂度，并输出一个类似下面的 `message`。

```
Method 'testFunc' has a complexity of 12. Maximum allowed is 0
Async function has a complexity of 6. Maximum allowed is 0.
...
```

5.2 CLIEngine

构建及插件设计，查看原文[前端代码质量-圈复杂度原理和实践](https://segmentfault.com/a/1190000020674381#articleHeader20),[插件awesome-cli](https://github.com/ConardLi/awesome-cli)