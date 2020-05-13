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
    var range;
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