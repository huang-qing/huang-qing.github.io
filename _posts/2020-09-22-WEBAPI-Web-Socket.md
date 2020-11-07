---
layout:     post
title:      HTML5 WebSocket
subtitle:   
date:       2020-09-22
author:     huangqing
header-img: img/post-bg-html.jpg
catalog: true
categories: [WEB API]
tags:
    - WEB API
---

## 概述

WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。

WebSocket 使得客户端和服务器之间的数据交换变得更加简单，允许服务端主动向客户端推送数据。在 WebSocket API 中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。

很多网站为了实现推送技术，所用的技术都是 Ajax 轮询。轮询是在特定的的时间间隔（如每1秒），由浏览器对服务器发出HTTP请求，然后由服务器返回最新的数据给客户端的浏览器。这种传统的模式带来很明显的缺点，即浏览器需要不断的向服务器发出请求，然而HTTP请求可能包含较长的头部，其中真正有效的数据可能只是很小的一部分，显然这样会浪费很多的带宽等资源。

HTML5 定义的 WebSocket 协议，能更好的节省服务器资源和带宽,并且能够更实时地进行通讯。

![](/images/html5/poll-vs-ws.png)

浏览器通过 JavaScript 向服务器发出建立 WebSocket 连接的请求，连接建立以后，客户端和服务器端就可以通过 TCP 连接直接交换数据。

当你获取 Web Socket 连接后，你可以通过 send() 方法来向服务器发送数据，并通过 onmessage 事件来接收服务器返回的数据。

WebSocket 优点:

+ 较少的控制开销。在连接创建后，服务器和客户端之间交换数据时，用于协议控制的数据包头部相对较小。
+ 更强的实时性。由于协议是全双工的，所以服务器可以随时主动给客户端下发数据。相对于 HTTP 请求需要等待客户端发起请求服务端才能响应，延迟明显更少。
+ 保持连接状态。与 HTTP 不同的是，WebSocket 需要先创建连接，这就使得其成为一种有状态的协议，之后通信时可以省略部分状态信息。
+ 更好的二进制支持。WebSocket 定义了二进制帧，相对 HTTP，可以更轻松地处理二进制内容。
+ 可以支持扩展。WebSocket 定义了扩展，用户可以扩展协议、实现部分自定义的子协议。

由于 WebSocket 拥有上述的优点，所以它被广泛地应用在即时通信、实时音视频、在线教育和游戏等领域。

其他特点包括：

（1）建立在 TCP 协议之上，服务器端的实现比较容易。

（2）与 HTTP 协议有着良好的兼容性。默认端口也是80和443，并且握手阶段采用 HTTP 协议，因此握手时不容易屏蔽，能通过各种 HTTP 代理服务器。

（3）数据格式比较轻量，性能开销小，通信高效。

（4）可以发送文本，也可以发送二进制数据。

（5）**没有同源限制**，客户端可以与任意服务器通信。

（6）协议标识符是`ws`（如果加密，则为`wss`），服务器网址就是 `URL`

```js
var Socket = new WebSocket(url, [protocol] );
```
以上代码中的第一个参数 `url`, 指定连接的 `URL`。第二个参数 `protocol` 是可选的，指定了可接受的子协议。


## WebSocket 属性

+ `binaryType`：使用二进制的数据类型连接。
+ `bufferedAmount`（只读）：未发送至服务器的字节数。
+ `extensions`（只读）：服务器选择的扩展。
+ `url`（只读）：返回值为当构造函数创建 WebSocket 实例对象时 URL 的绝对路径。
+ `protocol`（只读）：用于返回服务器端选中的子协议的名字。
+ `readyState`（只读）：返回当前 WebSocket 的连接状态，共有 4 种状态：
  + `CONNECTING` — 正在连接中，对应的值为 0；
  + `OPEN` — 已经连接并且可以通讯，对应的值为 1；
  + `CLOSING` — 连接正在关闭，对应的值为 2；
  + `CLOSED` — 连接已关闭或者没有连接成功，对应的值为 3。
