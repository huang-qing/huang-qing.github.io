---
layout: post
title: 前端跨域知识总结
subtitle:
date: 2017-12-03
author: huangqing
header-img: img/post-bg-javascript.jpg
catalog: true
categories: [JavaScript]
tags:
  - 跨域
---

## 什么是跨域

只要`协议`、`域名`、`端口`有任何一个不同，都被当作是不同的域。之所以会产生跨域这个问题，主要就是安全问题。但在安全限制的同时也给注入`iframe`或是`ajax`应用上带来了不少麻烦。

所以我们要通过一些方法使本域的 js 能够操作其他域的页面对象或者使其他域的 js 能操作本域的页面对象（iframe 之间）。

## 同源策略

同源策略是一种约定，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，浏览器很容易受到 `XSS`、`CSFR` 等攻击。

同源是指"协议+域名+端口"三者相同，即便两个不同的域名指向同一个 ip 地址，也非同源。

![同源策略](/images/javascript/2019-03-06_164945.png)

- 限制之一是`不能通过ajax的方法去请求不同源中的文档`。
- 第二个限制是`浏览器中不同域的框架之间不能进行js的交互操作`。

不同的框架之间可以获取 window 对象，但却无法获取相应的属性和方法。

[浏览器同源政策及其规避方法(阮一峰)](http://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html)

比如，有一个页面，它的地址是http://www.damonare.cn/a.html ， 在这个页面里面有一个 iframe，它的 src 是http://damonare.cn/b.html, 很显然，这个页面与它里面的 iframe 框架是不同域的，所以我们是无法通过在页面中书写 js 代码来获取 iframe 中的东西：

```html
<script type="text/javascript">
  function test() {
    var iframe = document.getElementById("￼ifame");
    var win = document.contentWindow; //可以获取到iframe里的window对象，但该window对象的属性和方法几乎是不可用的
    var doc = win.document; //这里获取不到iframe里的document对象
    var name = win.name; //这里同样获取不到window对象的name属性
  }
</script>

<iframe id="iframe" src="http://damonare.cn/b.html" onload="test()"></iframe>
```

同源策略限制内容有：

- Cookie、LocalStorage、IndexedDB 等存储性内容
- DOM 节点
- AJAX 请求发送后，结果被浏览器拦截了

但是有三个标签是允许跨域加载资源：

```html
<img src=XXX>
<link href=XXX>
<script src=XXX>
```

## 常见跨域场景：

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
2. 在跨域问题上，域仅仅是通过“`URL的首部`”来识别而不会去尝试判断相同的 ip 地址对应着两个域或两个域是否在同一个 ip 上。
   (“URL 的首部”指`window.location.protocol` +`window.location.host`，也可以理解为“Domains, protocols and ports must match”。)

**跨域并不是请求发不出去，请求能发出去，服务端能收到请求并正常返回结果，只是结果被浏览器拦截了。**你可能会疑问明明通过表单的方式可以发起跨域请求，为什么 Ajax 就不会?因为归根结底，跨域是为了阻止用户读取到另一个域名下的内容，Ajax 可以获取响应，浏览器认为这不安全，所以拦截了响应。但是表单并不会获取新的内容，所以可以发起跨域请求。同时也说明了跨域并不能完全阻止 `CSRF`，因为请求毕竟是发出去了。

## `document.domain`

`修改document.domain的方法只适用于不同子域的框架间的交互。即只有在主域相同的时候才能使用该方法。`

注意：`document.domain的设置是有限制的，只能把document.domain设置成自身或更高一级的父域，且主域必须相同`。

1. 在 www.a.com/a.html 中：

```javascript
document.domain = "a.com";

var iframe = document.createElement("iframe");
iframe.src = "http://www.script.a.com/b.html";
iframe.display = none;
document.body.appendChild(iframe);
iframe.onload = function() {
  var doc = iframe.contentDocument || iframe.contentWindow.document;
  //在这里操作doc，也就是b.html
  iframe.onload = null;
};
```

2. 在 www.script.a.com/b.html 中：

```javascript
document.domain = "a.com";
```

## `location.hash` + `iframe`

原理是利用 location.hash 来进行传值。

假设域名`a.com`下的文件`cs1.html`要和`cnblogs.com`域名下的`cs2.html`传递信息。

1. cs1.html 首先创建自动创建一个隐藏的 iframe，iframe 的 src 指向 cnblogs.com 域名下的 cs2.html 页面
2. cs2.html 响应请求后再将通过修改 cs1.html 的 hash 值来传递数据
3. 同时在 cs1.html 上加一个定时器，隔一段时间来判断 location.hash 的值有没有变化，一旦有变化则获取获取 hash 值

注：由于两个页面不在同一个域下 IE、Chrome 不允许修改 parent.location.hash 的值，所以要借助于 a.com 域名下的一个代理 iframe

`a.com`下的文件`cs1.html`文件：

```javascript
function startRequest() {
  var ifr = document.createElement("iframe");
  ifr.style.display = "none";
  ifr.src = "http://www.cnblogs.com/lab/cscript/cs2.html#paramdo";
  document.body.appendChild(ifr);
}

function checkHash() {
  try {
    var data = location.hash ? location.hash.substring(1) : "";
    if (console.log) {
      console.log("Now the data is " + data);
    }
  } catch (e) {}
}
setInterval(checkHash, 2000);
```

`cnblogs.com`域名下的`cs2.html`:

```javascript
//模拟一个简单的参数处理操作
switch (location.hash) {
  case "#paramdo":
    callBack();
    break;
  case "#paramset":
    //do something……
    break;
}

function callBack() {
  try {
    parent.location.hash = "somedata";
  } catch (e) {
    // ie、chrome的安全机制无法修改parent.location.hash，
    // 所以要利用一个中间的cnblogs域下的代理iframe
    var ifrproxy = document.createElement("iframe");
    ifrproxy.style.display = "none";
    ifrproxy.src = "http://a.com/test/cscript/cs3.html#somedata"; // 注意该文件在"a.com"域下
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

`window`对象有个`name`属性，该属性有个特征：即在一个窗口(window)的生命周期内,窗口载入的所有的页面都是共享一个 window.name 的，每个页面对 window.name 都有读写的权限，window.name 是持久存在一个窗口载入过的所有页面中的，并不会因新页面的载入而进行重置。

1. 创建 a.com/cs1.html

2. 创建 a.com/proxy.html，并加入如下代码

```html
<head>
  <script>
    function proxy(url, func) {
      var isFirst = true,
        ifr = document.createElement("iframe"),
        loadFunc = function() {
          if (isFirst) {
            //请求http://www.b.com/cs1.html后，b.com/cs1.html设置了windlow.name的值
            //由于同源策略无法直接获取ifr中window.name的值，将url指定为同源站点或about:blank空页面(about:blank，javascript: 和 data: 中的内容，继承了载入他们的页面的源)
            //iframe将会重新加载，再次触发 loadFunc 方法
            ifr.contentWindow.location = "about:blank";
            //ifr.contentWindow.location = 'http://a.com/cs1.html';
            isFirst = false;
          } else {
            //第二次重新加载后，会执行到这个分支，这时可以通过iframe中的window.name获取在http://www.b.com/cs1.html页面中设置的window.name值了
            func(ifr.contentWindow.name);
            ifr.contentWindow.close();
            document.body.removeChild(ifr);
            ifr.src = "";
            ifr = null;
          }
        };

      ifr.src = url;
      ifr.style.display = "none";
      if (ifr.attachEvent) ifr.attachEvent("onload", loadFunc);
      else ifr.onload = loadFunc;

      document.body.appendChild(iframe);
    }
  </script>
</head>
<body>
  <script>
    proxy("http://www.b.com/cs1.html", function(data) {
      console.log(data);
    });
  </script>
</body>
```

3. 在 b.com/cs1.html 中包含：

```javascript
window.name = "要传送的内容";
```

## 动态创建 script（简化版本的 jsonp）

script 标签不受同源策略的限制。

```javascript
function loadScript(url, func) {
  var head = document.head || document.getElementByTagName("head")[0];
  var script = document.createElement("script");
  script.src = url;

  script.onload = script.onreadystatechange = function() {
    if (
      !this.readyState ||
      this.readyState == "loaded" ||
      this.readyState == "complete"
    ) {
      func();
      //返回的执行脚本字符串
      //window.baidu.sug({q:"w",p:false,s:["微信网页版","微信公众平台官网","我的前半生","网易云音乐","微信公众平台","王者荣耀","微博","微信","我们的少年时代","网易邮箱"]});
      script.onload = script.onreadystatechange = null;
    }
  };

  head.insertBefore(script, 0);
}

window.baidu = {
  sug: function(data) {
    console.log(data);
  }
};

loadScript("http://suggestion.baidu.com/su?wd=w", function() {
  console.log("loaded");
});
//我们请求的内容在哪里？
//我们可以在chorme调试面板的source中看到script引入的内容
```

## jsonp


#### JSONP 原理

利用 script 标签没有跨域限制的漏洞，网页可以得到从其他来源动态产生的 JSON 数据。JSONP 请求一定需要对方的服务器做支持才可以。
通过 script 标签引入的 js 是不受同源策略的限制的。所以我们可以通过 script 标签引入一个 js 或者是一个其他后缀形式（如 php，jsp 等）的文件，此文件返回一个 js 函数的调用。


![JSONP 实现原理](/images/javascript/323484072-5a3733859025d_articlex.png)


#### JSONP 和 AJAX 对比

JSONP 和 AJAX 相同，都是客户端向服务器端发送请求，从服务器端获取数据的方式。但 AJAX 属于同源策略，JSONP 属于非同源策略（跨域请求）

#### JSONP 的优缺点：

JSONP 的优点是：

+ 它不像 XMLHttpRequest 对象实现的 Ajax 请求那样受到同源策略的限制；
+ 它的兼容性更好，在更加古老的浏览器中都可以运行，不需要 XMLHttpRequest 或 ActiveX 的支持；
+ 在请求完毕后可以通过调用 callback 的方式回传结果。

JSONP 的缺点则是：

+ 它只支持 GET 请求而不支持 POST 等其它类型的 HTTP 请求；
+ 它只支持跨域 HTTP 请求这种情况，不能解决不同域的两个页面之间如何进行 JavaScript 调用的问题；
+ 安全问题(请求代码中可能存在安全隐患)；不安全可能会遭受 XSS 攻击
+ 要确定 jsonp 请求是否失败并不容易。

#### JSONP 的实现流程

- 声明一个回调函数，其函数名(如 show)当做参数值，要传递给跨域请求数据的服务器，函数形参为要获取目标数据(服务器返回的 data)。
- 创建一个script标签，把那个跨域的 API 数据接口地址，赋值给 script 的 src,还要在这个地址中向服务器传递该函数名（可以通过问号传参:?callback=show）。
- 服务器接收到请求后，需要进行特殊的处理：把传递进来的函数名和它需要给你的数据拼接成一个字符串。
- 最后服务器把准备的数据通过 HTTP 协议返回给客户端，客户端再调用执行之前声明的回调函数，对返回的数据进行操作。


#### 例子

```js
// 封装 jsonp 跨域请求的方法
function jsonp({ url, params, cb }) {
    return new Promise((resolve, reject) => {
        // 创建一个 script 标签帮助我们发送请求
        let script = document.createElement("script");
        let arr = [];
        params = { ...params, cb };

        // 循环构建键值对形式的参数
        for (let key in params) {
            arr.push(`${key}=${params[key]}`);
        }

        // 创建全局函数
        window[cb] = function(data) {
            resolve(data);
            // 在跨域拿到数据以后将 script 标签销毁
            document.body.removeChild(script);
        };

        // 拼接发送请求的参数并赋值到 src 属性
        script.src = `${url}?${arr.join("&")}`;
        document.body.appendChild(script);
    });
}

// 调用方法跨域请求百度搜索的接口
json({
    url: "https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su",
    params: {
        wd: "jsonp"
    },
    cb: "show"
}).then(data => {
    // 打印请求回的数据
    console.log(data);
})
```

```javascript
function handleResponse(response) {
  console.log("The responsed data is: " + response.data);
}
var script = document.createElement("script");
script.src = "http://www.baidu.com/json/?callback=handleResponse";
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

1. a.html

```javascript
<script type="text/javascript">
    function dosomething(jsondata){
        //处理获得的json数据
    }
</script>
<script src="http://example.com/data.php?callback=dosomething"></script>
```

2. data.php

```php
<?php
$callback = $_GET['callback'];//得到回调函数名
$data = array('a','b','c');//要返回的数据
echo $callback.'('.json_encode($data).')';//输出
?>

//最终，输出结果为：dosomething(['a','b','c']);
```

Jquery:

jquery 会自动生成一个全局函数来替换 callback=?中的问号，之后获取到数据后又会自动销毁，实际上就是起一个临时代理函数的作用。

`$.getJSON` 方法会自动判断是否跨域，不跨域的话，就调用普通的 ajax 方法；
跨域的话，则会以异步加载 js 文件的形式来调用 jsonp 的回调函数。

```javascript
$.getJSON('http://example.com/data.php?callback=?,function(jsondata)'){
    //处理获得的json数据
});
```


## `postMessage`

postMessage 是 HTML5 XMLHttpRequest Level 2 中的 API.

高级浏览器 Internet Explorer 8+, chrome，Firefox , Opera 和 Safari 都将支持这个功能。

可用于解决以下方面的问题：

- 页面和其打开的新窗口的数据传递
- 多窗口之间消息传递
- 页面与嵌套的 iframe 消息传递
- 上面三个场景的跨域数据传递

`postMessage()`方法允许来自不同源的脚本采用异步方式进行有限的通信，可以实现跨文本档、多窗口、跨域消息传递。

```javascript
otherWindow.postMessage(message, targetOrigin, [transfer]);
```

- `message`: 将要发送到其他 window 的数据。
- `targetOrigin`:通过窗口的 origin 属性来指定哪些窗口能接收到消息事件，其值可以是字符串"*"（表示无限制）或者一个 URI。在发送消息的时候，如果目标窗口的协议、主机地址或端口这三者的任意一项不匹配 targetOrigin 提供的值，那么消息就不会被发送；只有三者完全匹配，消息才会被发送。
- `transfer`(可选)：是一串和 message 同时传递的 Transferable 对象. 这些对象的所有权将被转移给消息的接收方，而发送一方将不再保有所有权。

`a.com/index.html` 中的代码：
```html
<iframe id="ifr" src="b.com/index.html"></iframe>
<script type="text/javascript">
  window.onload = function() {
    var ifr = document.getElementById("ifr");
    // 若写成'http://b.com/c/proxy.html'效果一样
    var targetOrigin = "http://b.com"; 
    // 若写成'http://c.com'就不会执行postMessage了
    ifr.contentWindow.postMessage("I was there!", targetOrigin);
  };
</script>
```

`b.com/index.html` 中的代码：
```html
<script type="text/javascript">
  window.addEventListener(
    "message",
    function(event) {
      // 通过origin属性判断消息来源地址
      if (event.origin == "http://a.com") {
        // 弹出"I was there!"
        alert(event.data);
        // 对a.com、index.html中window对象的引用 
        // 但由于同源策略，这里event.source不可以访问window对象
        alert(event.source);        
      }
    },
    false
  );
</script>
```

## CORS


`CORS`（Cross-Origin Resource Sharing）跨域资源共享，定义了必须在访问跨域资源时，浏览器与服务器应该如何沟通。
`CORS`背后的基本思想就是使用自定义的`HTTP头部`让浏览器与服务器进行沟通，从而决定请求或响应是应该成功还是失败。


CORS 需要浏览器和后端同时支持。IE 8 和 9 需要通过 XDomainRequest 来实现。


#### CORS 原理:

![](/images/javascript/3236727274-5a37338316b31_articlex.png)

浏览器会自动进行 CORS 通信，实现 CORS 通信的关键是后端。只要后端实现了 CORS，就实现了跨域。`因此，实现CORS通信的关键是服务器。只要服务器实现了CORS接口，就可以跨源通信`。

对于开发者来说，CORS 通信与同源的 AJAX 通信没有差别，代码完全一样。浏览器一旦发现 AJAX 请求跨源，就会自动添加一些附加的头信息，有时还会多出一次附加的请求，但用户不会有感觉。

服务器端对于`CORS`的支持，主要就是通过设置`Access-Control-Allow-Origin`来进行的。该属性表示哪些域名可以访问资源，如果设置通配符则表示所有网站都可以访问资源。浏览器检测到相应的设置，就可以允许 Ajax 进行跨域的访问。

#### 请求


虽然设置 CORS 和前端没什么关系，但是通过这种方式解决跨域问题的话，会在发送请求时出现两种情况，分别为简单请求和复杂请求。

1) 简单请求

