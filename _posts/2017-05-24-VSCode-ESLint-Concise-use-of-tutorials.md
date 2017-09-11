---
layout:     post
title:      Visual Studio Code ESLint 插件
subtitle:   Visual Studio Code 下使用 ESLint
date:       2017-05-24
author:     huangqing
header-img: img/post-bg-vscode.jpg
catalog: true
categories: [vscode]
tags:
    - vscode
    - eslint
---

# Visual Studio Code ESLint 插件

## 安装ESLint vscode 插件

ESLint dbaeumer.vscode-eslint

[eslint 中文网站](http://eslint.cn/docs/user-guide/configuring)

## 安装ESLint

```sql
cnpm install -g eslint
cnpm install eslint-plugin-react --save-dev
cnpm install eslint-plugin-promise --save-dev
```

## settings.json 文件示例

F1调出控制台:

``` sql
>Open User Settings
```

配置VSCODE全局 settings.json:

``` json 
{
    // ESLint
    "eslint.autoFixOnSave": true,
    "eslint.options": {
        "rules": {
            "semi": 0,
            "indent": 0,
            "one-var": 0,
            "space-before-function-paren": 0
        }
    }
}
```

## .eslintrc.json 文件示例

~~~json
{
    "parserOptions": {
        "ecmaVersion": 6,
        "sourceType": "module",
        "ecmaFeatures": {
            "jsx": true
        }
    },
    "rules": {
        "semi": 2
    }
}
~~~

## package.json 文件示例

~~~json
{
    "env": {
        "browser": true
    },
    "rules": {
        // Override our default settings just for this directory
        "eqeqeq": "warn",
        "strict": "off"
    }
}
~~~

-----

# Error

>Error: Cannot find module 'eslint-config-defaults/configurations/eslint'

~~~javascript
$ npm install --save eslint-config-defaults
~~~
----

>The react/wrap-multilines rule is deprecated. Please use the react/jsx-wrap-multilines rule instead.

~~~javascript
eslint --init

? How would you like to configure ESLint? Answer questions about your style
? Are you using ECMAScript 6 features? Yes
? Are you using ES6 modules? Yes
? Where will your code run? Browser
? Do you use CommonJS? Yes
? Do you use JSX? No
? What style of indentation do you use? Spaces
? What quotes do you use for strings? Single
? What line endings do you use? Windows
? Do you require semicolons? Yes
? What format do you want your config file to be in? JavaScript
~~~
----

> error  Unexpected console statement  no-console

`.eslintrc.js`

~~~javascript
"rules": {
    'no-console': 'off'
}
~~~
----
