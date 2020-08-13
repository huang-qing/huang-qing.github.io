---
layout: post
title:  JavaScript 调试技巧
subtitle: 
date: 2020-07-20
author: huangqing
header-img: img/post-bg-javascript.gif
catalog: true
categories: [JavaScript]
tags:
  - Debugger
---

1. `debugger;` ：只要把它写到代码里，Chrome 运行的时候就会自动自动停在那。
   
2. 把 objects 输出成表格: `console.table`

3. 试遍所有的尺寸:  Chrome 浏览器提， 进入检查面板点击 ‘切换设备模式’ 按钮，这样你就可以调整视窗的大小了

4. 快速定位 DOM 元素 : 在元素面板上标记一个 DOM 元素并在 console 中使用它。Chrome Inspector 的历史记录保存最近的五个元素，最后被标记的元素记为 `$0`，倒数第二个被标记的记为 `$1`，以此类推。

5. 用 `console.time()` 和 `console.timeEnd()` 测试循环耗时

6. 获取函数的堆栈轨迹信息: `console.trace()`

7. 快速找到调试函数: `debug(funcName)`

8. 美化调试信息:
   ```js
   console.important = function(msg) {
     console.log(‘ % c % s % s % s’, ‘color: brown; font - weight: bold; text - decoration: underline;’, ‘–‘, msg, ‘–‘);
    }
   ```

9. 查看具体的函数调用和它的参数: `monitor(fn)`

10. 节点变化时中断: Chrome 可以在 DOM 元素发生变化的时候暂停处理。在 Chrome 探查器上，右键点击某个元素，并选择中断（Break on）选项来使用
11. `console.memory` 检查堆大小状态
12. c`onsole.profile(‘profileName’)` & `console.profileEnd(‘profileName’)` 从代码中启动和结束浏览器性能工具 - “performance profile”。