只要同时满足以下两大条件，就属于简单请求

条件 1：使用下列方法之一：

- GET
- HEAD
- POST

条件 2：Content-Type 的值仅限于下列三者之一：

- text/plain
- multipart/form-data
- application/x-www-form-urlencoded

请求中的任意 XMLHttpRequestUpload 对象均没有注册任何事件监听器； XMLHttpRequestUpload 对象可以使用 XMLHttpRequest.upload 属性访问。

2) 复杂请求

不符合以上条件的请求就肯定是复杂请求了。

复杂请求的 CORS 请求，会在正式通信之前，增加一次 HTTP 查询请求，称为"预检"请求,该请求是 option 方法的，通过该请求来知道服务端是否允许跨域请求。

我们用PUT向后台请求时，属于复杂请求，后台需做如下配置：

```javascript
// 允许哪个方法访问我
res.setHeader('Access-Control-Allow-Methods', 'PUT')
// 预检的存活时间
res.setHeader('Access-Control-Max-Age', 6)
// OPTIONS请求不做任何处理
if (req.method === 'OPTIONS') {
  res.end()
}
// 定义后台返回的内容
app.put('/getData', function(req, res) {
  console.log(req.headers)
  res.end('我不爱你')
})
```

接下来我们看下一个完整复杂请求的例子，并且介绍下 CORS 请求相关的字段