+ `onclose`：用于指定连接关闭后的回调函数。
+ `onerror`：用于指定连接失败后的回调函数。
+ `onmessage`：用于指定当从服务器接受到信息时的回调函数。
+ `onopen`：用于指定连接成功后的回调函数。

## WebSocket 事件


| 事件    | 事件处理程序     | 描述                       |
| ------- | ---------------- | -------------------------- |
| open    | Socket.onopen    | 连接建立时触发             |
| message | Socket.onmessage | 客户端接收服务端数据时触发 |
| error   | Socket.onerror   | 通信发生错误时触发         |
| close   | Socket.onclose   | 连接关闭时触发             |

## WebSocket 方法

| 方法           | 描述                                                                                                                                                                                   |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Socket.send()  | 该方法将需要通过 WebSocket 链接传输至服务器的数据排入队列，并根据所需要传输的数据的大小来增加 bufferedAmount 的值 。若数据无法传输（比如数据需要缓存而缓冲区已满）时，套接字会自行关闭 |
| Socket.close() | 该方法用于关闭 WebSocket  连接，如果连接已经关闭，则此方法不执行任何操作。                                                                                                             |


## 代码示例

```js

 // 打开一个 web socket
var ws = new WebSocket("ws://localhost:9998/echo");

ws.onopen = function(){
    // Web Socket 已连接上，使用 send() 方法发送数据
    ws.send("发送数据");
};

ws.onmessage = function (evt) { 
    var received_msg = evt.data;
    alert("数据已接收...");
};

ws.onclose = function(){ 
    // 关闭 websocket
    alert("连接已关闭..."); 
};
```

服务器数据可能是文本，也可能是二进制数据（blob对象或Arraybuffer对象）。

### onmessage

```js
ws.onmessage = function(event){
  if(typeof event.data === String) {
    console.log("Received data string");
  }

  if(event.data instanceof ArrayBuffer){
    var buffer = event.data;
    console.log("Received arraybuffer");
  }
}
```

可以使用binaryType属性，显式指定收到的二进制数据类型。

```js
// 收到的是 blob 数据
ws.binaryType = "blob";
ws.onmessage = function(e) {
  console.log(e.data.size);
};

// 收到的是 ArrayBuffer 数据
ws.binaryType = "arraybuffer";
ws.onmessage = function(e) {
  console.log(e.data.byteLength);
};
```

### onsend

```js
//发送文本
ws.send('your message');

//发送 Blob 对象
var file = document
  .querySelector('input[type="file"]')
  .files[0];
ws.send(file);

//发送 ArrayBuffer 对象
// Sending canvas ImageData as ArrayBuffer
var img = canvas_context.getImageData(0, 0, 400, 320);
var binary = new Uint8Array(img.data.length);
for (var i = 0; i < img.data.length; i++) {
  binary[i] = img.data[i];
}
ws.send(binary.buffer);
```

## Websocket握手请求

websocket 使用 ws 或 wss 的统一资源标志符，类似于 HTTPS，其中 wss 表示在 TLS 之上的 Websocket。如：

```
ws://example.com/wsapi
wss://secure.example.com/
```

客户端请求

```
GET / HTTP/1.1
Upgrade: websocket
Connection: Upgrade
Host: example.com
Origin: http://example.com
Sec-WebSocket-Key: sN9cRrP/n9NdMgdcy2VJFQ==
Sec-WebSocket-Version: 13
```

服务器回应
```
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: fFBooB7FAkLlXgRSz0BT3v4hq5s=
Sec-WebSocket-Location: ws://example.com/
```

