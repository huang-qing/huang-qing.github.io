---
layout:     post
title:      Clipboard API and events
subtitle:   
date:       2020-09-25
author:     huangqing
header-img: img/post-bg-html.jpg
catalog: true
categories: [WEB API]
tags:
    - WEB API
---

## IE

IE浏览器支持直接读写剪切板内容：

```js
window.clipboardData.clearData();  
window.clipboardData.setData('Text', 'abcd');
window.clipboardData.getData('Text');
```
但是这种方式不安全，很容易泄露用户的隐私，所以现在浏览器如chrome都不支持这种方式了。

1.IE下，直接支持一个window.clipboardData对象，通过这个对象可以访问到粘贴板中内容。

2.chrome和firefox，在oncopy/onpaste/oncut事件对象event中有一个clipboardData对象，而在IE的事件中是没有这个对象的

## Chrome

### copy

```js
// Overwrite what is being copied to the clipboard.
document.addEventListener('copy', function(e) {
  // e.clipboardData is initially empty, but we can set it to the
  // data that we want copied onto the clipboard.
  e.clipboardData.setData('text/plain', 'Hello, world!');
  e.clipboardData.setData('text/html', '<b>Hello, world!</b>');

  // This is necessary to prevent the current document selection from
  // being written to the clipboard.
  e.preventDefault();
});
```

### cut

```js
// Overwrite what is copied to the clipboard.
document.addEventListener('cut', function(e) {
  // e.clipboardData is initially empty, but we can set it to the
  // data that we want copied onto the clipboard as part of the cut.
  // Write the data that we want copied onto the clipboard.
  e.clipboardData.setData('text/plain', 'Hello, world!');
  e.clipboardData.setData('text/html', '<b>Hello, world!</b>');

  // Since we will be canceling the cut operation, we need to manually
  // update the document to remove the currently selected text.
  deleteCurrentDocumentSelection();

  // This is necessary to prevent the document selection from being
  // written to the clipboard.
  e.preventDefault();
});
```

### paste

```js
// Overwrite what is being pasted onto the clipboard.
document.addEventListener('paste', function(e) {
  // e.clipboardData contains the data that is about to be pasted.
  if (e.clipboardData.types.indexOf('text/html') > -1) {
    var oldData = e.clipboardData.getData('text/html');
    var newData = '<b>Ha Ha!</b> ' + oldData;

    // Since we are canceling the paste operation, we need to manually
    // paste the data into the document.
    pasteClipboardData(newData);

    // This is necessary to prevent the default paste action.
    e.preventDefault();
  }
});
```

### To trigger 

```js
execCommand('cut' / 'copy' / 'paste')
```

### clipboardData.types

Mandatory data types

+ text/plain
+ text/uri-list
+ text/csv
+ text/css
+ text/html
+ application/xhtml+xml
+ image/png
+ image/jpg, image/jpeg
+ image/gif
+ image/svg+xml
+ application/xml, text/xml
+ application/javascript
+ application/json
+ application/octet-stream


## Asynchronous Clipboard API

`read()`

```js
const items = await navigator.clipboard.read();
const textBlob = await items[0].getType("text/plain");
const text = await (new Response(textBlob)).text();
```

`readText()`

```js
navigator.clipboard.readText().then(function(data) {
  console.log("Your string: ", data);
});
```

`write(data)`

```js
var data = [new ClipboardItem({ "text/plain": new Blob(["Text data"], { type: "text/plain" }) })];
navigator.clipboard.write(data).then(function() {
  console.log("Copied to clipboard successfully!");
}, function() {
  console.error("Unable to write to clipboard. :-(");
});
```

`writeText(data)`

```js
await navigator.clipboard.writeText("Howdy, partner!");
```

## 常见用法

1：禁止复制

```html
<script>
    // 网页禁止复制 body oncopy事件中 设置 return false；
    // oncopy、onpase事件： 复制、粘贴事件，可用于多数控件
    // div 没有oncopy事件，  body 与 文本框有这个事件
    function forbidCopy() {
        window.alert("网页的内容，只能看，不能动！");
        return false;
    }
</script>
<body  oncopy="forbidCopy();" >
    输入密码：
    <input type="text" oncopy="window.alert('禁止复制!');return false;" />
    再输入一边密码：
    <input type="text" onpaste="window.alert('禁止粘贴!');return false;" />
</body>
```

2：点击复制指定标记中的内容

```html
<script type="text/javascript">
    function copyText(obj) { 
        var rng = document.body.createTextRange(); 
        rng.moveToElementText(obj); 
        rng.scrollIntoView(); 
        rng.select(); 
        rng.execCommand("Copy"); 
        rng.collapse(false); 
        alert("复制成功!"); 
    } 
    function oCopy(id){
        var obj=document.getElementById(id);
        obj.select();
        js=obj.createTextRange();
        js.execCommand("Copy");
        alert("代码已经被成功复制！");
    }
</script>


<span id="tbid">http://pmp.www.jb51.net</span>   
<a href="#" onclick="copyText(document.all.tbid)">点击复制</a>
<span id="tbid2">http://www.www.jb51.net/pmp</span>   
<a href="#" onclick="oCopy(tbid2)">点击复制</a>
```

3：复制内容后附加信息

```html
<script>
    function SetCopyContent() {
        window.event.returnValue = false;
        var content = window.clipboardData.getData("Text") + "/r/n";
        content += "本资源来自简书 " + this.location.href;
        window.clipboardData.setData('Text', content);
        alert("复制成功，请粘贴到你的QQ/MSN上推荐给你的好友");
    }
</script>
<body  oncopy="SetCopyContent();" >
</body>
```

## 参考

+ [w3c-Clipboard API and events](https://w3c.github.io/clipboard-apis/#clipboard-event-interfaces)
+ [event.clipboardData.setData in copy event](https://www.e-learn.cn/topic/2550795)
+ [Chrome浏览器读写系统剪切板](https://www.cnblogs.com/fangsmile/p/8028940.html)
+ [js 剪切板的用法(clipboardData.setData)](https://www.jianshu.com/p/bd7159ac6ced)