```javascript
// index.html
let xhr = new XMLHttpRequest()
document.cookie = 'name=xiamen' // cookie不能跨域
xhr.withCredentials = true // 前端设置是否带cookie
xhr.open('PUT', 'http://localhost:4000/getData', true)
xhr.setRequestHeader('name', 'xiamen')
xhr.onreadystatechange = function() {
  if (xhr.readyState === 4) {
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
      console.log(xhr.response)
      //得到响应头，后台需设置Access-Control-Expose-Headers
      console.log(xhr.getResponseHeader('name'))
    }
  }
}
xhr.send()

//server1.js
let express = require('express');
let app = express();
app.use(express.static(__dirname));
app.listen(3000);

//server2.js
let express = require('express')
let app = express()
let whitList = ['http://localhost:3000'] //设置白名单
app.use(function(req, res, next) {
  let origin = req.headers.origin
  if (whitList.includes(origin)) {
    // 设置哪个源可以访问我
    res.setHeader('Access-Control-Allow-Origin', origin)
    // 允许携带哪个头访问我
    res.setHeader('Access-Control-Allow-Headers', 'name')
    // 允许哪个方法访问我
    res.setHeader('Access-Control-Allow-Methods', 'PUT')
    // 允许携带cookie
    res.setHeader('Access-Control-Allow-Credentials', true)
    // 预检的存活时间
    res.setHeader('Access-Control-Max-Age', 6)
    // 允许返回的头
    res.setHeader('Access-Control-Expose-Headers', 'name')
    if (req.method === 'OPTIONS') {
      res.end() // OPTIONS请求不做任何处理
    }
  }
  next()
})
app.put('/getData', function(req, res) {
  console.log(req.headers)
  res.setHeader('name', 'jw') //返回一个响应头，后台需设置
  res.end('我不爱你')
})
app.get('/getData', function(req, res) {
  console.log(req.headers)
  res.end('我不爱你')
})
app.use(express.static(__dirname))
app.listen(4000)
```

