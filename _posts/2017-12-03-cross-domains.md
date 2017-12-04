---
layout:     post
title:      前端跨域知识总结
subtitle:   
date:       2017-12-03
author:     huangqing
header-img: img/post-bg-javascript.jpg
catalog: true
categories: [javascript]
tags:
    - 跨域
---

## 什么是跨域

只要`协议`、`域名`、`端口`有任何一个不同，都被当作是不同的域。之所以会产生跨域这个问题，主要就是安全问题。但在安全限制的同时也给注入`iframe`或是`ajax`应用上带来了不少麻烦。

所以我们要通过一些方法使本域的js能够操作其他域的页面对象或者使其他域的js能操作本域的页面对象（iframe之间）。

## 同源策略

浏览器有一个同源策略，其限制之一是`不能通过ajax的方法去请求不同源中的文档`。 第二个限制是`浏览器中不同域的框架之间不能进行js的交互操作`。

不同的框架之间可以获取window对象，但却无法获取相应的属性和方法。

比如，有一个页面，它的地址是http://www.damonare.cn/a.html ， 在这个页面里面有一个iframe，它的src是http://damonare.cn/b.html, 很显然，这个页面与它里面的iframe框架是不同域的，所以我们是无法通过在页面中书写js代码来获取iframe中的东西：

```html
<script type="text/javascript">
    function test(){
        var iframe = document.getElementById('￼ifame');
        var win = document.contentWindow;//可以获取到iframe里的window对象，但该window对象的属性和方法几乎是不可用的
        var doc = win.document;//这里获取不到iframe里的document对象
        var name = win.name;//这里同样获取不到window对象的name属性
    }
</script>

<iframe id = "iframe" src="http://damonare.cn/b.html" onload = "test()"></iframe>
```

具体说明：

```
URL                           说明                    是否允许通信
http://www.a.com/a.js
http://www.a.com/b.js         同一域名下               允许

http://www.a.com/lab/a.js
http://www.a.com/script/b.js  同一域名下不同文件夹      允许

http://www.a.com:8000/a.js
http://www.a.com/b.js         同一域名，不同端口        不允许

http://www.a.com/a.js
https://www.a.com/b.js        同一域名，不同协议        不允许

http://www.a.com/a.js
http://70.32.92.74/b.js       域名和域名对应ip         不允许

http://www.a.com/a.js
http://script.a.com/b.js      主域相同，子域不同        不允许（cookie这种情况下也不允许访问）

http://www.a.com/a.js
http://a.com/b.js             同一域名，不同二级域名（同上） 不允许（cookie这种情况下也不允许访问）

http://www.cnblogs.com/a.js
http://www.a.com/b.js         不同域名                  不允许
```

注意两点:

1. 如果是`协议和端口`造成的跨域问题“前台”是无能为力的，只能通过后台来解决。
2. 在跨域问题上，域仅仅是通过“`URL的首部`”来识别而不会去尝试判断相同的ip地址对应着两个域或两个域是否在同一个ip上。
(“URL的首部”指`window.location.protocol` +`window.location.host`，也可以理解为“Domains, protocols and ports must match”。)

## `document.domain`

`修改document.domain的方法只适用于不同子域的框架间的交互。即只有在主域相同的时候才能使用该方法。`

注意：`document.domain的设置是有限制的，只能把document.domain设置成自身或更高一级的父域，且主域必须相同`。

1) 在www.a.com/a.html中：

```javascript

document.domain = 'a.com';

var iframe = document.createElement('iframe');
iframe.src = 'http://www.script.a.com/b.html';
iframe.display = none;
document.body.appendChild(iframe);
iframe.onload = function(){
    var doc = iframe.contentDocument || iframe.contentWindow.document;
    //在这里操作doc，也就是b.html
    iframe.onload = null;
};
```

2) 在www.script.a.com/b.html中：

```javascript
document.domain = 'a.com';
```