+ `Connection` 必须设置 `Upgrade` ，表示客户端希望连接升级。
+ `Upgrade` 字段必须设置 `Websocket` ，表示希望升级到 Websocket 协议。
+ `Sec-WebSocket-Key` 是随机的字符串，服务器端会用这些数据来构造出一个 SHA-1 的信息摘要。把“Sec-WebSocket-Key” 加上一个特殊字符串 “258EAFA5-E914-47DA-95CA-C5AB0DC85B11”，然后计算 SHA-1 摘要，之后进行 BASE-64 编码，将结果做为 “Sec-WebSocket-Accept” 头的值，返回给客户端。如此操作，可以尽量避免普通 HTTP 请求被误认为 Websocket 协议。
+ `Sec-WebSocket-Version` 表示支持的 Websocket 版本。RFC6455 要求使用的版本是 `13`。
+ `Origin` 字段是可选的，通常用来表示在浏览器中发起此 Websocket 连接所在的页面，类似于 `Referer` 。但是，与 Referer 不同的是，Origin 只包含了协议和主机名称。
+ 其他一些定义在 HTTP 协议中的字段，如 Cookie 等，也可以在 Websocket 中使用。


Node.js 内置的 http 模块来创建一个 HTTP 服务器:

util.js
```js
const crypto = require("crypto");
const MAGIC_KEY = '258EAFA5-E914-47DA-95CA-C5AB0DC85B11';

function generateAcceptValue(secWskey) {
    return crypto
        .createHash("sha1")
        .update(secWskey + MAGIC_KEY, "utf8")
        .digest("base64");

}

function parseMessage(buffer) {
    // 第一个字节，包含了FIN位，opcode, 掩码位
    const firstByte = buffer.readUInt8(0);
    // 右移7位取首位，1位，表示是否是最后一帧数据
    const isFinalFrame = Boolean((firstByte >>> 7) & 0x01);
    console.log("isFIN: ", isFinalFrame);
    // 取出操作码，低四位
    /**
     * %x0：表示一个延续帧。当 Opcode 为 0 时，表示本次数据传输采用了数据分片，当前收到的数据帧为其中一个数据分片；
     * %x1：表示这是一个文本帧（text frame）；
     * %x2：表示这是一个二进制帧（binary frame）；
     * %x3-7：保留的操作代码，用于后续定义的非控制帧；
     * %x8：表示连接断开；
     * %x9：表示这是一个心跳请求（ping）；
     * %xA：表示这是一个心跳响应（pong）；
     * %xB-F：保留的操作代码，用于后续定义的控制帧。
     */
    const opcode = firstByte & 0x0f;
    if (opcode === 0x08) {
        //连接关闭
        return;
    }
    if (opcode === 0x02) {
        //二进制帧
        return;
    }
    if (opcode === 0x01) {
        //文本帧
        let offset = 1;
        const secondByte = buffer.readUInt8(offset);
        // MASK: 1位，表示是否使用了掩码，在发送给服务端的数据帧里必须使用掩码，而服务端返回时不需要掩码
        const useMask = Boolean((secondByte >>> 7) & 0x01);
        console.log(`use MASK: ${useMask}`);

        const payloadLen = secondByte & 0x7f;// 低7位表示载荷字节长度
        offset += 1;
        // 四个字节的掩码
        let MASK = [];
        // 如果这个值在0-125之间，则后面的4个字节（32位）就应该被直接识别成掩码；
        if (payloadLen <= 0x7d) {
            // 载荷长度小于125
            MASK = buffer.slice(offset, 4 + offset);
            offset += 4;
            console.log(`payload length: ${payloadLen}`);
        }
        else if (payloadLen === 0x7e) {
            // 如果这个值是126，则后面两个字节（16位）内容应该，被识别成一个16位的二进制数表示数据内容大小；
            console.log(`payload length: ${buffer.readInt16BE(offset)}`);
            // 长度是126， 则后面两个字节作为payload length，32位的掩码
            MASK = buffer.slice(offset + 2, offset + 2 + 4);
        }
        else {
            // 如果这个值是127，则后面的8个字节（64位）内容应该被识别成一个64位的二进制数表示数据内容大小
            MASK = buffer.slice(offset + 8, offset + 8 + 4);
            offset += 12;
        }

        // 开始读取后面的payload，与掩码计算，得到原来的字节内容
        const newBuffer = [];
        const dataBuffer = buffer.slice(offset);
        for (let i = 0, j = 0; i < dataBuffer.length; i++, j = i % 4) {
            const nextBuf = dataBuffer[i];
            newBuffer.push(nextBuf ^ MASK[j]);
        }
        return Buffer.from(newBuffer).toString();

    }

    return '';
}

function constructReply(data) {
    const json = JSON.stringify(data);
    const jsonByteLength = Buffer.byteLength(json);
    // 目前只支持小于65535字节的负载
    const lengthByteCount = jsonByteLength < 126 ? 0 : 2;
    const payloadLength = lengthByteCount === 0 ? jsonByteLength : 126;
    const buffer = Buffer.alloc(2 + lengthByteCount + jsonByteLength);
    // 设置数据帧首字节，设置opcode为1，表示文本帧
    buffer.writeUInt8(0b10000001, 0);
    buffer.writeUInt8(payloadLength, 1);
    // 如果payloadLength为126，则后面两个字节（16位）内容应该，被识别成一个16位的二进制数表示数据内容大小
    let payloadOffset = 2;
    if (lengthByteCount > 0) {
        buffer.writeUInt16BE(jsonByteLength, 2);
        payloadOffset += lengthByteCount;
    }
    // 把JSON数据写入到Buffer缓冲区中
    buffer.write(json, payloadOffset);
    return buffer;
}

module.exports = {
    generateAcceptValue,
    parseMessage,
    constructReply,
};
```

