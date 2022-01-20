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

## 准备

- [prettier](https://prettier.io/)
- [ESLint](https://eslint.org/docs/user-guide/getting-started)
- [Stylelint](https://github.com/stylelint/stylelint)
- [EditorConfig](https://editorconfig.org/)

## prettier

- [prettier](https://prettier.io/)

```shell
# install Prettier locally
npm install --save-dev --save-exact prettier
# create an empty config file
{}> .prettierrc.config.js
# create a .prettierignore file
# Base your .prettierignore on .gitignore and .eslintignore
{}> .prettierignore
# format all files with Prettier
npx prettier --write .
#  only checks that files are already formatted, rather than overwriting them.
npx prettier --check .
## format
npx prettier --write . &&  npx prettier --check .
```

### prettier.js

[prettier configuration](https://prettier.io/docs/en/configuration.html)

```js
module.exports = {
  semi: true,
  singleQuote: true,
  endOfLine: "lf",
};
```

### .prettierignore

```yaml
node_modules
dist
build
```

## ESLint

### ESLint 准备

- [eslint](https://eslint.org/)

- [TypeScript ESLint](https://typescript-eslint.io/docs/linting/)
- [@typescript-eslint/parser](https://www.npmjs.com/package/@typescript-eslint/parser)
- [@typescript-eslint](https://www.npmjs.com/package/@typescript-eslint/eslint-plugin)

- [eslint-config-prettier](https://github.com/prettier/eslint-config-prettier)
- [eslint-plugin-prettier](https://github.com/prettier/eslint-plugin-prettier)

- [vue-eslint-parser](https://www.npmjs.com/package/vue-eslint-parser)
- [eslint-plugin-vue](https://eslint.vuejs.org/)

- [vscode-eslint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

### install

```shell
# Install eslint
npm install --save-dev eslint
# Install eslint typescript
npm install --save-dev typescript  @typescript-eslint/parser @typescript-eslint/eslint-plugin
# Install eslint prettier
npm install --save-dev eslint-config-prettier eslint-plugin-prettier
# Install eslint vue
npm install --save-dev vue-eslint-parser eslint-plugin-vue
# lint
npx eslint --fix .
```

### .eslintrc.js

通用的配置：

```js
module.exports = {
  env: {
    browser: true,
    es2021: true,
    node: true,
  },
  extends: [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:vue/vue3-recommended",
    "plugin:prettier/recommended",
    "prettier",
  ],
  // https://github.com/vuejs/vue-eslint-parser#parseroptionsparser
  parser: "vue-eslint-parser",
  parserOptions: {
    ecmaVersion: 13,
    parser: "@typescript-eslint/parser",
    sourceType: "module",
  },
  plugins: ["vue", "@typescript-eslint", "prettier"],
  rules: {
    "prettier/prettier": "warn",
    "@typescript-eslint/explicit-module-boundary-types": "off",
    "@typescript-eslint/no-explicit-any": "off",
    "@typescript-eslint/no-var-requires": "off",
    "no-unused-vars": ["error", { varsIgnorePattern: ".*", args: "none" }],
  },
};
```

对 `extends` 的解释：

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

### .eslintignore

```yaml
node_modules
dist
!.prettierrc.js
```

## stylelint

## stylelint 准备

- [stylelint](https://stylelint.io/)

- [awesome stylelint:configs and plugins listed](https://github.com/stylelint/awesome-stylelint)

- [prettier Integrating with Linters](https://prettier.io/docs/en/integrating-with-linters.html)
- [stylelint-prettier](https://github.com/prettier/stylelint-prettier)
- [stylelint-order](https://github.com/hudochenkov/stylelint-order)

- [stylelint-config-prettier](https://github.com/prettier/stylelint-config-prettier)

- [vscode-stylelint](https://marketplace.visualstudio.com/items?itemName=stylelint.vscode-stylelint)

### install stylelint

```shell
# install Stylelint and its standard configuration
npm install --save-dev stylelint stylelint-config-standard
# install stylelint prettier
npm install --save-dev prettier stylelint-config-prettier stylelint-prettier
# A plugin pack of order-related linting rules for Stylelint
npm install --save-dev stylelint-order
# run stylelint
npx stylelint "**/*.css"
# run stylelint auto fix
npx stylelint "**/*.css" --fix
```

### .stylelintrc.js

```js
module.exports = {
  extends: ["stylelint-config-standard", "stylelint-prettier/recommended"],
  plugins: ["stylelint-prettier"],
  rules: {
    "prettier/prettier": true,
  },
};
```

1. Enables the `stylelint-prettier` plugin.
2. Enables the `prettier/prettier` rule.
3. Extends the `stylelint-config-prettier` configuration.

### .stylelintignore

```yaml
node_modules
dist
```

## git

### .gitattributes

当执行 git 动作时，`.gitattributes` 文件允许你指定由 git 使用的文件和路径的属性，例如：git commit 等。换句话说，每当有文件保存或者创建时，git 会根据指定的属性来自动地保存。其中的一个属性是 `eol`(end of line)，用于配置文件的结尾。

```yaml
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

### 插件

- [VS Code ESLint extension](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
- [vscode-stylelint](https://marketplace.visualstudio.com/items?itemName=stylelint.vscode-stylelint)
- [Prettier Formatter for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
- [Vue Language Features (Volar)](https://marketplace.visualstudio.com/items?itemName=johnsoncodehk.volar)

### extensions.json

.vscode > extensions.json

```json
{
  "recommendations": [
    "johnsoncodehk.volar",
    "stylelint.vscode-stylelint",
    "dbaeumer.vscode-eslint"
  ]
}
```

### .settings.json

按键 「F1」 打开命令面板(Show Command Palette), 选择 「首选项：打开用户设置」,设置工作区配置项。

或者，手动创建 `.vscode/settings.json` 文件。

通过`.vscode/settings.json` 文件，统一配置当前工作区的 vscode 的开发环境。

```json
{
  "files.eol": "\n",
  "eslint.validate": ["javascript", "javascriptreact", "typescript", "vue"],

  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll": true
    //"source.fixAll.stylelint": true
  },
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "[html]": { "editor.defaultFormatter": "esbenp.prettier-vscode" },
  "[vue]": { "editor.defaultFormatter": "esbenp.prettier-vscode" }
}
```

## 参考

- [vue3-eslint-stylelint-demo](https://github.com/sethidden/vue3-eslint-stylelint-demo)
- [prettier-vscode](https://github.com/prettier/prettier-vscode)
- [eslint](https://eslint.org/docs/user-guide/getting-started)
- [eslint-plugin-vue](https://eslint.vuejs.org/)
- [stylelint](https://github.com/stylelint/stylelint)
- [editorconfig](https://editorconfig.org/)