## `location.hash` + `iframe`

原理是利用location.hash来进行传值。

假设域名`a.com`下的文件`cs1.html`要和`cnblogs.com`域名下的`cs2.html`传递信息。

1. cs1.html首先创建自动创建一个隐藏的iframe，iframe的src指向cnblogs.com域名下的cs2.html页面
2. cs2.html响应请求后再将通过修改cs1.html的hash值来传递数据
3. 同时在cs1.html上加一个定时器，隔一段时间来判断location.hash的值有没有变化，一旦有变化则获取获取hash值

注：由于两个页面不在同一个域下IE、Chrome不允许修改parent.location.hash的值，所以要借助于a.com域名下的一个代理iframe

`a.com`下的文件`cs1.html`文件：

```javascript

function startRequest(){
    var ifr = document.createElement('iframe');
    ifr.style.display = 'none';
    ifr.src = 'http://www.cnblogs.com/lab/cscript/cs2.html#paramdo';
    document.body.appendChild(ifr);
}
 
function checkHash() {
    try {
        var data = location.hash ? location.hash.substring(1) : '';
        if (console.log) {
            console.log('Now the data is '+data);
        }
    } catch(e) {};
}
setInterval(checkHash, 2000);
```

`cnblogs.com`域名下的`cs2.html`:

```javascript
//模拟一个简单的参数处理操作
switch(location.hash){
    case '#paramdo':
        callBack();
        break;
    case '#paramset':
        //do something……
        break;
}
 
function callBack(){
    try {
        parent.location.hash = 'somedata';
    } catch (e) {
        // ie、chrome的安全机制无法修改parent.location.hash，
        // 所以要利用一个中间的cnblogs域下的代理iframe
        var ifrproxy = document.createElement('iframe');
        ifrproxy.style.display = 'none';
        ifrproxy.src = 'http://a.com/test/cscript/cs3.html#somedata';    // 注意该文件在"a.com"域下
        document.body.appendChild(ifrproxy);
    }
}
```

`a.com`下的域名`cs3.html`

```javascript
//因为parent.parent和自身属于同一个域，所以可以改变其location.hash的值
parent.parent.location.hash = self.location.hash.substring(1);
```

## `window.name` + `iframe`

`window.name` 的美妙之处：`name` 值在不同的页面（甚至不同域名）加载后依旧存在，并且可以支持非常长的 name 值（2MB）。

这种方法与 `document.domain` 方法相比，放宽了域名后缀要相同的限制，可以从任意页面获取`string` 类型的数据。

`window`对象有个`name`属性，该属性有个特征：即在一个窗口(window)的生命周期内,窗口载入的所有的页面都是共享一个window.name的，每个页面对window.name都有读写的权限，window.name是持久存在一个窗口载入过的所有页面中的，并不会因新页面的载入而进行重置。

1) 创建a.com/cs1.html

2) 创建a.com/proxy.html，并加入如下代码

```html
<head>
  <script>
  function proxy(url, func){
    var isFirst = true,
        ifr = document.createElement('iframe'),
        loadFunc = function(){
          if(isFirst){
            //请求http://www.b.com/cs1.html后，b.com/cs1.html设置了windlow.name的值
            //由于同源策略无法直接获取ifr中window.name的值，将url指定为同源站点或about:blank空页面(about:blank，javascript: 和 data: 中的内容，继承了载入他们的页面的源)
            //iframe将会重新加载，再次触发 loadFunc 方法
            ifr.contentWindow.location = 'about:blank';
            //ifr.contentWindow.location = 'http://a.com/cs1.html';
            isFirst = false;
          }else{
            //第二次重新加载后，会执行到这个分支，这时可以通过iframe中的window.name获取在http://www.b.com/cs1.html页面中设置的window.name值了
            func(ifr.contentWindow.name);
            ifr.contentWindow.close();
            document.body.removeChild(ifr);
            ifr.src = '';
            ifr = null;
          }
        };
 
    ifr.src = url;
    ifr.style.display = 'none';
    if(ifr.attachEvent) ifr.attachEvent('onload', loadFunc);
    else ifr.onload = loadFunc;
 
    document.body.appendChild(iframe);
  }
</script>
</head>
<body>
  <script>
    proxy('http://www.b.com/cs1.html', function(data){
      console.log(data);
    });
  </script>
</body>
```

