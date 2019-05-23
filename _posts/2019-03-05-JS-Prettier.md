---
layout: post
title: Prettier
subtitle: 使用ESLint ＆ Prettier美化代码
date: 2019-03-05
author: huangqing
header-img: img/post-bg-javascript.jpg
catalog: true
categories: [JavaScript]
tags:
  - javascript
---

# Prettier

`Prettier` 是一个有见识的代码格式化工具。它通过解析代码并使用自己的规则重新打印它，并考虑最大行长来强制执行一致的样式，并在必要时包装代码。如今，它已成为解决所有代码格式问题的优选方案。支持 `JavaScript`、 `Flow`、 `TypeScript`、 `CSS`、 `SCSS`、 `Less`、 `JSX`、 `Vue`、 `GraphQL`、 `JSON`、 `Markdown` 等语言，您可以结合 `ESLint` 和 `Prettier`，检测代码中潜在问题的同时，还能统一团队代码风格，从而促使写出高质量代码，来提升工作效率。

Prettier 具有以下几个有优点：

- 可配置化
- 支持多种语言
- 集成多数的编辑器
- 简洁的配置项

使用 Prettier 在 code review 时不需要再讨论代码样式，节省了时间与精力。下面使用官方的例子来简单的了解下它的工作方式。

Input

```javascript
foo(
  reallyLongArg(),
  omgSoManyParameters(),
  IShouldRefactorThis(),
  isThereSeriouslyAnotherOne()
);
```

Output

```javascript
foo(
  reallyLongArg(),
  omgSoManyParameters(),
  IShouldRefactorThis(),
  isThereSeriouslyAnotherOne()
);
```

## 安装

使用 yarn

```
yarn add prettier --dev --exact
# or globally
yarn global add prettier
```

使用 npm

```
npm install --save-dev --save-exact prettier
# or globally
npm install --global prettier
```

## 使用 ESLint 运行 Prettier

如果你已经在你的项目中使用 ESLint 并且想要只通过单独一条命令来执行你的所有的代码检查的话，你可以使用 ESLint 来为你运行 Prettier。
只需要使用`eslint-plugin-prettier`来添加 Prettier 作为 ESLint 的规则配置。

```
yarn add --dev prettier eslint-plugin-prettier
```

`.eslintrc.json`

```json
{
  "plugins": ["prettier"],
  "rules": {
    "prettier/prettier": "error"
  }
}
```

关闭 ESLint 的格式规则

你是否通过 ESLint 来运行 Prettier，又或者是单独运行两个工具，那你大概只想要每个格式问题只出现一次，而且你特别不想要 ESLint 仅仅是和 Prettier 有简单的不同和偏好而报出“问题”。
所以你大概想要禁用冲突的规则（当保留其他 Prettier 不关心的规则时）最简单的方式是使用`eslint-config-prettier`。它可以添加到任何 ESLint 配置上面。

```
yarn add --dev eslint-config-prettier
```

`.eslintrc.json`

```json
{
  "extends": ["prettier"]
}
```

## VS Code 编辑器

#### 插件

- 安装插件：`ESlint`
- 安装插件：`Prettier`

安装成功后，编辑器默认的格式化处理就会被 `prettier` 代替， 默认快捷键是`alt + shift + f`

#### 配置

插件安装成功后，编辑器的配置 `setting.json`会出现 prettier 插件的相关配置节点，同时也能看到一些默认的配置信息。

VS Code 插件配置统一在 `preference setting user setting` 设置 (快捷键： `Command + ,`)，即可实现保存时自动格式化：

```json
{
  "prettier.eslintIntegration": true,
  "eslint.autoFixOnSave": true,
  "editor.formatOnSave": true
}
```
