---
layout: post
title: 前端二进制
subtitle: 文件下载 图片浏览 Blob FileReader
date: 2020-07-20
author: huangqing
header-img: img/post-bg-html.jpg
catalog: true
categories: [WEB API]
tags:
  - WEB API
---



## 实现下载文件的函数

```js
function loadFile(fileName, content){

    var aLink = document.createElement('a');

    // 使用 Blob 构造函数将文件内容编译为指定格式的二进制
    var blob = new Blob([content], {
      type: 'text/plain'
    });
    // 设置 download 属性设置文件名称
    aLink.download = fileName;
    // Blob 对象作为 Url 也赋给 a 标签
    aLink.href = URL.createObjectURL(blob);

    aLink.click();
    // 回收内存
    URL.revokeObjectURL(aLink.href);

}
```

##  预览图片

![](/images/preview-image.jpg)

```html
<input id="inputFile" type="file" accept="image/*">
<img src="" id="previewImage" alt="图片预览">
<script>
    const $ = document.getElementById.bind(document);
    const $inputFile = $('inputFile');
    const $previewImage = $('previewImage');
    $inputFile.addEventListener('change', function() {
        const file = this.files[0];
        // 使用 URL.createObjectURL
        $previewImage.src = file ? URL.createObjectURL(file) : '';
        // 使用 readAsDataURL
        const reader = new FileReader();
        reader.addEventListener('load', function() {
            $previewImage.src = reader.result;
        }, false);
        if (file) {
            reader.readAsDataURL(file);
        }
    }, this);
</script>
```

## 关于Blob

Blob是用来支持文件操作的。简单的说：在JS中，有两个构造函数 File 和 Blob, 而File继承了所有Blob的属性。File对象可以看作一种特殊的Blob对象。

Blob 对象是一个字节序列。拥有 `size` 和 `type` 等属性。初始化`Blob` 接受的内容类型:

+ ArrayBuffer [TypedArrays] elements.
+ ArrayBufferView [TypedArrays] elements.
+ Blob elements.
+ DOMString [WebIDL] elements.


**URL.createObjectURL** & **URL.revokeObjectURL()**


`URL.createObjectURL()` 静态方法会创建一个 `DOMString`，它的 URL 表示参数中的对象。这个 URL 的生命周期和创建它的窗口中的 document 绑定。这个新的URL 对象表示着指定的 File 对象或者 Blob 对象。


在每次调用 `createObjectURL()` 方法时，都会创建一个新的 URL 对象，即使你已经用相同的对象作为参数创建过。当不再需要这些 URL 对象时，每个对象必须通过调用 `URL.revokeObjectURL()` 方法来释放。浏览器会在文档退出的时候自动释放它们，但是为了获得最佳性能和内存使用状况，你应该在安全的时机主动释放掉它们。

### ArrayBuffer

![](/images/webapi/ArrayBuffer.jpg)

>`ArrayBuffer` 对象用来表示通用的、固定长度的原始二进制数据缓冲区.`ArrayBuffer` 不能直接操作,而是要通过类型数组对象或 `DataView` 对象来操作,它们会将缓冲区中的数据表示为特定的格式,并通过这些格式来读写缓冲区的内容.

由于无法对 `Arraybuffer` 直接进行操作,所以我们需要借助其他对象来操作. 所有就有了 `TypedArray`(类型数组对象)和 `DataView`对象。

### DataView 对象

DataView视图是一个可以从二进制ArrayBuffer对象中读写多种数值类型的底层接口。

```js
new DataView(buffer, [, byteOffset [, byteLength]])
```

```js
let buffer = new ArrayBuffer(2);
console.log(buffer.byteLength); // 2
let dataView = new DataView(buffer);
dataView.setInt(0, 1);
dataView.setInt(1, 2);
console.log(dataView.getInt8(0)); // 1
console.log(dataView.getInt8(1)); // 2
console.log(dataView.getInt16(0)); // 258
```

### TypedArray

另一种TypedArray视图，与DataView视图的一个区别是，它不是一个构造函数，而是一组构造函数，代表不同的数据格式。

TypedArray对象描述了一个底层的二进制数据缓存区（binary data buffer）的一个类数组视图（view）。

|类型	|单个元素值的范围	|大小（bytes）|	描述|
|----|----|----|----|
|Int8Array	|-128 to 127|	1	|8 位二进制有符号整数|
|Uint8Array|	0 to 255	|1	|8 位无符号整数|
|Int16Array	|-32768 to 32767	|2	|16 位二进制有符号整数|
|Uint16Array	|0 to 65535	|2	|16 位无符号整数|

```js
const buffer = new ArrayBuffer(8);
console.log(buffer.byteLength); // 8
const int8Array = new Int8Array(buffer);
console.log(int8Array.length); // 8
const int16Array = new Int16Array(buffer);
console.log(int16Array.length); // 4
```

## BASE64

Base64 解码

```js
var decodedData = window.atob(encodedData);
```

Base64 编码
```js
var encodedData = window.btoa(stringToEncode);
```

## Canvas中的ImageData对象

ImageData对象中存储着canvas对象真实的像素数据，它包含以下几个只读属性：

+ width：图片宽度，单位是像素
+ height：图片高度，单位是像素
+ data：Uint8ClampedArray类型的一维数组，包含着RGBA格式的整型数据，范围在 0 至 255 之间（包括 255）。


```js
var canvas = document.getElementById("canvas");
let ctx = canvas.getContext("2d");
//使用createImageData() 方法去创建一个新的，空白的ImageData对象。
var myImageData = ctx.createImageData(width, height);
//为了获得一个包含画布场景像素数据的ImageData对象，你可以用getImageData()方法：
var myImageData = ctx.getImageData(left, top, width, height);
//用putImageData()方法去对场景进行像素数据的写入。
ctx.putImageData(myImageData, dx, dy);
//裁剪图片
drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)
//toDataURL 将canvas转为 data URI格式
var dataURL = canvas.toDataURL();
```
## 参考

[从图片裁剪来聊聊前端二进制](https://mp.weixin.qq.com/s?__biz=MzA5NzkwNDk3MQ==&mid=2650593132&idx=1&sn=8996c3a8ddc7bdd89656683cc071ce59&chksm=8891c748bfe64e5eb5d4e8a1801fc43f3f7ed748bfe5a9a04b640837e6cd7b0effa777d74c57&mpshare=1&scene=24&srcid=0820vUCpUSTAQ6Nr8KHDEt3p&sharer_sharetime=1597936693468&sharer_shareid=3f8e3a43f78ce137b6d0613608887aa1#rd)