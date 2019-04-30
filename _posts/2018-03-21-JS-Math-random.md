---
layout: post
title: Math.random()
subtitle: 范围内的随机数、指定长度字符串
date: 2018-03-21
author: huangqing
header-img: img/post-bg-javascript.jpg
catalog: true
categories: [javascript]
tags:
  - javascript
---

## Math

`Math`：数学对象，提供对数据的数学计算。

1. `Math.random()`; 结果为`0-1间的一个随机数`(包括 0,不包括 1)

2. `Math.floor(num)`; 参数 num 为一个数值，函数结果为`num的整数部分`,即返回小于等于 n 的最大整数。

3. `Math.round(num)`; 参数 num 为一个数值，函数结果为 num`四舍五入`后的整数。

4. `Math.ceil(n)`; 返回大于等于 n 的最小整数。

## JS 如何用 Math.random()来生成指定范围内的随机数

用`Math.round(Math.random())`;可均衡获取`0到1`的随机整数。

用`Math.floor(Math.random()*10)`;时，可均衡获取`0到9`的随机整数。

用`Math.ceil(Math.random()*10)`;时，主要获取`1到10`的随机整数，取 0 的几率极小。

用`Math.round(Math.random()*10)`;时，可基本均衡获取`0到10`的随机整数，其中获取最小值 0 和最大值 10 的几率少一半。

## 生成随机数的公式

```javascript
// max - 期望的最大值
// min - 期望的最小值
Math.floor(Math.random() * (max - min + 1) + min);
```

## Math.random()生成指定长度随机字符串

`Math.random` -> 随机数字 16 位小数

```javascript
Math.random();
//-> 0.42207325632264925
```

`number.toString(36)` -> 0-9 a-Z 的字符串

```javascript
Math.random().toString(36);
//-> "0.mutxit4db6k"
```

`Math.random().toString(36).substr(2)` -> 随机字符串

```javascript
Math.random()
  .toString(36)
  .substr(2);
//-> "mutxit4db6k"
```

生成指定长度随机字符串:

```javascript
function random(length) {
  var str = Math.random()
    .toString(36)
    .substr(2);
  if (str.length >= length) {
    return str.substr(0, length);
  }
  str += random(length - str.length);
  return str;
}
```
