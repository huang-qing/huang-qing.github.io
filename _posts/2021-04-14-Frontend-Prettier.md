---
layout: post
title: Prettier 集成
subtitle: ESLint StyleLint VSCode Vue3
date: 2021-04-14
author: huangqing
header-img: img/post-bg-prettier.jpg
catalog: true
categories: [Frontend Engineering]
tags:
  - 前端工程化
---

## prettier

- [prettier](https://prettier.io/)

### prettier.config.js

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
  endOfLine: "lf",
  rangeStart: 0,
  //'vue-indent-script-and-style': false,
  // vueIndentScriptAndStyle: false,
};
```

### .prettierignore

```
/dist/*
.local
.output.js
/node_modules/**

**/*.svg
**/*.sh

/public/*
```

## eslint

### list

- [eslint](https://eslint.org/)

- [@typescript-eslint/parser](https://github.com/typescript-eslint/typescript-eslint/tree/26d71b57fbff013b9c9434c96e2ba98c6c541259/packages/parser)
- [typescript-eslint](https://github.com/typescript-eslint/typescript-eslint)
- [eslint-plugin-prettier](https://github.com/prettier/eslint-plugin-prettier)
- [eslint-plugin-vue](https://eslint.vuejs.org/user-guide/#usage)

- [vue-eslint-parser](https://www.npmjs.com/package/vue-eslint-parser)

- [eslint-config-prettier](https://github.com/prettier/eslint-config-prettier)

- [vscode-eslint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

### install

```
npm install --save-dev prettier

npm install --save-dev eslint
eslint --init

//parse
npm i --save-dev typescript @typescript-eslint/parser
npm install --save-dev eslint vue-eslint-parser

//plugin:vue
npm i --save-dev @typescript-eslint/eslint-plugin
npm install --save-dev eslint-plugin-prettier
npm install --save-dev --save-exact prettier
npm install --save-dev eslint eslint-plugin-vue

//config
npm install --save-dev eslint-config-prettier

```

```js
module.exports = {
  extends: [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:vue/vue3-recommended",
    "prettier",
    "plugin:prettier/recommended",
  ],
  parser: "vue-eslint-parser",
  parserOptions: {
    ecmaVersion: 2020,
    parser: "@typescript-eslint/parser",
    sourceType: "module",
  },
  rules: {
    "prettier/prettier": "error",
  },
};
```

### vscode

**.vscode/settings.json**:

```js
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "vue"
  ]
```

### example

**.eslintrc.json**:

```json
{
  "extends": ["plugin:prettier/recommended"]
}
```

You can then set Prettier's own options inside a `.prettierrc` file.

Exactly what does `plugin:prettier/recommended` do? Well, this is what it expands to:

```json
{
  "extends": ["prettier"],
  "plugins": ["prettier"],
  "rules": {
    "prettier/prettier": "error",
    "arrow-body-style": "off",
    "prefer-arrow-callback": "off"
  }
}
```

- `"extends"`: ["prettier"] enables the config from eslint-config-prettier, which turns off some ESLint rules that conflict with Prettier.
- `"plugins"`: ["prettier"] registers this plugin.
- `"prettier/prettier"`: "error" turns on the rule provided by this plugin, which runs Prettier from within ESLint.
- `"arrow-body-style"`: "off" and "prefer-arrow-callback": "off" turns off two ESLint core rules that unfortunately are problematic with this plugin – see the next section.

## stylelint

### list

- [stylelint](https://stylelint.io/)

- [awesome stylelint:configs and plugins listed](https://github.com/stylelint/awesome-stylelint)

- [stylelint-prettier](https://github.com/prettier/stylelint-prettier)
- [stylelint-order](https://github.com/hudochenkov/stylelint-order)

- [stylelint-config-prettier](https://github.com/prettier/stylelint-config-prettier)

- [vscode-stylelint](https://marketplace.visualstudio.com/items?itemName=stylelint.vscode-stylelint)

### install

```
npm install --save-dev stylelint stylelint-config-standard
npm install --save-dev stylelint-prettier prettier
npm install --save-dev stylelint-config-prettier
npm install stylelint-order --save-dev
```

> stylelint-order 之后用到再补充

### stylelint.config.js

1. Enables the `stylelint-prettier` plugin.
2. Enables the `prettier/prettier` rule.
3. Extends the `stylelint-config-prettier` configuration.

```js
module.exports = {
  plugins: ["stylelint-prettier"],
  extends: [
    "stylelint-config-standard",
    "stylelint-prettier/recommended"
    ]
  rules: {
    "prettier/prettier": true,
  },
};
```

### run stylelint

```
npx stylelint "**/*.css"

//auto fix
npx stylelint "**/*.css" --fix
```

### vscode「」

按键 「F1」 打开命令面板(Show Command Palette), 选择 「首选项：打开用户设置」,设置工作区配置项。

或者，手动创建 `.vscode/settings.json` 文件。

通过`.vscode/settings.json` 文件，统一配置当前工作区的 vscode 的开发环境。

- [vscode-stylelint](https://marketplace.visualstudio.com/items?itemName=stylelint.vscode-stylelint)

```json
 "editor.codeActionsOnSave": {
    "source.fixAll.stylelint": true
  }
```

## Vetur

### list

- [vetur](https://vuejs.github.io/vetur/guide/formatting.html#formatters)
- [vscode-Vetur](https://marketplace.visualstudio.com/items?itemName=octref.vetur)

### format

Current default:

```json
{
  "vetur.format.defaultFormatter.html": "prettier",
  "vetur.format.defaultFormatter.pug": "prettier",
  "vetur.format.defaultFormatter.css": "prettier",
  "vetur.format.defaultFormatter.postcss": "prettier",
  "vetur.format.defaultFormatter.scss": "prettier",
  "vetur.format.defaultFormatter.less": "prettier",
  "vetur.format.defaultFormatter.stylus": "stylus-supremacy",
  "vetur.format.defaultFormatter.js": "prettier",
  "vetur.format.defaultFormatter.ts": "prettier",
  "vetur.format.defaultFormatter.sass": "sass-formatter"
}
```

## git

### .gitattributes

当执行 git 动作时，`.gitattributes` 文件允许你指定由 git 使用的文件和路径的属性，例如：git commit 等。换句话说，每当有文件保存或者创建时，git 会根据指定的属性来自动地保存。其中的一个属性是 `eol`(end of line)，用于配置文件的结尾。

```
*.js    eol=lf
*.ts    eol=lf
*.jsx   eol=lf
*.json  eol=lf
*.css    eol=lf
*.less    eol=lf
*.vue   eol=lf
```

重置 `GitAttributes`

```bash
git rm --cached -r
git reset --hard
```

上面的命令就会根据文件 `.gitattributes` 中的定义，更新文件的结尾行。任何变更都会自动使用指定文件的文件结尾行格式。

## vscode

### .settings.json

```json
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.stylelint": true
  },
  "eslint.validate": ["javascript", "typescript", "html", "vue"],
  "files.eol": "\n",
  "vetur.format.scriptInitialIndent": true,
  "vetur.format.styleInitialIndent": true
}
```
