---
layout:     post
title:      HTML contenteditable 属性
subtitle:   
date:       2020-05-12
author:     huangqing
header-img: img/post-bg-html.jpg
catalog: true
categories: [HTML]
tags:
    - HTML
---

## `contenteditable`

```html
<element contenteditable="true|false">
```

```html
<p contenteditable="true">这是一个可编辑的段落。</p>
```

## `contenteditable` 光标定位到最后

```js
function keepLastIndex(elem){
    var range,
        length;
    
    //IE兼容性处理，光标定位到最后时直接输入文字，会自动换行，且不用div标签包含
    if(elem && elem.children.length>0){
        length=elem.children.length;
        elem = elem.children[length-1];
    }
    if(window.getSelection){// ie9-10 ff chrome safari
        elem.focus();
        range = window.getSelection();
        // 选择elem下的所有子内容
        range.selectAllChildren(elem);
        // 光标移到最后
        range.collapseToEnd();
    }
    else if(document.selection){//ie 10-
        range = document.selection.createRange();
        // range定位到elem
        range.moveToElementText(elem);
        //光标移动到最后
        range.collapse(false);
        range.select();
    }
}
```

## 文本转换

1 text encode/decode

```js
function htmlEncode(str) {
    if (typeof str === 'number') {
        return str;
    }

    var s = str || "";
    s = s + "";

    if (s.length == 0) {
        return "";
    }
    s = s.replace(/&/g, "&amp;");
    s = s.replace(/</g, "&lt;");
    s = s.replace(/>/g, "&gt;");
    s = s.replace(/ /g, "&nbsp;");
    s = s.replace(/\'/g, "&#39;");
    s = s.replace(/\"/g, "&quot;");

    return s;
}

// decode
function htmlDecode(str) {
    var s = str || "";
    s = s + "";
    if (s.length == 0) {
        return "";
    }
    s = str.replace(/&amp;/g, "&");
    s = s.replace(/&lt;/g, "<");
    s = s.replace(/&gt;/g, ">");
    s = s.replace(/&nbsp;/g, " ");
    s = s.replace(/&#39;/g, "\'");
    s = s.replace(/&quot;/g, "\"");

    return s;
}
```

2 html to text

```js
function textTohtml(text){
    var html = '',
        item;
    if(text){
        text = text.split(/\n/g);
        for(var i=0,len=text.length;i<len;i++){
            item = text[i];
            html += "<div>" + (item? htmlEncode(item): "&nbsp;") + "</div>";
        }
    }

    return html;
}
```

3 text to html

```js
function htmlToText(html){
    var patt=/(<p>.*?<\/p>)|(<div>.*?<\/div>)/ig,
        //patt=/(<p>.*?<\/p>)|(<div>.*?<\/div>)/ig, IE不能使用
        text,
        arr;

    text = html || '';
    arr = text.match(patt);

    if(arr){
        text = arr.join('\n');
    }

    if(text){
        text = text.replace(/(<br>)|(<br\/>)/ig,'')
                .replace(/&nbsp;/ig,' ')
                .replace(/(<p>)|(<\/p>)|(<div>)|(<\/div>)/ig,'');
    }

    text = htmlDecode(text);

    return text;
}
```

## `contenteditable`富文本编辑器

1. 基于HTML DOM的`Contenteditable`属性来实现，代表如UEditor、tinyMec、Quill
2. 基于自定义Model的实现，代表如：draft.js、trix

`contenteditable`是浏览器Dom的一个原生属性，值为`true`时表示该元素变为可编辑状态。因此原生就直接支持很多内容编辑操作，包括光标位移、内容选择的行为、键盘事件（如方向键控制光标）等等，甚至是富文本编辑所需要用到的绝大部分实现（`document.execCommand`）.辅以`iframe`技术，可以将编辑器放在一个独立的`docment`对象下，与页面的`document`对象分离.

缺点也非常要命，以why-contenteditable-is-terrible为代表的文章，几乎说明了一切，总结下来无非是：浏览器兼容性差、用户行为难以控制、难以抽象编辑器内的视图逻辑关系并将它们映射到代码模型中


### 技术核心 UEditor

+ **dtd规则**：用来规定编辑器内的dom嵌套规则，和过滤方法搭配使用，避免出现`<span><p>xxx</p></span>`
+ **uNode对象**：根据HTML DOM抽象而成的文档模型对象，抽象了dom的属性和层级关系，保留了一些dom操作的方法（与第二种实现方式的自定义model类似），将编辑器内容的HTML映射过来之后可以很方便的执行规则过滤，如剔除冗余属性和非白名单标签等
+ **Range对象**：光标和选区的信息对象，记录了当前光标（选区）的开始、结束边界的容器节点和偏移量以及当前光标（选区）的闭合状态，还提供了一系列对光标（选区）操作的API
+ **EventBase**：提供注册、销毁和触发自定义事件监听器的方法，用来生成一些钩子
+ **execCommand指令集**：`document.execCommand`增强版，执行指令的通用接口，富文本格式操作的核心，提供了一系列指定命令的执行和状态查询方法（如对选区内容执行字体加粗命令、查询当前选区内容是否处于加粗状态）
+ **undoManager**：撤销重做的堆栈，记录内容变化过程
+ **domUtils**：Dom操作方法集

### 难点

面对固定结构内容：
1. 结构简单但需要进行交互的场景，就像图片注释那样，可以使用前面提到的contenteditable=false+行为劫持+过滤规则的方式实现
2. 结构较为复杂但不需要进行交互，可以使用canvas来实现

光标：
 劫持=>判断=>代理，这也是编辑器对光标进行严格控制的通用解决方案

### 总结 

基于`contenteditable`编辑器稳定可靠的定制开发要注意的几个点:

1. 严格控制内容（格式规则检查、内容输入和输出过滤）
2. 严格控制光标（劫持、检查、代理）
3. 控制撤销重做堆栈
4. 为一些关键操作添加生命周期钩子