上述代码由http://localhost:3000/index.html向http://localhost:4000/跨域请求，正如我们上面所说的，后端是实现 CORS 通信的关键。

#### CORS 和 JSONP 对比:

- JSONP 只能实现 GET 请求，而 CORS 支持所有类型的 HTTP 请求。
- 使用 CORS，开发者可以使用普通的 XMLHttpRequest 发起请求和获得数据，比起 JSONP 有更好的错误处理。
- JSONP 主要被老的浏览器支持，它们往往不支持 CORS，而绝大多数现代浏览器都已经支持了 CORS）。

CORS 与 JSONP 相比，无疑更为先进、方便和可靠。

## web sockets

Websocket 是 HTML5 的一个持久化的协议，它实现了浏览器与服务器的全双工通信，同时也是跨域的一种解决方案。(同源策略对 web sockets 不适用)
WebSocket 和 HTTP 都是应用层协议，都基于 TCP 协议。但是 WebSocket 是一种双向通信协议，在建立连接之后，WebSocket 的 server 与 client 都能主动向对方发送或接收数据。

web sockets 原理：在 JS 创建了 web socket 之后，会有一个 HTTP 请求发送到浏览器以发起连接。取得服务器响应后，建立的连接会使用 HTTP 升级从 HTTP 协议交换为 web sockt 协议。

