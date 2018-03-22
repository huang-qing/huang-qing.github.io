---
layout:     post
title:      Math.random()
subtitle:   JS如何用Math.random()来生成指定范围内的随机数
date:       2018-03-21
author:     huangqing
header-img: img/post-bg-javascript.jpg
catalog: true
categories: [javascript]
tags:
    - javascript
---

## Math

`Math`：数学对象，提供对数据的数学计算。

1. `Math.random()`; 结果为`0-1间的一个随机数`(包括0,不包括1)

2. `Math.floor(num)`; 参数num为一个数值，函数结果为`num的整数部分`,即返回小于等于n的最大整数。

3. `Math.round(num)`; 参数num为一个数值，函数结果为num`四舍五入`后的整数。

4. `Math.ceil(n)`; 返回大于等于n的最小整数。


用`Math.round(Math.random())`;可均衡获取`0到1`的随机整数。

用`Math.floor(Math.random()*10)`;时，可均衡获取`0到9`的随机整数。

用`Math.ceil(Math.random()*10)`;时，主要获取`1到10`的随机整数，取0的几率极小。

用`Math.round(Math.random()*10)`;时，可基本均衡获取`0到10`的随机整数，其中获取最小值0和最大值10的几率少一半。

## 生成随机数的公式

```javascript
// max - 期望的最大值
// min - 期望的最小值
Math.floor(Math.random()*(max-min+1)+min);
```



 