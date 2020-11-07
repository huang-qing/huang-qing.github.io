---
layout:     post
title:      file upload
subtitle:   大文件上传
date:       2020-09-24
author:     huangqing
header-img: img/post-bg-html.jpg
catalog: true
categories: [JavaScript]
tags:
    - javascript
---


## 整体思路

前端：

1. 核心是利用 `Blob.prototype.slice` 方法返回原文件的某个切片。
2. 根据预先设置好的切片最大数量将文件切分为一个个切片，然后借助 `http` 的可并发性，同时上传多个切片，这样从原本传一个大文件，变成了同时传多个小的文件切片，可以大大减少上传时间。
3. 由于是并发，传输到服务端的顺序可能会发生变化，所以还需要给每个切片记录顺序。

服务端：

1. 服务端需要负责接受这些切片，并在接收到所有切片后合并切片
2. 前端在每个切片中都携带切片最大数量的信息，当服务端接受到这个数量的切片时自动合并，也可以额外发一个请求主动通知服务端进行切片的合并
3. 使用 nodejs 的 读写流（`readStream`/`writeStream`），将所有切片的流传输到最终文件的流里

## 前端部分

### 上传控件

首先创建选择文件的控件，监听 `change` 事件以及上传按钮

```html
 <template>
   <div>
    <input type="file" @change="handleFileChange" />
    <el-button @click="handleUpload">上传</el-button>
  </div>
</template>

<script>
export default {
  data: () => ({
    container: {
      file: null
    }
  }),
  methods: {
     handleFileChange(e) {
      const [file] = e.target.files;
      if (!file) return;
      Object.assign(this.$data, this.$options.data());
      this.container.file = file;
    },
    async handleUpload() {}
  }
};
</script>
```

### 请求逻辑

用原生 `XMLHttpRequest` 做一层简单的封装来发请求

```js
request({
      url,
      method = "post",
      data,
      headers = {},
      requestList
    }) {
      return new Promise(resolve => {
        const xhr = new XMLHttpRequest();
        xhr.open(method, url);
        Object.keys(headers).forEach(key =>
          xhr.setRequestHeader(key, headers[key])
        );
        xhr.send(data);
        xhr.onload = e => {
          resolve({
            data: e.target.response
          });
        };
      });
    }

```

### 上传切片

+ 对文件进行切片
+ 将切片传输给服务端


```js
const SIZE = 10 * 1024 * 1024; // 切片大小

export default{
    ...

    methods:{
        creatFileChunk(file,size=SIZE){
            const fileChunkList = [];
            let cur = 0;
            while (cur < file.size) {
                fileChunkList.push({ file: file.slice(cur, cur + size) });
                cur += size;
            }
            return fileChunkList;
        },
        async uploadChunks(){
            const requestList=this.data.map({chunk,hash}=>{
                    const formData = new FormData();
                    formData.append("chunk", chunk);
                    formData.append("hash", hash);
                    formData.append("filename", this.container.file.name );
                    return {formData}
                }).map(async ({formData})=>{
                    this.request({
                        url: "http://localhost:3000",
                        data: formData 
                    })
                });

            // 并发切片
            await Promise.all(requestList);
        },
        async handleUpload(){
            if(!this.container.file){
                return;
            }

            const fileChunkList=this.createFileChunk(this.container.file);
            this.data = fileChunkList.map(({file},index)=>({
                chunk:file,
                hash:this.container.file.name + "-" + index
            }));

            await this.uploadChunks();
        }
    }

}
```

调用 `createFileChunk` 将文件切片，切片数量通过文件大小控制，这里设置 10MB，也就是说 100 MB 的文件会被分成 10 个切片

在生成文件切片时，需要给每个切片一个标识作为 hash

调用 `uploadChunks` 上传所有的文件切片，将文件切片，切片 `hash`，以及文件名放入 `FormData` 中，再调用上一步的 `request` 函数返回一个 `proimise`，最后调用 `Promise.all` 并发上传所有的切片


