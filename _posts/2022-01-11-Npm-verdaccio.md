---
layout: post
title:
subtitle:
date: 2022-01-11
author: huangqing
header-img: img/post-bg-npm.jpg
catalog: true
categories: [NPM]
tags:
  - npm
---

## npm 私有仓库的原理

![private npm](/images/npm/private-npm.jpg)

用户 install 后向私有 npm 发起请求，服务器会先查询所请求的这个模块是否是我们自己的私有模块或已经缓存过的公共模块，如果是则直接返回给用户；如果请求的是一个还没有被缓存的公共模块，那么则会向上游源请求模块并进行缓存后返回给用户。上游的源可以是 npm 仓库，也可以是淘宝镜像。

## npm 私有仓库框架选型

- [Nexus](https://www.sonatype.com/nexus-repository-oss)
- [Sinopia](https://github.com/rlidwka/sinopia)
- [Verdaccio](https://verdaccio.org)
- [cnpm](https://cnpmjs.org)
- [cpm](https://github.com/cevio/cpm)
- [npmfrog](https://github.com/dmstern/npmfrog)

Nexus 是 Java 社区的一个方案，可以用于 Maven、npm 多种类型的仓库，界面比较丑。

Sinopia 是基于 Node.js 构建的，已经年久失修不维护了。

Verdaccio 比较偏向于一个零配置、轻量型的私有 npm 模块管理工具，不需要额外的数据库配置，它内部自带小型数据库，支持私有模块管理的同时也支持缓存使用过的公共模块，发布及缓存的模块以静态资源形式本地存储。

cnpm 支持静态配置型用户管理机制，以及分层模块权限设置，可以实现公共模块镜像更新以及私有模块管理，支持拓展多种存储形式，相对的数据库的配置较多，部署过程略复杂，是淘宝及多家大型公司搭建内部私有 npm 仓库选择的方案。

## 常用的仓库地址

- [npm —— https://registry.npmjs.org](https://registry.npmjs.org)
- [cnpm —— http://r.cnpmjs.org](http://r.cnpmjs.org)
- [taobao —— https://registry.npm.taobao.org](https://registry.npm.taobao.org)
- [nj —— https://registry.nodejitsu.com](https://registry.nodejitsu.com)
- [rednpm —— http://registry.mirror.cqupt.edu.cn](http://registry.mirror.cqupt.edu.cn)
- [npmMirror —— https://skimdb.npmjs.com/registry](https://skimdb.npmjs.com/registry)
- [edunpm —— http://registry.enpmjs.org](http://registry.enpmjs.org)

## Verdaccio 框架

Verdaccio 是一个 Node.js 创建的轻量的私有 npm proxy registry。

- 基于 Node.js 的网页应用程序
- 私有 npm registry
- 本地网络 proxy
- 可插入式应用程序
- 易安装和使用
- 提供 Docker 和 Kubernetes 支持
- 与 yarn, npm 和 pnpm 100% 兼容
- forked 于 sinopia@1.4.0 并且 100% 向后兼容。

## 实现思路

1. 搭建 verdaccio 私有库
2. 使用 pm2 管理进程，避免服务挂掉的情况
3. 发布安装 npm 包
4. 使用 nrm 管理\切换 npm registry

## node.js 安装 verdaccio

[Verdaccio docs](https://verdaccio.org/zh-CN/docs/installation)

```shell
pnpm install -g verdaccio
```

## 启动 verdaccio

```shell
verdaccio
```

```shell
 warn --- config file  - C:\Users\Administrator\AppData\Roaming\verdaccio\config.yaml
 warn --- Plugin successfully loaded: verdaccio-htpasswd
 warn --- Plugin successfully loaded: verdaccio-audit
 warn --- http address - http://localhost:4873/ - verdaccio/5.4.0
```

## config.yaml 配置

`C:\Users\Administrator\AppData\Roaming\verdaccio\config.yaml`:

逐个认识一下默认配置中的几个值的含义：

- `storage` 已发布的包的存储位置，默认存储在 `~/.config/Verdaccio/` 文件夹下
- `plugins` 插件所在的目录
- `web` 界面相关的配置
- `auth` 用户相关，例如注册、鉴权插件（默认使用的是 `htpasswd`）
- `uplinks` 用于提供对外部包的访问，例如访问 npm、cnpm 对应的源
- `packages` 用于配置发布包、删除包、查看包的权限
- `server` 私有库服务端相关的配置
- `middlewares` 中间件相关配置，默认会引入 `auit` 中间件，来支持 `npm audit` 命令
- `logs` 终端输出的信息的配置

着重配置:

- `storage`：#所有包的缓存目录 建议修改如：D:\npmPackes\storage
- `listen`: 0.0.0.0:4873 #建议追加该配置，不配置只能本地访问，若只是本地访问可不用 。
- 访问控制三种身份：所有人("`$all`"),匿名用户("`$anonymous`"),认证(登陆)用户("`$authenticated`") 。一般发布者都设置成 认证/登陆用户("`$authenticated`")。安装者设置成所有人("$all")。
- `proxy`:代理 表示没有的包 verdaccio 库会去这个 npmjs (即配置中<https://registry.npmjs.org/>)里面去找.

权限把控:

- 禁止用户注册（在团队成员已注册完成后）
- 限制 npm 包的查看，只能为已注册的用户

uplinks常用仓储有:

```shell
npmjs:
url: https://registry.npmjs.org
yarnjs:
url: https://registry.yarnpkg.com
cnpmjs:
url: https://registry.npm.taobao.org
```

```yaml
storage: ./storage
plugins: ./plugins
web:
  title: Verdaccio
auth:
  htpasswd:
    file: ./htpasswd
    # Maximum amount of users allowed to register, defaults to "+inf".
    # You can set this to -1 to disable registration.
    max_users: -1
uplinks:
  npmjs:
    url: https://registry.npmjs.org/
packages:
  "@*/*":
    access: $authenticated
    publish: $authenticated
    unpublish: $authenticated
    proxy: npmjs
  "**":
    access: $authenticated
    publish: $authenticated
    unpublish: $authenticated
    proxy: npmjs
```

## 使用 pm2 启动 verdaccio

pm2 守护 verdaccio 进程:

安装 pm2：

```shell
npm install -g pm2 --unsafe-perm
```

首先找到 verdaccio 的安装目录，就是 npm 安装全局包的路径。

```shell
verdaccio -h
```

```shell
Launch the server

━━━ Usage ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

$ C:\Users\Administrator\AppData\Local\pnpm\nodejs\16.13.2\node.exe C:\Users\Administrator\AppData\Roaming\npm\pnpm-global\5\node_modules\verdaccio\bin\verdaccio
```

找到 verdaccio 文件夹并打开 bin 文件夹：

verdaccio 的启动文件的路径就是 `C:\Users\Administrator\AppData\Roaming\npm\pnpm-global\5\node_modules\verdaccio\bin\verdaccio`

启动 verdaccio:

```shell
pm2 start C:\Users\Administrator\AppData\Roaming\npm\pnpm-global\5\node_modules\verdaccio\bin\verdaccio
```

> 如果你的 `pm2` 的启动路径和 `verdaccio` 的启动文件的路径在一个目录下可直接 `pm2 start verdaccio`

启动成功如下：

| id  | name      | namespace | version | mode | pid   | uptime | ↺   | status | cpu | mem    | user | watching |
| --- | --------- | --------- | ------- | ---- | ----- | ------ | --- | ------ | --- | ------ | ---- | -------- |
| 0   | verdaccio | default   | 5.4.0   | fork | 10340 | 1s     | 0   | online | 0%  | 69.8mb | Adm… | disabled |

status 为“online”即为成功，然后打开对应地址[http://localhost:4873](http://localhost:4873) 或者你配置的端口

查看该进程详细信息:

```shell
pm2 show verdaccio
```

查看错误日志:

```shell
pm2 logs
```

## 用户管理/私有包管理

设置仓库源：

```shell
npm set registry http://localhost:4873
```

```shell
npm install --registry http://localhost:4873
```

添加用户:

```shell
npm adduser --registry http://localhost:4873
```

输入 username、password 以及 Email 即可

登录:

```shell
npm login --registry http://localhost:4873
```

上传私有包:

```shell
npm publish --registry http://localhost:4873
```

## 参考

- [verdaccio](https://www.npmjs.com/package/verdaccio)
- [pm2](https://www.npmjs.com/package/pm2)
- [nrm](https://www.npmjs.com/package/nrm)
- [npm 私有仓库工具 Verdaccio 搭建](https://zhaomenghuan.js.org/blog/npm-private-repository-verdaccio.html)
- [使用 Verdaccio 搭建一个企业级私有 npm 服务器](https://www.mybj123.com/8993.html)