只有在支持 web socket 协议的服务器上才能正常工作。

```javascript
//http->ws; https->wss
var socket = new WebSockt("ws://www.baidu.com"); 
socket.send("hello WebSockt");
socket.onmessage = function(event) {
  var data = event.data;
};
```

原生 WebSocket API 使用起来不太方便，使用`Socket.io`，它很好地封装了 webSocket 接口，提供了更简单、灵活的接口，也对不支持 webSocket 的浏览器提供了向下兼容。


```html
<div>user input：<input type="text" /></div>
<script src="./socket.io.js"></script>
```

```javascript
var socket = io("http://www.domain2.com:8080");

// 连接成功处理
socket.on("connect", function() {
  // 监听服务端消息
  socket.on("message", function(msg) {
    console.log("data from server: ---> " + msg);
  });

  // 监听服务端关闭
  socket.on("disconnect", function() {
    console.log("Server socket has closed.");
  });
});

document.getElementsByTagName("input")[0].onblur = function() {
  socket.send(this.value);
};
```

```javascript
//nodejs websocket
var http = require("http");
var socket = require("socket.io");

// 启http服务
var server = http.createServer(function(req, res) {
  res.writeHead(200, {
    "Content-type": "text/html"
  });
  res.end();
});

server.listen("8080");
console.log("Server is running at port 8080...");

// 监听socket连接
socket.listen(server).on("connection", function(client) {
  // 接收信息
  client.on("message", function(msg) {
    client.send("hello：" + msg);
    console.log("data from client: ---> " + msg);
  });

  // 断开处理
  client.on("disconnect", function() {
    console.log("Client socket has closed.");
  });
});
```

