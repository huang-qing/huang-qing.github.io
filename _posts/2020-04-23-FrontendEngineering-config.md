---
layout: post
title: 前端工程配置模板
subtitle: 
date: 2020-04-23
author: huangqing
header-img: img/post-bg-javascript.gif
catalog: true
categories: [Frontend Engineering]
tags:
  - javascript
  - babel
  - webpack
---


# .eslintrc.js

[eslint configuring](https://eslint.org/docs/user-guide/configuring)

```js
module.exports = {
    "extends": "eslint:recommended",
    "rules": {
        // enable additional rules
        "indent": ["error", 4],
        "linebreak-style": ["error", "unix"],
        "quotes": ["error", "double"],
        "semi": ["error", "always"],

        // override default options for rules from base configurations
        "comma-dangle": ["error", "always"],
        "no-cond-assign": ["error", "always"],

        // disable rules from base configurations
        "no-console": ["error", {
            "allow": ["warn", "error", "info"]
        }]
    },
    "parser": "babel-eslint",
    "parserOptions": {
        "ecmaVersion": 6,
        "sourceType": "script"
    },
    "globals": {
        "window": true
    },
    "env": {
        "node": true,
        "es6": true,
        "mocha": true
    }
}

```

## babel.config.js

[babel doc](https://www.babeljs.cn/docs/)

1 
```
npm install --save-dev @bable/core @babel/cli @bable/preset-env @bable/node
```
2
```
npm install --save @babel/polyfill
```
3 `babel.config.js`

4
```javascript
const  presets= [
    [
      "@babel/env",
      {
        "targets": {
          "edge": "17",
          "firefox": "60",
          "chrome": "67",
          "safari": "11.1",
        },
        "useBuiltIns": "usage",
      }
    ]
];

module.exports={presets};
```

5
```
npx bable-node index.js
```

## webpack.config.js

1. [Webpack 5.0](https://webpack.js.org/concepts/)
2. [Webpack 4.43](https://www.webpackjs.com/concepts/)
----
1
```
npm install webpack webpack-cli -D
```

2 `webpack.config.js`

3
```js
const path= require('path');
module.exports = {
    mode:'development', //production development
    entry:'./src/index.js',
    output:{
        path: path.resolve(__dirname, "dist"),
        filename:'bundle.js'
    }
}
```

4 `package.json`
```js
"script":{
    "build":"webpack",
    "dev":"webpack-dev-server"
}
```

