---
layout:     post
title:      FileReader
subtitle:   
date:       2020-10-04
author:     huangqing
header-img: img/post-bg-html.jpg
catalog: true
categories: [WEB API]
tags:
    - WEB API
---


## 介绍

`FileReader` 对象允许Web应用程序异步读取存储在用户计算机上的文件（或原始数据缓冲区）的内容，使用 `File` 或 `Blob` 对象指定要读取的文件或数据。

其中`File`对象可以是来自用户在一个`<input>`元素上选择文件后返回的`FileList`对象,也可以来自拖放操作生成的 `DataTransfer`对象,还可以是来自在一个`HTMLCanvasElement`上执行`mozGetAsFile()`方法后返回结果。

## 属性

1. `FileReader.error` 只读
2. `FileReader.readyState` 只读
   1. EMPTY: 0 还没有加载任何数据
   2. LOADING:	1	数据正在被加载.
   3. DONE:	2	已完成全部的读取请求.
3. `FileReader.result` 只读 文件的内容。该属性仅在读取操作完成后才有效，数据的格式取决于使用哪个方法来启动读取操作。

## 事件处理

1. `FileReader.onabort` : 处理`abort`事件。该事件在读取操作被中断时触发。
2. `FileReader.onerror` : 处理`error`事件。该事件在读取操作发生错误时触发。
3. `FileReader.onload` : 处理`load`事件。该事件在读取操作完成时触发。
4. `FileReader.onloadstart` : 处理`loadstart`事件。该事件在读取操作开始时触发。
5. `FileReader.onloadend` : 处理`loadend`事件。该事件在读取操作结束时（要么成功，要么失败）触发。
6 `FileReader.onprogress` : 处理`progress`事件。该事件在读取`Blob`时触发。


## 方法

`FileReader.abort()` : 中止读取操作。在返回时，`readyState`属性为`DONE`。
`FileReader.readAsArrayBuffer()` : 开始读取指定的 `Blob`中的内容, 一旦完成, `result` 属性中保存的将是被读取文件的 `ArrayBuffer` 数据对象.
`FileReader.readAsBinaryString()`  : 开始读取指定的`Blob`中的内容。一旦完成，`result`属性中将包含所读取文件的原始二进制数据。
`FileReader.readAsDataURL()` : 开始读取指定的`Blob`中的内容。一旦完成，`result`属性中将包含一个`data: URL`格式的`Base64`字符串以表示所读取文件的内容。
`FileReader.readAsText()`: 开始读取指定的`Blob`中的内容。一旦完成，`result`属性中将包含一个字符串以表示所读取的文件内容。（可以读取txt的文本信息,实际上读的是文件类型为`text/*`的文件的信息）

## 示例

读取txt文件

```js

const input = document.querySelector('input[type=file]');

input.addEventListener('change', ()=>{
  const reader = new FileReader()
  reader.readAsText(input.files[0],'utf8') // input.files[0]为第一个文件
  reader.onload = ()=>{
    document.body.innerHTML += reader.result  // reader.result为获取结果
  }
}, false)
```

读取图片文件

```js
const input = document.querySelector('input[type=file]')

input.addEventListener('change', ()=>{
  console.log( input.files )
  const reader = new FileReader()
  reader.readAsDataURL(input.files[0]) // input.files[0]为第一个文件
  reader.onload = ()=>{
    const img = new Image()
    img.src = reader.result
    document.body.appendChild(img)  // reader.result为获取结果
  }
}, false)
```

## 参考

[MDN FileReader](https://developer.mozilla.org/zh-CN/docs/Web/API/FileReader)