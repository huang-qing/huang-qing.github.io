---
layout:     post
title:      基于websocket的文件上传控件
subtitle:   ws-file-upload
date:       2020-10-09
author:     huangqing
header-img: img/post-bg-html.jpg
catalog: true
categories: [JavaScript]
tags:
    - javascript
---


## 功能描述

开发一个基于websocket传输的文件上传控件，并实现以下功能：

1. 上传文件选择：支持选择单文件、选择多文件、选择文件夹。控件提供单文件上传按钮、多文件上传按钮。
2. 上传文件拖拽选择，自动检测文件或文件夹，递归遍历获取全部文件。控件提供文件拖拽上传区域
3. 文件过滤：支持选择文件的过滤功能，可配置。如：指定的可选文件类型，可以选择图片类型。
4. 文件上传：基于浏览器websocket API,实现文件的分片上传。支持单文件上传、多文件上传，可配置文件并行上传文件的数量。
5. 文件上传进度展示。支持非模态和模态两种模式，可配置。
6. websocket 适配功能。


## 上传文件选择

使用HTML标准的`<input type="file">`

```html
<input type = "file" id = "file" multiple = "multiple" accept = "image/*" directory webkitdirectory />
```

+ `multiple`  支持多选文件
+ `directory(webkitdirectory)` chrome浏览器支持选择文件夹


## 文件拖拽选择

使用HTML Drag and Drop API 实现文件拖拽选择的功能。

与拖拽文件相关的事件有`dragenter`(文件拖拽进)、`dragover`(文件拖拽在悬浮)、`dragleave`(文件拖拽离开)、`drop`(文件拖拽放下)。

需要注意的是：使用 `e.preventDefault()` 阻止拖拽文件相关的事件的默认行为。

主要的文件获取逻辑在`drop`事件中完成。

```js
var dropZone = document.querySelector('#dropZone');

dropZone.addEventListener("dragenter", function (e) {
  e.preventDefault();
  e.stopPropagation();
}, false);
 
dropZone.addEventListener("dragover", function (e) {
  e.preventDefault();
  e.stopPropagation();
}, false);
 
dropZone.addEventListener("dragleave", function (e) {
  e.preventDefault();
  e.stopPropagation();
}, false);
 
dropZone.addEventListener("drop", function (e) {
  e.preventDefault();
  e.stopPropagation();
  // 处理拖拽文件的逻辑
}
```

## 获取文件内容

由于提供了点击file选择文件、点击file选择文件夹、拖拽文件、文件夹三种不同的获取文件的方式，需要整合三种获取文件方式的内容格式。

获取文件的触发时机：

1. 在 `input` 对象在触发`onchange`事件时。在`file`类型上会出现事件只触发一次问题,每次上传文件的时候，都会将当前的文件路径保存至`$event.target.value`中，当第二次选择文件时，由于两次`$event.target.value`相同，所以不会触发change事件。解决的方案是：每点击浏览激活`onchange`事件一次，就`重新初始化一下这个控件`。

2. 在拖拽文件的`drop`事件触发时。

获取到 `File` 对象：

| 途径      | 得到 File 对象的过程 |
| ----     | -------------------- |
|拖拽上传   |	在 `drop` 事件中获取 `e.dataTransfer.items` ，是一个 `DataTransferItemList` 对象，遍历得到 `DataTransferItem` 对象，用 `webkitGetAsEntry` 方法得到 `FileSystemEntry` 对象；</br>根据 `isFile` 属性判断 `entry` 是文件还是文件夹。</br>是文件的话，用 `file` 方法获取 `File` 对象；</br>是文件夹的话，递归地用 `reader` 读取包含的文件|
|上传文件   |	在 `change` 事件中获取 `input.files` ，是一个 `FileList` 对象，遍历得到 `File` 对象|
|上传文件夹 |	同上|

统一格式为 *文件夹1/文件夹2/xxx.txt*:

|途径	|相对路径	说明|
| ----   | --------- | ----|
|拖拽上传	|`entry.fullPath.substring(1)`|	`file.webkitRelativePath` 都是空。</br>所以从 `entry` 中取，注意要把 `entry.fullPath` 最前面的斜杠去掉|
|上传文件	|`file.name`	|`file.webkitRelativePath` 都是空。直接用 `file.name`|
|上传文件夹	|`file.webkitRelativePath`|	`file.webkitRelativePath` 就是想要的格式，直接用|


## 文件过滤

1. 通过`input`中的`accept`属性配置，在用户选择文件时进行初步的过滤。

2. 在获取文件时进行第二次的验证（浏览器不支持的过滤文件类型和通过拖拽选择的文件），使用代码判定文件后缀类型进行验证过滤，不进行上传，不在上传列表中显示。

`accept`属性支持的文件过滤类型：

+ `*.txt`    text/plain 
+ `*.css `   text/css
+ `*.csv`    text/csv
+ `*.htm`    text/html    
+ `*.html`   text/html   
+ `*.js`     text/javascript, application/javascript
+ `*.xml`    text/xml, application/xml  

+ `*.dwg`    image/vnd.dwg    
+ `*.dxf`    image/vnd.dxf
+ `*.gif`    image/gif
+ `*.png`    image/png        
+ `*.jp2`    image/jp2    
+ `*.jpe`    image/jpeg
+ `*.jpeg`   image/jpeg
+ `*.jpg`    image/jpeg  
+ `*.svf`    image/vnd.svf    
+ `*.tif`    image/tiff    
+ `*.tiff`   image/tiff     

