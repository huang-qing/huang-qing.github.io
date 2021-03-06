---
layout: post
title: Mock
subtitle:
date: 2021-04-12
author: huangqing
header-img: img/post-bg-frontend.jpg
catalog: true
categories: [Frontend Engineering]
tags:
  - mock
  - Vue3
---

- [Mock.js](http://mockjs.com/)
- [vite-plugin-mock](https://github.com/anncwb/vite-plugin-mock)

## 安装

```
npm i axios -S
npm i  mockjs -S
npm i vite-plugin-mock -D
```

## Mock.js

Mock.js 因为两个重要的特性风靡前端:

**数据类型丰富**

支持生成随机的文本、数字、布尔值、日期、邮箱、链接、图片、颜色等。

**拦截 Ajax 请求**

不需要修改既有代码，就可以拦截 Ajax 请求，返回模拟的响应数据。安全又便捷

Mock 经常使用的 API:

`Mock.mock(rurl?, rtype?, template|function( options ))`

| 参数名     | 参数需求                    | 参数描述       | 例子                   |
| ---------- | --------------------------- | -------------- | ---------------------- | -------------------------- |
| `url`      | 可选: URL 字符串或 URL 正则 | 拦截请求的地址 | `/mock`                |
| `type`     | 可选                        | 拦截 Ajax 类型 | GET、POST、PUT、DELETE |
| `template` | 可选: 可以是对象或字符串    | 生成数据的模板 | `{'data                | 1-10':['mock'] }` `@EMAIL` |

[mockjs examples](http://mockjs.com/examples.html)

生成模拟数据：

```js
// 使用 Mock
var Mock = require("mockjs");

//类型1: 名字|规则: 内容
var data = Mock.mock({
  // 属性 list 的值是一个数组，其中含有 1 到 10 个元素
  "list|1-10": [
    {
      // 属性 id 是一个自增数，起始值为 1，每次增 1
      "id|+1": 1,
    },
  ],
});
// 输出结果
console.log(JSON.stringify(data, null, 4));

//类型2: Mock自带模板
Mock.mock("@province");
```

根据参数创建数据:

```js
function createTableMock(key?: string) {
  let nameList = ["Jon", "Edrward", "God", "HQ", "LJY", "ZLJ", "ZNN", "LLL"];
  if (key) {
    nameList = [key];
  }
  return Mock.mock({
    "rows|10-20": [
      {
        "key|+1": 1,
        "age|1-100": 1,
        "name|1": nameList,
        "address|1": [
          Mock.mock("@county(true)"),
          Mock.mock("@county(true)"),
          Mock.mock("@county(true)"),
          Mock.mock("@county(true)"),
          Mock.mock("@county(true)"),
          Mock.mock("@county(true)"),
        ],
      },
    ],
  });
}
```

ajax 拦截器：

```js
// mock.js

// 添加信息
let projectList = [];
Mock.mock("/mock/addProject", (ops) => {
  // 拦截ajax请求，调用函数
  console.log(ops); // 先看一下这个ops是什么
  projectList.push(ops);
});

// 获取信息
Mock.mock("/mock/projects", projectList);
```

使用 axios 发送请求，会进行拦截

```js
axios.post("/mock/addProject", {
  // 添加数据的接口,数据为一个对象，有个title属性
  title: this.title,
});

axios.get("/mock/projects").then((res) => {
  // 获取数据
  console.log(res.data);
});
```

**使用 Mock.js**

- 直接在需要 Mock 数据的 js 文件使用
- 通过 node 开启 Mock 服务, 可加入到 package.json 命令里

直接创建的 Mock 数据, 在控制台的 network 是无法看到的.而通过服务开启的 Mock,在控制台是真真切切看得到拦截的。

## [vite-plugin-mock](https://github.com/anncwb/vite-plugin-mock/blob/main/README.zh_CN.md)

提供本地和生产模拟服务。

vite 的数据模拟插件，是基于 vite.js 开发的。 并同时支持本地环境和生产环境。 Connect 服务中间件在本地使用，mockjs 在生产环境中使用。

开发环境是使用 Connect 中间件实现的。

与生产环境不同，您可以在 Google Chrome 控制台中查看网络请求记录

```js
//mock get数据
export default [
  {
    url: "/api/getRoleById",
    method: "get",
    response: ({ query }) => {
      console.log("id>>>>>>>>", query.id);
      return {
        code: 0,
        message: "ok",
        data: {
          roleName: "admin",
          roleValue: "admin",
        },
      };
    },
  },
];

// 使用
axios.get("/api/getRoleById", { params: { id: 1 } }).then(({ data }) => {});
```

使用 `post`

```js
//mock post数据
export default [
  {
    url: "/api/createUser",
    method: "post",
    response: ({ body }) => {
      console.log("body>>>>>>>>", body);
      return {
        code: 0,
        message: "ok",
        data: null,
      };
    },
  },
];

// 使用
axios
  .post("/api/createUser", {
    name: "vben",
    gender: "man",
  })
  .then(({ data }) => {
    requestLists.value[1].info = data;
    requestLists.value[1].show = false;
  });
```