server.js
```js
const http = require("http");
const port = 8888;
const { generateAcceptValue, parseMessage, constructReply } = require("./util");

const server = http.createServer((req, res) => {
    res.writeHead(200, { "Content-Type": "text/plain;charset=utf-8" });
    res.end("WebSocket Server");
});

server.on("upgrade", (req, socket) => {
    socket.on("data", (buffer) => {
        const message = parseMessage(buffer);
        if(message){
            console.log(`Message from client: ${message}`);
            socket.write(constructReply({message}));
        }
        else if(message ===null){
            console.log("WebSocket connection closed by the client.");
        }
    });

    if (req.headers["upgrade"] !== "websocket") {
        socket.end("HTTP/1.1 400 Bad Request");
        return;
      }
      // 读取客户端提供的Sec-WebSocket-Key
      const secWsKey = req.headers["sec-websocket-key"];
      // 使用SHA-1算法生成Sec-WebSocket-Accept
      const hash = generateAcceptValue(secWsKey);
      // 设置HTTP响应头
      const responseHeaders = [
        "HTTP/1.1 101 Web Socket Protocol Handshake",
        "Upgrade: WebSocket",
        "Connection: Upgrade",
        `Sec-WebSocket-Accept: ${hash}`,
      ];
      // 返回握手请求的响应信息
      socket.write(responseHeaders.join("\r\n") + "\r\n\r\n");
});

server.listen(port, () =>
  console.log(`Server running at http://localhost:${port}`)
);
```

## 简单应用

### 发送text、file

客户端：
```js
const ws = new WebSocket("ws://localhost:8888");

ws.onopen = function (event) {
    console.log('连接成功');
}
ws.onmessage = async function (event) {
    console.log('message form server ', event.data);
    const data = event.data;
    console.log('text: ' + data);
}

function send() {
    // if (ws && ws.readyState !== WebSocket.OPEN) {
    //     console.log('连接未建立');
    // }

    var file = document
        .querySelector('#file')
        .files[0];
    if (file) {
        let fileName=file.name;
        //发送text
        ws.send(fileName);
        let reader = new FileReader();
        reader.readAsArrayBuffer(file);
        reader.onload = () => {
            console.log(reader.result);
            //发送二进制ArrayBuffer
            ws.send(reader.result);
        }
    }
}

