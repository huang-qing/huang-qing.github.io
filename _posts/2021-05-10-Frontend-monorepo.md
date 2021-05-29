---
layout: post
title: monorepo
subtitle: 使用 lerna 和 yarn 构建 monorepo 项目
date: 2021-05-10
author: huangqing
header-img: img/post-bg-frontend.jpg
catalog: true
categories: [Frontend Engineering]
tags:
  - 前端工程化
  - monorepo
---

使用 `lerna` 和 `yarn` 构建 `monorepo` 项目，核心思想是 用 `yarn` 来处理依赖问题，用 `lerna` 来处理发布问题。

## Monorepo

Monorepo(monolithic repository) 是管理项目代码的一个方式，指在一个项目仓库 (repo) 中管理多个模块/包 (package)，不同于常见的每个模块建一个 repo

- 多个项目的代码放在在同一存储库中这种开发策略称之为 `Monorepo`
- [`lerna`](https://lerna.js.org/) Babel 开发用来管理多包的工具，基于 `Monorepo` 理念在工具端的实现
- [`yarn`](https://classic.yarnpkg.com/en/) Facebook 贡献的 Javascript 包管理器
- `commitlint` 用来规范`git commit`信息

```
├── packages
|   ├── pkg1
|   |   ├── package.json
|   ├── pkg2
|   |   ├── package.json
├── package.json
```

monorepo 最主要的好处是统一的工作流和 Code Sharing。比如我想看一个 pacakge 的代码、了解某段逻辑，不需要找它的 repo，直接就在当前 repo；当某个需求要修改多个 pacakge 时，不需要分别到各自的 repo 进行修改、测试、发版或者 npm link，直接在当前 repo 修改，统一测试、统一发版。只要搭建一套脚手架，就能管理（构建、测试、发布）多个 package。
但是 repo 的体积较大。因为各个 package 理论上都是独立的，所以每个 package 都维护着自己的 dependencies，而很大的可能性，package 之间有不少相同的依赖，而这就可能使 install 时出现重复安装。

实际开发中的场景，「Monorepo」的使用通常是通过 `yarn` 的 `workspaces` 工作空间，又或者是 `lerna` 这种第三方工具库来实现。使用「Monorepo」的方式来管理项目会给我们带来以下这些好处：

+ 只需要一个仓库，就可以便捷地管理多个项目。
+ 可以管理不同项目中的相同第三方依赖，做到依赖的同步更新。
+ 可以使用其他项目中的代码，清晰地建立起项目间的依赖关系。

「Vue3」正是采用的 yarn 的 workspaces 工作空间的方式管理整个项目:

![vue2 monorepo](/images/monorepo/vue3-monorepo.png)

## Yarn Workspace

Yarn 是一个软件包管理器，还可以作为项目管理工具。无论你是小型项目还是大型单体仓库(monorepos)。

Yarn 从 1.0 开始支持 Workspace。这个功能的主要作用是让多个项目共用一个 `node_modules` 目录，提升开发效率和降低磁盘空间占用。

yarn 突出的是对依赖的管理，包括 packages 的相互依赖、packages 对第三方的依赖，yarn 会以 semver 约定来分析 dependencies 的版本，安装依赖时更快、占用体积更小；但欠缺了「统一工作流」方面的实现。

- [yarn](https://yarnpkg.com/)
- [yarn workspaces](https://classic.yarnpkg.com/en/docs/workspaces)

### 配置

```
├── packages
|   ├── workspace-a
|   |   ├── package.json
|   ├── workspace-b
|   |   ├── package.json
├── package.json
```

package.json

```json
{
  "private": true,
  "workspaces": ["workspace-a", "workspace-b"]
}
```

或

```json
{
  "private": true,
  "workspaces": ["packages/*"]
}
```

workspace-a/package.json:

```json
{
  "name": "workspace-a",
  "version": "1.0.0",

  "dependencies": {
    "cross-env": "5.0.5"
  }
}
```

workspace-b/package.json:

```json
{
  "name": "workspace-b",
  "version": "1.0.0",

  "dependencies": {
    "cross-env": "5.0.5",
    "workspace-a": "1.0.0"
  }
}
```

### 使用

> 注意：执行安装之前,删除主目录下的 `node_modules`,并删除 `packages` 目录下全部的 `node_modules`

**[安装工程依赖包:](https://yarnpkg.com/cli/workspace)，**在主工程上执行一次即可**。**

```shell
yarn install
```

**[执行 pagkages 中的项目](https://yarnpkg.com/cli/workspace/#gatsby-focus-wrapper)**

```shell
yarn workspace <workspaceName> <commandName> ...
```

例如：

```
yarn workspace workspace-a run dev
```

**使用 `packages` 中的工程包：**

workspace-b -> package.json

```json
  "dependencies": {
     "workspace-a": "1.0.0"
  }
```

**查看依赖树关系：**

```
yarn workspaces info
```

**安装/删除依赖：**

单个 package 工作区

```shell
# packageA 安装 axios
yarn workspace packageA add axios

# packageA 移除 axios
yarn workspace packageA remove axios
```

root package

```shell
# root package 安装 commitizen
yarn add -W -D commitizen

# root package 移除 commitizen
yarn remove -W commitizen
```

安装完依赖文件结构如下：

![yarn-workspace-files](/images/yarn/yarn-workspace-files.jpg)

## Lerna

## 举例

用 `monorepo` 的形式来管理这个仓库。

由于他们使用了相同的技术栈，那么 `eslint` 、 `prettier` ，甚至 `webpack` 配置都可以提取到最外面，不用维护在每个项目里面。

以 create-react-app peject 之后的配置为例：

```
- node_modules
  - react
  - react dom
  - redux
  - lodash
- packages
  - project1
    - package.json
  - project2
    - package.json
- config
  - webpack.config.js
  - webpack.dev.config.js
- scripts
  - create.js
  - bin.js
  - build.js
  - start.js
- .eslintrc.js
- .prettierrc
- commitlint.config.js
- jest.config.js
- tsconfig.js
- package.json
- yarn.lock
```

我们可以看到，通用配置都被提取到了最外层。

## 参考资料

- [Monorepo 实战](https://www.jianshu.com/p/dafc2052eedc)
- [Yarn Workspace 的简单使用](https://www.yuque.com/negivup/blog/nyw4yx)