3) 在b.com/cs1.html中包含：

```javascript

window.name = '要传送的内容';

```

## 动态创建script（简化版本的jsonp）

script标签不受同源策略的限制。

```javascript

function loadScript(url, func) {
  var head = document.head || document.getElementByTagName('head')[0];
  var script = document.createElement('script');
  script.src = url;
 
  script.onload = script.onreadystatechange = function(){
    if(!this.readyState || this.readyState=='loaded' || this.readyState=='complete'){
      func();
      //返回的执行脚本字符串
      //window.baidu.sug({q:"w",p:false,s:["微信网页版","微信公众平台官网","我的前半生","网易云音乐","微信公众平台","王者荣耀","微博","微信","我们的少年时代","网易邮箱"]});
      script.onload = script.onreadystatechange = null;
    }
  };
 
  head.insertBefore(script, 0);
}

window.baidu = {
  sug: function(data){
    console.log(data);
  }
}

loadScript('http://suggestion.baidu.com/su?wd=w',function(){console.log('loaded')});
//我们请求的内容在哪里？
//我们可以在chorme调试面板的source中看到script引入的内容

```

## jsonp

通过script标签引入的js是不受同源策略的限制的。所以我们可以通过script标签引入一个js或者是一个其他后缀形式（如php，jsp等）的文件，此文件返回一个js函数的调用。

```javascript
function handleResponse(response){
    console.log('The responsed data is: '+response.data);
}
var script = document.createElement('script');
script.src = 'http://www.baidu.com/json/?callback=handleResponse';
document.body.insertBefore(script, document.body.firstChild);
/*handleResonse({"data": "zhe"})*/
//原理如下：
//当我们通过script标签请求时
//后台就会根据相应的参数(json,handleResponse)
//来生成相应的json数据(handleResponse({"data": "zhe"}))
//最后这个返回的json数据(代码)就会被放在当前js文件中被执行
//至此跨域通信完成
```

再来一个例子：

1) a.html

```javascript
<script type="text/javascript">
    function dosomething(jsondata){
        //处理获得的json数据
    }
</script>
<script src="http://example.com/data.php?callback=dosomething"></script>
```

2) data.php

```php
<?php
$callback = $_GET['callback'];//得到回调函数名
$data = array('a','b','c');//要返回的数据
echo $callback.'('.json_encode($data).')';//输出
?>

//最终，输出结果为：dosomething(['a','b','c']);
```

Jquery:

jquery会自动生成一个全局函数来替换callback=?中的问号，之后获取到数据后又会自动销毁，实际上就是起一个临时代理函数的作用。

$.getJSON方法会自动判断是否跨域，不跨域的话，就调用普通的ajax方法；
跨域的话，则会以异步加载js文件的形式来调用jsonp的回调函数。

```javascript
$.getJSON('http://example.com/data.php?callback=?,function(jsondata)'){
    //处理获得的json数据
});
```

JSONP的优缺点：

JSONP的优点是：它不像XMLHttpRequest对象实现的Ajax请求那样受到同源策略的限制；它的兼容性更好，在更加古老的浏览器中都可以运行，不需要XMLHttpRequest或ActiveX的支持；并且在请求完毕后可以通过调用callback的方式回传结果。

JSONP的缺点则是：它只支持GET请求而不支持POST等其它类型的HTTP请求；它只支持跨域HTTP请求这种情况，不能解决不同域的两个页面之间如何进行JavaScript调用的问题；安全问题(请求代码中可能存在安全隐患)；要确定jsonp请求是否失败并不容易。