var file = document.getElementById("file");
file.onchange = function (e) {
    send();
}
```

服务器(Node.js)：

```js
let ws = require('nodejs-websocket')
let fs = require('fs')
let fileName = '';
let PORT = 8888;
let path = [];

let server = ws.createServer(function (conn) {
    console.log('New connection')

    conn.on("binary", function (inStream) {
        // Empty buffer for collecting binary data
        var data = Buffer.alloc(0)
        console.log(inStream)
        inStream.on("readable", function () {
            var newData = inStream.read()
            if (newData)
                data = Buffer.concat([data, newData], data.length + newData.length)
        })
        inStream.on("end", function () {
            console.log("Received " + data.length + " bytes of binary data")
            console.log(data)
            fs.writeFile('./files/' + fileName, data, (err) => {
                if (err) throw err;
                console.log('文件已保存');
                let response = {};
                response.message = "文件已保存";
                response.type = 'message';
                conn.sendText(JSON.stringify(response));
            });
        })
    })

    conn.on("text", function (str) {
        console.log("Received" + str);
        fileName = str;
    })

    conn.on("close", function (code, reason) {
        console.log("connection closed")
    })
    
    conn.on("error", function (err) {
        console.log("handle err")
        console.log(err)
    })
}).listen(PORT)

console.log('websocket server listening on port ' + PORT)
```

### 分片传输


客户端：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>websocket sent file</title>
</head>

<body>
    <input type="file" id='file' multiple />
</body>
<script>
    const ws = new WebSocket("ws://localhost:8888");
    var reader = null;
    var offset = 1024 * 256;
    var curLoaded = 0;
    var file = null;
    var total = 0;
    var startTime = null;
    var endTime;

    ws.onopen = function (event) {
        console.log('连接成功');
    }
    ws.onmessage = async function (event) {
        console.log('message form server ', event.data);
        const data = event.data;
        console.log('text: ' + data);

        if (data == 'reveived') {
            return;
        }
        else if (data == 'receiving') {
            sendFile();
        }

    }

    function upload() {
        file = document
            .querySelector('#file')
            .files[0];

        if (file) {
            curLoaded = 0;
            startTime = new Date();
            let fileName = file.name;

            ws.send(fileName);
            sendFile();
        }
    }

    function sendFile() {
        reader = new FileReader();
        reader.onload = function (e) {
            console.log(`读取总数：${e.loaded}`);
            var size = e.loaded;
            var blob = reader.result;
            if (size <= 0) {
                ws.send('uploaded');
            }
            else {
                curLoaded = curLoaded + offset;
                ws.send(blob);
            }

        }
        var blob = file.slice(curLoaded, curLoaded + offset);
        reader.readAsArrayBuffer(blob);
    }


    var file = document.getElementById("file");
    file.onchange = function (e) {
        upload();
    }
</script>

</html>
```

服务端：Node.js

```js
let ws = require('nodejs-websocket')
let fs = require('fs')
let fileName = '';
let PORT = 8888;
let file;

let server = ws.createServer(function (conn) {
    console.log('New connection')
    conn.on("binary", function (inStream) {
        // Empty buffer for collecting binary data
        var data = Buffer.alloc(0);
        console.log(inStream)
        inStream.on("readable", function () {
            var newData = inStream.read()
            if (newData)
                data = Buffer.concat([data, newData], data.length + newData.length);
        })

        inStream.on("end", function () {
            console.log("Received " + data.length + " bytes of binary data")
            console.log(data)
            file = Buffer.concat([file, data], file.length + data.length);
            conn.sendText('receiving');
        })
    })
    conn.on("text", function (str) {

        console.log("Received" + str);
        if (str === 'uploaded') {

            fs.writeFile('./files/' + fileName, file, (err) => {
                if (err) throw err;
                console.log('文件已保存');
                conn.sendText('received');
            });

        }
        else {
            fileName = str;
            file = Buffer.alloc(0);
        }

    })
    conn.on("close", function (code, reason) {
        console.log("connection closed")
    })
    conn.on("error", function (err) {
        console.log("handle err");
        console.log(err);
    })
}).listen(PORT)

console.log('websocket server listening on port ' + PORT);
```