## nginx 代理跨域

#### 1、 `nginx`配置解决`iconfont`跨域

浏览器跨域访问 js、css、img 等常规静态资源被同源策略许可，但 iconfont 字体文件`(eot|otf|ttf|woff|svg)`例外，此时可在 nginx 的静态资源服务器中加入以下配置。

```javascript
location / {
  add_header Access-Control-Allow-Origin *;
}
```

#### 2、 nginx 反向代理接口跨域

跨域原理： 同源策略是浏览器的安全策略，不是 HTTP 协议的一部分。服务器端调用 HTTP 接口只是使用 HTTP 协议，不会执行 JS 脚本，不需要同源策略，也就不存在跨越问题。

实现思路：通过 nginx 配置一个代理服务器（域名与 domain1 相同，端口不同）做跳板机，反向代理访问 domain2 接口，并且可以顺便修改 cookie 中 domain 信息，方便当前域 cookie 写入，实现跨域登录。

先下载nginx，然后将 nginx 目录下的 nginx.conf 修改如下:

```javascript
// proxy服务器
server {
    listen       80;
    server_name  www.domain1.com;
    location / {
        proxy_pass   http://www.domain2.com:8080;  #反向代理
        proxy_cookie_domain www.domain2.com www.domain1.com; #修改cookie里域名
        index  index.html index.htm;

        # 当用webpack-dev-server等中间件代理接口访问nignx时，此时无浏览器参与，故没有同源限制，下面的跨域配置可不启用
        add_header Access-Control-Allow-Origin http://www.domain1.com;  #当前端只跨域不带cookie时，可为*
        add_header Access-Control-Allow-Credentials true;
    }
}
```