### 发送合并请求

这里使用整体思路中提到的第二种合并切片的方式，即前端主动通知服务端进行合并，所以前端还需要额外发请求，服务端接受到这个请求时主动合并切片

```js
async uploadChunks(){
    ...

    await this.mergeRequest();
},
async mergeRequest() {
    await this.request({
        url: "http://localhost:3000/merge",
        headers: {
            "content-type": "application/json"
        },
        data: JSON.stringify({
            size:SIZE
            filename: this.container.file.name
        })
    });
}
```

## 服务端部分

简单使用 http 模块搭建服务端

```js
const http = require("http");
const server = http.createServer();

server.on("request", async (req, res) => {
  res.setHeader("Access-Control-Allow-Origin", "*");
  res.setHeader("Access-Control-Allow-Headers", "*");
  if (req.method === "OPTIONS") {
    res.status = 200;
    res.end();
    return;
  }
});

server.listen(3000, () => console.log("正在监听 3000 端口"));

```

### 接受切片

使用 `multiparty` 包处理前端传来的 FormData

在 `multiparty.parse` 的回调中，`files` 参数保存了 `FormData` 中文件，`fields` 参数保存了 `FormData` 中非文件的字段

```js
server.on("request", async (req, res) => {
    ...

    const multiparty = require("multiparty");
    multipart.parse(req, async (err, fields, files) => {
        if (err) {
        return;
        }
        const [chunk] = files.chunk;
        const [hash] = fields.hash;
        const [filename] = fields.filename;
        const chunkDir = path.resolve(UPLOAD_DIR, filename);
        // 切片目录不存在，创建切片目录
        if (!fse.existsSync(chunkDir)) {
        await fse.mkdirs(chunkDir);
        }

        await fse.move(chunk.path, `${chunkDir}/${hash}`);
        res.end("received file chunk");
    });

});

```

### 合并切片

在接收到前端发送的合并请求后，服务端将文件夹下的所有切片进行合并.

前端在发送合并请求时会携带文件名，服务端根据文件名可以找到上一步创建的切片文件夹
接着使用 `fs.createWriteStream` 创建一个可写流，可写流文件名就是切片文件夹名 + 后缀名组合而成

随后遍历整个切片文件夹，将切片通过 `fs.createReadStream` 创建可读流，传输合并到目标文件中

值得注意的是每次可读流都会传输到可写流的指定位置，这是通过 `createWriteStream` 的第二个参数 `start/end` 控制的，目的是能够并发合并多个可读流到可写流中，这样即使流的顺序不同也能传输到正确的位置，所以这里还需要让前端在请求的时候多提供一个 `size` 参数

## 显示上传进度条

上传进度分两种，一个是每个切片的上传进度，另一个是整个文件的上传进度，而整个文件的上传进度是基于每个切片上传进度计算而来，所以我们先实现切片的上传进度

切片进度条

`XMLHttpRequest` 原生支持上传进度的监听，只需要监听 `upload.onprogress` 即可，我们在原来的 `request` 基础上传入 `onProgress` 参数，给 `XMLHttpRequest` 注册监听事件

## 断点续传

断点续传的原理在于前端/服务端需要记住已上传的切片，这样下次上传就可以跳过之前已上传的部分，有两种方案实现记忆的功能

+ 前端使用 `localStorage` 记录已上传的切片` hash`
+ 服务端保存已上传的切片 `hash`，前端每次上传前向服务端获取已上传的切片

第一种是前端的解决方案，第二种是服务端，而前端方案有一个缺陷，如果换了个浏览器就失去了记忆的效果，所以这里选取后者

考虑到如果上传一个超大文件，读取文件内容计算 hash 是非常耗费时间的，并且会引起 UI 的阻塞，导致页面假死状态，可以使用 `web-worker` 在 `worker` 线程计算 `hash`

### 生成 hash