+ `*.json`   application/json   
+ `*.pdf`    application/pdf
+ `*.rtf`    application/rtf, text/rtf           
+ `*.dtd`    application/xml-dtd  
+ `*.xhtml`  application/xhtml+xml  

+ `*.doc`    application/msword    
+ `*.dot`    application/msword 
+ `*.asf`    application/vnd.ms-asf
+ `*.mpp`    application/vnd.ms-project   
+ `*.pot`    application/vnd.ms-powerpoint
+ `*.pps`    application/vnd.ms-powerpoint
+ `*.ppt`    application/vnd.ms-powerpoint
+ `*.wdb`    application/vnd.ms-works
+ `*.wps`    application/vnd.ms-works
+ `*.xlc`    application/vnd.ms-excel
+ `*.xlm`    application/vnd.ms-excel
+ `*.xls`    application/vnd.ms-excel
+ `*.xlsx`   application/vnd.openxmlformats-officedocument.spreadsheetml.sheet    
+ `*.xlt`    application/vnd.ms-excel    
+ `*.xlw`    application/vnd.ms-excel  
+ `*.ogg`    application/ogg, audio/ogg    
  
+ `*.zip`    aplication/zip    application/x-zip-compressed

+ `*.mpeg`   video/mpeg    
+ `*.mpg`    video/mpeg

+ `*.3gpp`   audio/3gpp, video/3gpp
+ `*.ac3`    audio/ac3
+ `*.au`     audio/basic
+ `*.mp2`    audio/mpeg, video/mpeg    
+ `*.mp3`    audio/mpeg    
+ `*.mp4`    audio/mp4, video/mp4

## 文件上传

主要原理是基于浏览器 Websocket API，对每一个文件创建一个websocket连接后，使用`Blob.prototype.slice` 方法对文件进行分片，逐次发送分片的文件内容，直到全部发送完成，关闭连接。

支持同时并发上传多个文件，默认3个文件同时上传。即在全局范围内，最多同时开启3个websocket实例进行内容的上传，其他的文件排队等待。

对普通文件和超大文件的处理方式相同，每个文件都是使用单个websocket实例，对文件使用串行的方式进行上传。

对于每个文件上传的流程如下：

1. 创建 websocket 连接，等待连接成功执行下一步操作；否则异常处理，关闭连接。
2. 发送 上传文件通知（字符串类型），附加必要的文件信息，等待服务端响应确认信息。
3. 接收 服务端的确认信息，执行下一步操作；否则异常处理，关闭连接。
4. 开始发送 首个分片文件（二进制），等待服务端响应确认信息。
5. 接收确认信息 计算并发送下一个分片文件；
6. 重复 5-6 的过程，直到分片文件全部正常发送完成；否则异常处理，关闭连接。
7. 发送完成 发送完成通知（字符串类型），等待服务端响应确认信息。
8. 接收完成 确认信息，本次文件上传完成，关闭连接；否则异常处理，关闭连接。

**注意**：分片内容大小不能超过64000Byte,超过此大小浏览器在传输时会自动进行分片传输并附加信息造成最终合并的数据不正确。因此，使用60000Byte的分片大小，保证传输内容的正确。

```js
const ws= new WebSocket("ws://localhost:8888");

ws.onopen = function (event) {
    //确认连接成功
}
ws.onmessage = async function (event) {
    //接收确认信息（文本类型）
        ...
    //发送指令（字符串）
    ws.send("...");

    //发送文件（Blob）
    ws.send();
}
ws.onerror = function(err) {
    //异常处理
}
```

## 配置及接口

WS-FileUpload 对象：

|属性名|默认值|描述|
|----|----|----|
|`create`|`options`|创建实例|
|`destroy`|`element`|上传实例对应的DOM元素容器|

WS-FileUpload 实例：

配置参数：

|属性名|默认值|描述|
|----|----|----|
|`element`|null|上传实例对应的DOM元素容器|
|`accept`|array|允许上传的文件类型|
|`allowDrop`|true|是否允许拖拽上传|
|`allowDirectory`|true|是否允许选择文件夹|
|`allowMultiple`|false|是否允许多文件上传|
|`allowAppend`|true||是否允许追加上传文件|
|`allowRemove`|true|是否允许删除上传文件|
|`maxParallelUploads`|3|最多支持并行上传的文件数量|
|`params`|null|自定义的参数|
|`isMenuDisplay`|true|是否显示功能控件|
|`filesViewType`|'float'|可以设置'float'/'inline'/'hidden' 三个值|
|`autoUpload`|false|是否自动上传|

事件：

|事件名称|方法|描述|
|----|----|----|
|`oninit`|`()`|实例创建完成|
|`onerror`|`(error[,file,status])`|上传异常|
|`oninitfile`|`(file)`|文件创建完成|
|`onaddfilestart`|`(file)`|准备文件开始上传|
|`onaddfilestart`|`(file)`|准备文件开始上传|
|`onprocessfileprogress`|`(file, progress)`|文件上传中|
|`onprocessfile`|`(file)`|上传完成|
|`onprocessfiles`|`()`|全部上传完成|
|`onbeforesentmessage`|`(file)`|准备发送消息(指令)|
|`onbeforereceivemessage`|`(file)`|准备接收消息(指令)|

注：file是一个包含文件相关信息的对象

方法：

|方法名|参数|描述|
|----|----|----|
|`upload`|`()`|上传文件|
|`choose`|`()`|弹出文件选择框文件|
|`destroy`|`()`|销毁实例|


