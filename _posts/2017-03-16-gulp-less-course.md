---
layout:     post
title:      gulp-less简明教程
subtitle:   学习使用 Glup 插件- gulp-less
date:       2017-03-16
author:     huangqing
header-img: img/post-bg-gulp.png
catalog: true
categories: [Frontend Engineering]
tags:
    - gulp
---

# gulp-less简明教程

使用gulp-less插件将less文件编译成css，当有less文件发生改变自动编译less，并保证less语法错误或出现异常时能正常工作并提示错误信息。

## 安装环境
+ nodejs
+ 全局安装gulp
+ 项目安装gulp
+ 创建package.json
+ gulpfile.js文件


## 本地安装[gulp-less](https://github.com/plus3network/gulp-less)

~~~
cnpm install gulp-less --save-dev
~~~

注意：没有安装cnpm请使用 `npm install gulp-less --save-dev `
说明：`--save-dev` 保存配置信息至 `package.json` 的 `devDependencies` 节点。

## 配置gulpfile.js

### 基本配置

~~~javascript
var gulp = require('gulp'),
    less = require('gulp-less');
 
gulp.task('testLess', function () {
    gulp.src('src/less/index.less')
        .pipe(less())
        .pipe(gulp.dest('src/css'));
});
~~~

### 编译多个less文件

~~~javascript
var gulp = require('gulp'),
    less = require('gulp-less');
 
gulp.task('testLess', function () {
    gulp.src(['src/less/index.less','src/less/detail.less']) //多个文件以数组形式传入
        .pipe(less())
        .pipe(gulp.dest('src/css')); //将会在src/css下生成index.css以及detail.css 
});
~~~

### 匹配符“!”，“*”，“**”，“{}”

~~~javascript
var gulp = require('gulp'),
    less = require('gulp-less');
 
gulp.task('testLess', function () {
    //编译src目录下的所有less文件
    //除了reset.less和test.less（**匹配src/less的0个或多个子文件夹）
    gulp.src(['src/less/*.less', '!src/less/**/{reset,test}.less']) 
        .pipe(less())
        .pipe(gulp.dest('src/css'));
});
~~~

### 调用多模块（编译less后压缩css）

~~~javascript
var gulp = require('gulp'),
    less = require('gulp-less'),
     //确保本地已安装gulp-minify-css [cnpm install gulp-minify-css --save-dev]
    cssmin = require('gulp-minify-css');
 
gulp.task('testLess', function () {
    gulp.src('src/less/index.less')
        .pipe(less())
        .pipe(cssmin()) //兼容IE7及以下需设置compatibility属性 .pipe(cssmin({compatibility: 'ie7'}))
        .pipe(gulp.dest('src/css'));
});
~~~

###  生成sourcemap文件

当less有各种引入关系时，编译后不容易找到对应less文件，所以需要生成sourcemap文件，方便修改。

~~~javascript
var gulp = require('gulp'),
    less = require('gulp-less'),
     //确保本地已安装gulp-sourcemaps [cnpm install gulp-sourcemaps --save-dev]
    sourcemaps = require('gulp-sourcemaps');
 
gulp.task('testLess', function () {
    gulp.src('src/less/**/*.less')
        .pipe(sourcemaps.init())
        .pipe(less())
        .pipe(sourcemaps.write())
        .pipe(gulp.dest('src/css'));
});
~~~

## 执行任务

命令提示符执行：`gulp testLess`

## 监听事件（自动编译less）

若每修改一次less，就要手动执行任务，显然是不合理的，所以当有less文件发生改变时使其自动编译

~~~javascript
var gulp = require('gulp'),
    less = require('gulp-less');
 
gulp.task('testLess', function () {
    gulp.src(['src/less/*.less','!src/less/extend/{reset,test}.less'])
        .pipe(less())
        .pipe(gulp.dest('src/css'));
});
 
gulp.task('testWatch', function () {
    gulp.watch('src/**/*.less', ['testLess']); //当所有less文件发生改变时，调用testLess任务
});
~~~

启动监听事件：命令提示符执行 `gulp testWatch`

注意：该命令提示符执行需处于打开状态，关闭后监听事件结束(Ctrl + C 或右上)

## 异常处理

当编译less时出现语法错误或者其他异常，会终止watch事件，通常需要查看命令提示符窗口才能知道，这并不是我们所希望的，所以我们需要处理出现异常并不终止watch事件（`gulp-plumber`），并提示我们出现了错误（`gulp-notify`）。

~~~javascript
var gulp = require('gulp'),
    less = require('gulp-less');
    //当发生异常时提示错误 确保本地安装gulp-notify和gulp-plumber
    notify = require('gulp-notify'),
    plumber = require('gulp-plumber');
 
gulp.task('testLess', function () {
    gulp.src('src/less/*.less')
        .pipe(plumber({errorHandler: notify.onError('Error: <%= error.message %>')}))
        .pipe(less())
        .pipe(gulp.dest('src/css'));
});
gulp.task('testWatch', function () {
    gulp.watch('src/**/*.less', ['testLess']);
});
~~~