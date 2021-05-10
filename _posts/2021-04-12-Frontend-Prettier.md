---
layout: post
title: Prettier
subtitle: 使用ESLint ＆ Prettier美化代码
date: 2021-04-12
author: huangqing
header-img: img/post-bg-prettier.jpg
catalog: true
categories: [Frontend Engineering]
tags:
  - 前端工程化
---

[prettier 官方网站](https://prettier.io/)

借助于`EditorConfig`+`Prettier`+`ESLint` 的组合，项目中通过统一约定配置，可以在团队成员在代码开发过程中就检查、约束、美化代码，统一编码风格；且可以省去很多的沟通成本，提前暴露代码缺陷，减少后期二次修改代码的风险；

简单归纳：

- `EditorConfig`: 跨编辑器和 IDE 编写代码，保持一致的简单编码风格；
- `Prettier`: 专注于代码格式化的工具，美化代码；
- `ESLint`：作代码质量检测、编码风格约束等；

# Prettier

`Prettier` 是一个有见识的代码格式化工具。它通过解析代码并使用自己的规则重新打印它，并考虑最大行长来强制执行一致的样式，并在必要时包装代码。如今，它已成为解决所有代码格式问题的优选方案。支持 `JavaScript`、 `Flow`、 `TypeScript`、 `CSS`、 `SCSS`、 `Less`、 `JSX`、 `Vue`、 `GraphQL`、 `JSON`、 `Markdown` 等语言，您可以结合 `ESLint` 和 `Prettier`，检测代码中潜在问题的同时，还能统一团队代码风格，从而促使写出高质量代码，来提升工作效率。

Prettier 具有以下几个有优点：

- 标准配置格式化代码 Opinionated code formatter
- 支持多种语言 Supports many languages
- 集成多数的编辑器 Integrates with most editors
- 简洁的配置项 Has few options

格式化的过程：

code -> AST -> prettier code

```js
module.exports = {
  printWidth: 100,
  tabWidth: 2,
  useTabs: false,
  semi: true,
  vueIndentScriptAndStyle: true,
  singleQuote: true,
  quoteProps: "as-needed",
  bracketSpacing: true,
  trailingComma: "es5",
  jsxBracketSameLine: false,
  jsxSingleQuote: false,
  arrowParens: "always",
  insertPragma: false,
  requirePragma: false,
  proseWrap: "never",
  htmlWhitespaceSensitivity: "strict",
  endOfLine: "lf", // 规定行尾序列为 LF 模式，可以的vscode右下角的【选择行尾序列】菜单进行调整
  rangeStart: 0,
};
```

## 安装

```
yarn add prettier --dev --exact

yarn prettier --write src/index.js
```

使用 npm

```
npm install --save-dev --save-exact prettier

npx prettier --write src/index.js
```

## ESLint 运行 Prettier

如果你已经在你的项目中使用 ESLint 并且想要只通过单独一条命令来执行你的所有的代码检查的话，你可以使用 ESLint 来为你运行 Prettier。

1 ESLint 的 Prettier 插件

使用`eslint-plugin-prettier`来添加 Prettier 作为 ESLint 的规则配置。

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

2 关闭 ESLint 的格式规则

通过使用`eslint-config-prettier`配置，能够关闭一些不必要的或者是与`prettier`冲突的`lint`选项。这样我们就不会看到一些`error`同时出现两次。使用的时候需要确保，这个配置在`extends`的最后一项。

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

**安装插件：**

- `ESlint Extention`
- `Prettier Extention`

安装成功后，编辑器默认的格式化处理就会被 `prettier` 代替。编辑器的配置 `setting.json`会出现 prettier 插件的相关配置节点，同时也能看到一些默认的配置信息。

**设置默认格式化工具：**

F1 -> Format Code 格式化选定内容方式 -> Prettier (设置为默认值)

**手动格式化：**

默认快捷键是`alt + shift + f`

**设置保存自动格式化：**

VS Code 插件配置统一在 `preference setting user setting` 设置 (快捷键： `Command + ,`)，即可实现保存时自动格式化：

F1 -> Setting -> 文本编辑器 -> 格式化 -> format on save

**统一换行符**

vscode 文件 --> 首选项 --> 设置 --> 搜索 eol，改变 eol 为\n(指 lf),统一的标准。

```json
{
  "prettier.eslintIntegration": true,
  "eslint.autoFixOnSave": true,
  "editor.formatOnSave": true
}
```

## 参考

[Prettier 看这一篇就行了](https://zhuanlan.zhihu.com/p/81764012?from_voters_page=true)
