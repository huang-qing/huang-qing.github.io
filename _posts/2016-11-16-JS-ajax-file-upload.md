---
layout:     post
title:      Ajax 文件上传-异步上传方案
subtitle:   
date:       2016-11-16
author:     huangqing
header-img: img/post-bg-ajax.jpg
catalog: true
categories: [JavaScript]
tags:
    - ajax
---



# Ajax 文件上传-异步上传方案

## 使用第三方控件（Flash，ActiveX, 浏览器插件等）

优点：

+  交互与可控性好（多文件、进度展示、续传、暂停）
+  性能好（可使用底层协议通信）
  
缺点：

+ 需要浏览器安装插件


## 使用隐藏的iframe模拟异步上传

优点：
+  浏览器原生支持，不需要插件
+  广泛的浏览器兼容性

缺点：

+  交互差，体验差，上传过程基本不可控
+  性能差

关键技术点：

1.  form指定target，提交结果定向到隐藏的iframe。
2.  提交完成后，iframe中页面与主页面通信，通知上传结果及服务端文件信息。

`html`

~~~html

<form action="/upload2" enctype="multipart/form-data" method="post" target="frm" onsubmit="loading(true);">
    <p id="upfile">
    附件： <input type="file" name="myfile" style="display: inline">
    </p>
    <p id="upbtn">
    <input class="btn btn-primary btn-sm" style="padding-left:50px;padding-right: 50px;" type="submit" value="异步上传">
    <span id="uptxt" style="display: none">正在上传...</span>
    </p>
</form>

<div id="flist" style="border:1px dotted darkgray;"></div>

~~~

`javascript`

~~~javascript

// 上传完成后的回调
function uploadFinished(fileName) {
    addToFlist(fileName);
    loading(false);
}

function addToFlist(fname) {
    var temp = ["<p id='" + fname + "'>",
                fname,
        "<button onclick='delFile(\"" + fname + "\");'>删除</button>",
                "</p>"
    ];
    $("#flist").append(temp.join(""));
}

function delFile(fname) {
    console.log('to delete file: ' + fname);
    // TODO: 请实现
}

function loading(showloading) {
    if (showloading) {
        $("#uptxt").show();
    } else {
        $("#uptxt").hide();
    }
}
~~~

`node.js`

~~~javascript

var express = require('express');
var router = express.Router();

var multipart = require('connect-multiparty');
var multipartMiddleware = multipart();

var fs = require('fs');

router.all('/', function (req, res) {
    res.sendFile('../public/index.html');
});

router.post('/upload2', multipartMiddleware, function(req, res) {
    console.log(req.body);
    console.log(req.files);

    // 实际编程时，一般要将临时文件移动到目标位置，之后删除临时文件
    // 课程中为简化操作，直接将临时文件当成目标文件
    var fpath = req.files.myfile.path;
    var fname = fpath.substr(fpath.lastIndexOf('\\') + 1);

    setTimeout(function() {
        var ret = ["<script>",
            "window.parent.uploadFinished('" + fname + "');",
            "</script>"];
        res.send(ret.join(""));
    }, 3000);

});

module.exports = router;
~~~

## 使用xhr level 2 纯ajax异步上传

优点：
+  支持H5的浏览器原生支持，不需要插件
+  交互性较好

缺点：
+  受浏览器支持限制

关键过程：

1.  创建FormData，放入待上传文件

2.  通过xhr操作将FormData发送到服务器，实现文件上传

3.  绑定progress、load、error等事件监听传输过程并在页面显示动态交互信息

`html`

~~~html

<div>
    <p id="upfile">附件： <input type="file" id="myfile" style="display: inline"></p>
    <p id="upbtn">
        <input class="btn btn-primary btn-sm" style="padding-left:50px;padding-right: 50px;" type="button" value="异步上传" onclick="upload();">
        <span id="uptxt" style="display: none">正在上传...</span>
        <span id="upprog"></span>
        <button id="stopbtn" style="display:none;">停止上传</button>
    </p>
</div>

<div id="flist" style="border:1px dotted darkgray;"></div>
~~~

`javascript`

~~~javascript

function upload() {
    // 1.准备FormData
    var fd = new FormData();
    fd.append("myfile", $("#myfile")[0].files[0]);

    // 创建xhr对象
    var xhr = new XMLHttpRequest();

    // 监听状态，实时响应
    // xhr 和 xhr.upload 都有progress事件，xhr.progress是下载进度，xhr.upload.progress是上传进度
    xhr.upload.onprogress = function(event) {
        if (event.lengthComputable) {
            var percent = Math.round(event.loaded * 100 / event.total);
            console.log('%d%', percent);
            $("#upprog").text(percent);
        }
    };

    // 传输开始事件
    xhr.onloadstart = function(event) {
        console.log('load start');
        $("#upprog").text('开始上传');

        $("#stopbtn").one('click', function() {
            xhr.abort();
            $(this).hide();
        });

        loading(true);
    };

    // ajax过程成功完成事件
    xhr.onload = function(event) {
        console.log('load success');
        $("#upprog").text('上传成功');

        console.log(xhr.responseText);
        var ret = JSON.parse(xhr.responseText);
        addToFlist(ret.fname);
    };

    // ajax过程发生错误事件
    xhr.onerror = function(event) {
        console.log('error');
        $("#upprog").text('发生错误');
    };

    // ajax被取消
    xhr.onabort = function(event) {
        console.log('abort');
        $("#upprog").text('操作被取消');
    };

    // loadend传输结束，不管成功失败都会被触发
    xhr.onloadend = function (event) {
        console.log('load end');
        loading(false);
    };

    // 发起ajax请求传送数据
    xhr.open('POST', '/upload3', true);
    xhr.send(fd);
}

function addToFlist(fname) {
    var temp = ["<p id='" + fname + "'>",
                fname,
        "<button onclick='delFile(\"" + fname + "\");'>删除</button>",
                "</p>"
                ];
    $("#flist").append(temp.join(""));
}

function delFile(fname) {
    console.log('to delete file: ' + fname);
    // TODO: 请实现
}

function loading(showloading) {
    if (showloading) {
        $("#uptxt").show();
        $("#stopbtn").show();
    } else {
        $("#uptxt").hide();
        $("#stopbtn").hide();
    }
}
~~~

`node.js`

~~~javascript

var express = require('express');
var router = express.Router();

var multipart = require('connect-multiparty');
var multipartMiddleware = multipart();

var fs = require('fs');

router.all('/', function (req, res) {
    res.sendFile('../public/index.html');
});

router.post('/upload3', multipartMiddleware, function(req, res) {
    console.log(req.body);
    console.log(req.files);

    // 实际编程时，一般要将临时文件移动到目标位置，之后删除临时文件
    // 课程中为简化操作，直接将临时文件当成目标文件
    var fpath = req.files.myfile.path;
    var fname = fpath.substr(fpath.lastIndexOf('\\') + 1);

    res.json({fname: fname});
});

module.exports = router;
~~~