无论是前端还是服务端，都必须要生成文件和切片的 hash,根据文件内容生成 hash.

这里用到另一个库 [spark-md5](https://www.npmjs.com/package/spark-md5)，它可以根据文件内容计算出文件的 hash 值

```js
// /public/hash.js
self.importScripts("/spark-md5.min.js"); // 导入脚本

// 生成文件 hash
self.onmessage = e => {
  const { fileChunkList } = e.data;
  const spark = new self.SparkMD5.ArrayBuffer();
  let percentage = 0;
  let count = 0;
  const loadNext = index => {
    const reader = new FileReader();
    reader.readAsArrayBuffer(fileChunkList[index].file);
    reader.onload = e => {
      count++;
      spark.append(e.target.result);
      if (count === fileChunkList.length) {
        self.postMessage({
          percentage: 100,
          hash: spark.end()
        });
        self.close();
      } else {
        percentage += 100 / fileChunkList.length;
        self.postMessage({
          percentage
        });
        // 递归计算下一个切片
        loadNext(count);
      }
    };
  };
  loadNext(0);
};

```

spark-md5 需要根据所有切片才能算出一个 hash 值，不能直接将整个文件放入计算，否则即使不同文件也会有相同的 hash，具体可以看官方文档

### 文件秒传

所谓的文件秒传，即在服务端已经存在了上传的资源，所以当用户再次上传时会直接提示上传成功

文件秒传需要依赖上一步生成的 hash，即在上传前，先计算出文件 hash，并把 hash 发送给服务端进行验证，由于 hash 的唯一性，所以一旦服务端能找到 hash 相同的文件，则直接返回上传成功的信息即可

### 暂停上传

讲完了生成 `hash` 和文件秒传，回到断点续传.断点续传顾名思义即 断点 + 续传，所以我们第一步先实现“断点”，也就是暂停上传。原理是使用 `XMLHttpRequest` 的 `abort` 方法

每当一个切片上传成功时，将对应的 `xhr` 从 `requestList` 中删除，所以 `requestList` 中只保存正在上传切片的 `xhr`

之后新建一个暂停按钮，当点击按钮时，调用保存在 `requestList` 中 `xhr` 的 `abort` 方法，即取消并清空所有正在上传的切片

### 恢复上传

之前在介绍断点续传的时提到使用第二种服务端存储的方式实现续传

由于当文件切片上传后，服务端会建立一个文件夹存储所有上传的切片，所以每次前端上传前可以调用一个接口，服务端将已上传的切片的切片名返回，前端再跳过这些已经上传切片，这样就实现了“续传”的效果

而这个接口可以和之前秒传的验证接口合并，前端每次上传前发送一个验证的请求，返回两种结果

+ 服务端已存在该文件，不需要再次上传
+ 服务端不存在该文件或者已上传部分文件切片，通知前端进行上传，并把已上传的文件切片返回给前端

##  总结

大文件上传

+ 前端上传大文件时使用 `Blob.prototype.slice` 将文件切片，并发上传多个切片，最后发送一个合并的请求通知服务端合并切片
+ 服务端接收切片并存储，收到合并请求后使用流将切片合并到最终文件
+ 原生 `XMLHttpRequest` 的 `upload.onprogress` 对切片上传进度的监听
+ 使用 Vue 计算属性根据每个切片的进度算出整个文件的上传进度

断点续传

+ 使用 `spark-md5` 根据文件内容算出文件 `hash`
+ 通过 `hash` 可以判断服务端是否已经上传该文件，从而直接提示用户上传成功（秒传）
+ 通过 `XMLHttpRequest` 的 `abort` 方法暂停切片的上传
+ 上传前服务端返回已经上传的切片名，前端跳过这些切片的上传

## 参考

[file-upload](https://github.com/yeyan1996/file-upload)

[字节跳动面试官：请你实现一个大文件上传和断点续传](https://juejin.im/post/6844904046436843527#heading-27)