---
layout:     post
title:      使用Rollup对TS文件进行打包
subtitle:   TS  VUE
date:       2021-07-01
author:     huangqing
header-img: img/post-bg-webpack.jpeg
catalog: true
categories: [Web]
tags:
    - rollup
---



## Vue3文件打包

+ [rollup-plugin-vue.github](https://github.com/vuejs/rollup-plugin-vue)
+ [rollup-plugin-vue.org](https://rollup-plugin-vue.vuejs.org/)


```js
import VuePlugin from 'rollup-plugin-vue';
import PostCSS from 'rollup-plugin-postcss';
import NodeResolve from '@rollup/plugin-node-resolve';
import typescript from 'rollup-plugin-typescript2';
import path from 'path';

/** @type {import('rollup').RollupOptions[]} */
const config = [
  {
    input: path.resolve(__dirname, 'src/index.ts'),
    output: {
      format: 'esm',
      file: 'dist/mylib.esm.js',
      globals: {
        moment: 'moment',
        vue: 'vue',
        lodash: 'lodash',
        'ant-design-vue': 'ant-design-vue',
        'icpx-declaration': 'icpx-declaration',
      },
      //sourcemap: 'inline',
    },
    plugins: [
      // Resolve packages from `node_modules` e.g. `style-inject` module
      // used by `rollup-plugin-postcss` to inline CSS.
      NodeResolve(),
      VuePlugin({
        // PostCSS-modules options for <style module> compilation
        cssModulesOptions: {
          generateScopedName: '[local]___[hash:base64:5]',
        },
      }),
      PostCSS(),
      typescript({
        // Absolute path to import correct config in e2e tests
        tsconfig: path.resolve(__dirname, 'tsconfig.json'),
        removeComments: true,
        useTsconfigDeclarationDir: true,
        tsconfigOverride: {
          include: ['src/*/*.ts', 'src/*/*.vue', 'src/index.ts'],
        },
      }),
    ],
    external: ['vue', 'moment', 'lodash', 'icpx-declaration', 'ant-design-vue'],
  },
];

export default config;

```