最后通过命令行nginx -s reload启动 nginx

```javascript
// index.html
var xhr = new XMLHttpRequest();
// 前端开关：浏览器是否读写cookie
xhr.withCredentials = true;
// 访问nginx中的代理服务器
xhr.open('get', 'http://www.domain1.com:81/?user=admin', true);
xhr.send();

// server.js
var http = require('http');
var server = http.createServer();
var qs = require('querystring');
server.on('request', function(req, res) {
    var params = qs.parse(req.url.substring(2));
    // 向前台写cookie
    res.writeHead(200, {
        'Set-Cookie': 'l=a123456;Path=/;Domain=www.domain2.com;HttpOnly'   // HttpOnly:脚本无法读取
    });
    res.write(JSON.stringify(params));
    res.end();
});
server.listen('8080');
console.log('Server is running at port 8080...');
```

## `Nodejs`中间件代理跨域

NodeJS 中间件 `http-proxy-middleware` 实现跨域代理，原理大致与 nginx 相同，都是通过启一个代理服务器，实现数据的转发。

实现原理：同源策略是浏览器需要遵循的标准，而如果是服务器向服务器请求就无需遵循同源策略。
代理服务器，需要做以下几个步骤：

- 接受客户端请求 。
- 将请求 转发给服务器。
- 拿到服务器 响应 数据。
- 将 响应 转发给客户端。

![](/images/javascript/2019-03-06_205037.png)


#### 1、 非 vue 框架的跨域（2 次跨域）

利用`node` + `express` + `http-proxy-middleware`搭建一个`proxy`服务器。

1.）前端代码示例：

```javascript
var xhr = new XMLHttpRequest();

// 前端开关：浏览器是否读写cookie
xhr.withCredentials = true;

// 访问http-proxy-middleware代理服务器
xhr.open("get", "http://www.domain1.com:3000/login?user=admin", true);
xhr.send();
```

2.）中间件服务器：

```javascript
var express = require("express");
var proxy = require("http-proxy-middleware");
var app = express();

app.use(
  "/",
  proxy({
    // 代理跨域目标接口
    target: "http://www.domain2.com:8080",
    changeOrigin: true,

    // 修改响应头信息，实现跨域并允许带cookie
    onProxyRes: function(proxyRes, req, res) {
      res.header("Access-Control-Allow-Origin", "http://www.domain1.com");
      res.header("Access-Control-Allow-Credentials", "true");
    },

    // 修改响应信息中的cookie域名
    cookieDomainRewrite: "www.domain1.com" // 可以为false，表示不修改
  })
);

app.listen(3000);
console.log("Proxy server is listen at port 3000...");
```

3.）Nodejs 后台同（nginx）

#### 2、 vue 框架的跨域（1 次跨域）

利用`node` +`webpack` + `webpack-dev-server`代理接口跨域。在开发环境下，由于 vue 渲染服务和接口代理服务都是 webpack-dev-server 同一个，所以页面与代理接口之间不再跨域，无须设置 headers 跨域信息了。

webpack.config.js 部分配置：

```javascript
module.exports = {
    entry: {},
    module: {},
    ...
    devServer: {
        historyApiFallback: true,
        proxy: [{
            context: '/login',
            target: 'http://www.domain2.com:8080',  // 代理跨域目标接口
            changeOrigin: true,
            cookieDomainRewrite: 'www.domain1.com'  // 可以为false，表示不修改
        }],
        noInfo: true
    }
}
```

## 总结

•CORS 支持所有类型的 HTTP 请求，是跨域 HTTP 请求的根本解决方案

•JSONP 只支持 GET 请求，JSONP 的优势在于支持老式浏览器，以及可以向不支持 CORS 的网站请求数据。

•不管是 Node 中间件代理还是 nginx 反向代理，主要是通过同源策略对服务器不加限制。

•日常工作中，用得比较多的跨域方案是 cors 和 nginx 反向代理