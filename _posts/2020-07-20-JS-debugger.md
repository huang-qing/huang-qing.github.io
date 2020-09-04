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
  - chrome
---

1. `debugger;` ：只要把它写到代码里，Chrome 运行的时候就会自动自动停在那。
   
2. 把 objects 输出成表格: `console.table`

3. 试遍所有的尺寸:  Chrome 浏览器提， 进入检查面板点击 ‘切换设备模式’ 按钮，这样你就可以调整视窗的大小了

4. `$_`是一个特殊变量，它的值始终等于控制台中上一次操作的执行结果。它可以让你更加优雅地调试代码。

5. 快速定位 DOM 元素 : 在元素面板上标记一个 DOM 元素并在 console 中使用它。Chrome Inspector 的历史记录保存最近的五个元素，最后被标记的元素记为 `$0`，倒数第二个被标记的记为 `$1`，以此类推。

6. 用 `console.time()` 和 `console.timeEnd()` 测试循环耗时

7. 获取函数的堆栈轨迹信息: `console.trace()`

8. 快速找到调试函数: `debug(funcName)`

9.  美化调试信息:
   ```js
   console.important = function(msg) {
     console.log(‘ % c % s % s % s’, ‘color: brown; font - weight: bold; text - decoration: underline;’, ‘–‘, msg, ‘–‘);
    }
   ```

11. 查看具体的函数调用和它的参数: `monitor(fn)`

12. 节点变化时中断: Chrome 可以在 DOM 元素发生变化的时候暂停处理。在 Chrome 探查器上，右键点击某个元素，并选择中断（Break on）选项来使用
    
13. `console.memory` 检查堆大小状态
    
14. `console.profile(‘profileName’)` & `console.profileEnd(‘profileName’)` 从代码中启动和结束浏览器性能工具 - “performance profile”。
    
15. `ctrl+shift+p` 调用控制台

16. 重新发送 XHR 请求 ： 打开“网络”面板 - 单击 XHR 按钮 - 选择要重新发送的 XHR 请求 - 重放 XHR 请求

17. `copy`函数不是由 ECMAScript 定义的，而是由 Chrome 浏览器提供的。使用此功能，你可以将 JavaScript 变量的值复制到你的剪贴板中，方便在其他位置使用。

18.  Chrome 浏览器中，将图像转换为 `Data URL` . network - img -选择图片 - preview - Copy image as data URL
    
19.  在“元素”面板对 DOM 元素进行拖放:有时我们想调整页面上某些 DOM 元素的位置以测试 UI。在“元素”面板中，你可以拖放任何 HTML 元素来更改其在页面中的显示位置
    
20.  隐藏元素的快捷方式 : 在调试 CSS 样式时，我们通常需要隐藏一个元素。如果选择元素并按下键盘上的H键，我们就可以快速隐藏该元素。
    
21.  将 DOM 元素存储在全局临时变量中 ： 选择某个元素 - 右键点击鼠标 - 存储为全局变量