## `postMessage`

`HTML5`中的`XMLHttpRequest Level 2`中的API。

高级浏览器Internet Explorer 8+, chrome，Firefox , Opera 和 Safari 都将支持这个功能。这个功能主要包括接受信息的`message`事件和发送消息的`postMessage`方法。

`otherWindow.postMessage(message, targetOrigin)`:

`otherWindow`:指目标窗口，也就是给哪个window发消息，是 window.frames 属性的成员或者由 window.open 方法创建的窗口
`message`: 是要发送的消息，类型为 String、Object (IE8、9 不支持)
`targetOrigin`: 是限定消息接收范围，不限制请使用 `*`

1) a.com/index.html中的代码：

```html
<iframe id="ifr" src="b.com/index.html"></iframe>
<script type="text/javascript">
window.onload = function() {
    var ifr = document.getElementById('ifr');
    var targetOrigin = 'http://b.com';  // 若写成'http://b.com/c/proxy.html'效果一样
                                        // 若写成'http://c.com'就不会执行postMessage了
    ifr.contentWindow.postMessage('I was there!', targetOrigin);
};
</script>
```

2) b.com/index.html中的代码：

```html
<script type="text/javascript">
    window.addEventListener('message', function(event){
        // 通过origin属性判断消息来源地址
        if (event.origin == 'http://a.com') {
            alert(event.data);    // 弹出"I was there!"
            alert(event.source);  // 对a.com、index.html中window对象的引用
                                  // 但由于同源策略，这里event.source不可以访问window对象
        }
    }, false);
</script>
```


## CORS

`CORS`（Cross-Origin Resource Sharing）跨域资源共享，定义了必须在访问跨域资源时，浏览器与服务器应该如何沟通。
`CORS`背后的基本思想就是使用自定义的`HTTP头部`让浏览器与服务器进行沟通，从而决定请求或响应是应该成功还是失败。

目前，所有浏览器都支持该功能，IE浏览器不能低于IE10。整个CORS通信过程，都是浏览器自动完成，不需要用户参与。
对于开发者来说，CORS通信与同源的AJAX通信没有差别，代码完全一样。浏览器一旦发现AJAX请求跨源，就会自动添加一些附加的头信息，有时还会多出一次附加的请求，但用户不会有感觉。

`因此，实现CORS通信的关键是服务器。只要服务器实现了CORS接口，就可以跨源通信`。

```javascript
    var xhr = new XMLHttpRequest();
    xhr.open("￼GET", "http://segmentfault.com/u/trigkit4/",true);
    xhr.send();
```

服务器端对于`CORS`的支持，主要就是通过设置`Access-Control-Allow-Origin`来进行的。如果浏览器检测到相应的设置，就可以允许Ajax进行跨域的访问。

CORS和JSONP对比:

+ JSONP只能实现GET请求，而CORS支持所有类型的HTTP请求。
+ 使用CORS，开发者可以使用普通的XMLHttpRequest发起请求和获得数据，比起JSONP有更好的错误处理。
+ JSONP主要被老的浏览器支持，它们往往不支持CORS，而绝大多数现代浏览器都已经支持了CORS）。

CORS与JSONP相比，无疑更为先进、方便和可靠。

## web sockets

web sockets是一种浏览器的API，它的目标是在一个单独的持久连接上提供全双工、双向通信。(同源策略对web sockets不适用)

web sockets原理：在JS创建了web socket之后，会有一个HTTP请求发送到浏览器以发起连接。取得服务器响应后，建立的连接会使用HTTP升级从HTTP协议交换为web sockt协议。

只有在支持web socket协议的服务器上才能正常工作。

```javascript
var socket = new WebSockt('ws://www.baidu.com');//http->ws; https->wss
socket.send('hello WebSockt');
socket.onmessage = function(event){
    var data = event.data;
}
```
