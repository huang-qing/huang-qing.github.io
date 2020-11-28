---
layout: post
title: Blob URL
subtitle: 浏览器端创建可下载文件
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
    URL.revokeObjectURL(blob);

}
```

##  预览图片

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


Blob 对象是一个字节序列。拥有 `size` 和 `type` 等属性。初始化`Blob` 接受的内容类型:

+ ArrayBuffer [TypedArrays] elements.
+ ArrayBufferView [TypedArrays] elements.
+ Blob elements.
+ DOMString [WebIDL] elements.


**URL.createObjectURL** & **URL.revokeObjectURL()**


`URL.createObjectURL()` 静态方法会创建一个 `DOMString`，它的 URL 表示参数中的对象。这个 URL 的生命周期和创建它的窗口中的 document 绑定。这个新的URL 对象表示着指定的 File 对象或者 Blob 对象。


在每次调用 `createObjectURL()` 方法时，都会创建一个新的 URL 对象，即使你已经用相同的对象作为参数创建过。当不再需要这些 URL 对象时，每个对象必须通过调用 `URL.revokeObjectURL()` 方法来释放。浏览器会在文档退出的时候自动释放它们，但是为了获得最佳性能和内存使用状况，你应该在安全的时机主动释放掉它们。

##