## 扩展

### 消息通信基础

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-------+-+-------------+-------------------------------+
|F|R|R|R| opcode|M| Payload len |    Extended payload length    |
|I|S|S|S|  (4)  |A|     (7)     |             (16/64)           |
|N|V|V|V|       |S|             |   (if payload len==126/127)   |
| |1|2|3|       |K|             |                               |
+-+-+-+-+-------+-+-------------+ - - - - - - - - - - - - - - - +
|     Extended payload length continued, if payload len == 127  |
+ - - - - - - - - - - - - - - - +-------------------------------+
|                               |Masking-key, if MASK set to 1  |
+-------------------------------+-------------------------------+
| Masking-key (continued)       |          Payload Data         |
+-------------------------------- - - - - - - - - - - - - - - - +
:                     Payload Data continued ...                :
+ - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - +
|                     Payload Data continued ...                |
+---------------------------------------------------------------+
```

![](/images/html5/socket-data-frame.jpg)

Payload length 表示以字节为单位的 “有效负载数据” 长度。它有以下几种情形：

+ 如果值为 0-125，那么就表示负载数据的长度。
+ 如果是 126，那么接下来的 2 个字节解释为 16 位的无符号整形作为负载数据的长度。
+ 如果是 127，那么接下来的 8 个字节解释为一个 64 位的无符号整形（最高位的 bit 必须为 0）作为负载数据的长度。

多字节长度量以网络字节顺序表示，有效负载长度是指 “扩展数据” + “应用数据” 的长度。“扩展数据” 的长度可能为 0，那么有效负载长度就是 “应用数据” 的长度。

另外，除非协商过扩展，否则 “扩展数据” 长度为 0 字节。

掩码算法:

掩码字段是一个由客户端随机选择的 32 位的值。掩码值必须是不可被预测的。因此，掩码必须来自强大的熵源（entropy），并且给定的掩码不能让服务器或者代理能够很容易的预测到后续帧。掩码的不可预测性对于预防恶意应用的作者在网上暴露相关的字节数据至关重要。

掩码不影响数据荷载的长度，对数据进行掩码操作和对数据进行反掩码操作所涉及的步骤是相同的。掩码、反掩码操作都采用如下算法：

```
j = i MOD 4
transformed-octet-i = original-octet-i XOR masking-key-octet-j
```
+ `original-octet-i`：为原始数据的第 i 字节。
+ `transformed-octet-i`：为转换后的数据的第 i 字节。
+ `masking-key-octet-j`：为 mask key 第 j 字节。

数据分片:

WebSocket 的每条消息可能被切分成多个数据帧。当 WebSocket 的接收方收到一个数据帧时，会根据 `FIN` 的值来判断，是否已经收到消息的最后一个数据帧

利用 FIN 和 Opcode，我们就可以跨帧发送消息。操作码告诉了帧应该做什么。如果是 0x1，有效载荷就是文本。如果是 0x2，有效载荷就是二进制数据。但是，如果是 0x0，则该帧是一个延续帧。这意味着服务器应该将帧的有效负载连接到从该客户机接收到的最后一个帧。

###  WebSocket 心跳

网络中的接收和发送数据都是使用 SOCKET 进行实现。但是如果此套接字已经断开，那发送数据和接收数据的时候就一定会有问题。可是如何判断这个套接字是否还可以使用呢？这个就需要在系统中创建心跳机制。所谓 “心跳” 就是定时发送一个自定义的结构体（心跳包或心跳帧），让对方知道自己 “在线”。以确保链接的有效性。

而所谓的心跳包就是客户端定时发送简单的信息给服务器端告诉它我还在而已。代码就是每隔几分钟发送一个固定信息给服务端，服务端收到后回复一个固定信息，如果服务端几分钟内没有收到客户端信息则视客户端断开。

在 WebSocket 协议中定义了 心跳 Ping 和 心跳 Pong 的控制帧：

+ 心跳 Ping 帧包含的操作码是 0x9。如果收到了一个心跳 Ping 帧，那么终端必须发送一个心跳 Pong 帧作为回应，除非已经收到了一个关闭帧。否则终端应该尽快回复 Pong 帧。

+ 心跳 Pong 帧包含的操作码是 0xA。作为回应发送的 Pong 帧必须完整携带 Ping 帧中传递过来的 “应用数据” 字段。如果终端收到一个 Ping 帧但是没有发送 Pong 帧来回应之前的 Ping 帧，那么终端可以选择仅为最近处理的 Ping 帧发送 Pong 帧。此外，可以自动发送一个 Pong 帧，这用作单向心跳。

###  WebSocket 与 HTTP

WebSocket 是一种与 HTTP 不同的协议。两者都位于 OSI（Open System Interconnection Model） 模型的应用层，并且都依赖于传输层的 TCP 协议。虽然它们不同，但是 RFC 6455 中规定：WebSocket 被设计为在 HTTP 80 和 443 端口上工作，并支持 HTTP 代理和中介，从而使其与 HTTP 协议兼容。为了实现兼容性，WebSocket 握手使用 HTTP Upgrade 头，从 HTTP 协议更改为 WebSocket 协议。

![](/images/html5/open-system-interconnection-model.png)

### Socket 

网络上的两个程序通过一个双向的通信连接实现数据的交换，这个连接的一端称为一个 socket（套接字），因此建立网络通信连接至少要一对端口号。socket 本质是对 TCP/IP 协议栈的封装，它提供了一个针对 TCP 或者 UDP 编程的接口，并不是另一种协议。通过 socket，你可以使用 TCP/IP 协议。

关于 Socket，可以总结以下几点：

+ 它可以实现底层通信，几乎所有的应用层都是通过 socket 进行通信的。
+ 对 TCP/IP 协议进行封装，便于应用层协议调用，属于二者之间的中间抽象层。
+ TCP/IP 协议族中，传输层存在两种通用协议: TCP、UDP，两种协议不同，因为不同参数的 socket 实现过程也不一样。

下图说明了面向连接的协议的套接字 API 的客户端/服务器关系。

![](/images/html5/socket.jpg)

## websocket支持的服务器

+ node.js - [WebSocket-Node](https://github.com/Worlize/WebSocket-Node)
+ node.js - [Socket.IO](http://socket.io)
+ C++ - [uWebSockets](https://github.com/uWebSockets/uWebSockets)
+ php - http://code.google.com/p/phpwebsocket/
+ jetty - http://jetty.codehaus.org/jetty/（版本7开始支持websocket）
+ netty - http://www.jboss.org/netty
+ ruby - http://github.com/gimite/web-socket-ruby
+ Kaazing - http://www.kaazing.org/confluence/display/KAAZING/Home
+ Tomcat - http://tomcat.apache.org/（建议用tomcat8）
+ WebLogic - http://www.oracle.com/us/products/middleware/cloud-app-foundation/weblogic/overview/index.html（12.1.2開始支持）
+ nginx - http://nginx.com/
+ mojolicious - http://mojolicio.us/
+ python - https://github.com/abourget/gevent-socketio
+ Django - https://github.com/stephenmcd/django-socketio
+ erlang - https://github.com/ninenines/cowboy.git

## 参考

+ [你不知道的 WebSocket](https://mp.weixin.qq.com/s/28FSBp4lj2l1H0caMNrjrw)
+ [HTML5 文件域+FileReader 分段读取文件并上传(七)-WebSocket](https://www.cnblogs.com/tianma3798/p/5852475.html)