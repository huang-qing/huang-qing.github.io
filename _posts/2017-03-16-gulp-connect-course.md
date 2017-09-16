---
layout:     post
title:      gulp-connect简明教程
subtitle:   学习使用 Glup 插件 - gulp-connect
date:       2017-03-16
author:     huangqing
header-img: img/post-bg-gulp.png
catalog: true
categories: [web]
tags:
    - gulp
---

# gulp-connect简明教程

[gulp-connect github](https://github.com/avevlad/gulp-connect)

## 作用

在进行前前端开发的时候，如果js或css或其他文件有任何改动，网页就会自动加载，不用自己手动去按Ctrl+R或者什么F5。
gulp-connect，它不仅能够自动启动一个web服务器，还能实现上述的热加载的功能。

## 安装

前提是你已经安装好`nodejs`，`gulp`，`npm`，并对他们的使用有基本的了解。且项目下已经初始化好了gulpfile.js和package.js文件。

## 安装命令

~~~
npm install --save-dev gulp-connect
~~~


## 创建一个服务器

使用默认的参数创建一个服务器

~~~javascript
var gulp = require('gulp'),
  connect = require('gulp-connect');
 
gulp.task('connect', function() {
  connect.server();
});
 
gulp.task('default', ['connect']);
~~~

## 监控并自动刷新

监控根目录下的app目录下所有的html文件情况，如有变动，则自动刷新，如果需要监控less，sass,css,js等等，同理。

~~~javascript
var gulp = require('gulp'),
  connect = require('gulp-connect');
 
gulp.task('connect', function() {
  connect.server({
    root: 'app',
    livereload: true
  });
});
 
gulp.task('html', function () {
  gulp.src('./app/*.html')
    .pipe(connect.reload());
});
 
gulp.task('watch', function () {
  gulp.watch(['./app/*.html'], ['html']);
});
 
gulp.task('default', ['connect', 'watch']);
~~~

## 启动与关闭服务器


~~~javascript
gulp.task('jenkins-tests', function() {
  connect.server({
    port: 8888
  });
  // run some headless tests with phantomjs 
  // when process exits: 

  connect.serverClose();
});
~~~

## 启动多个服务器


~~~javascript
var gulp = require('gulp'),
  stylus = require('gulp-stylus'),
  connect = require('gulp-connect');
 
gulp.task('connectDev', function () {
  connect.server({
    root: ['app', 'tmp'],
    port: 8000,
    livereload: true
  });
});
 
gulp.task('connectDist', function () {
  connect.server({
    root: 'dist',
    port: 8001,
    livereload: true
  });
});
 
gulp.task('html', function () {
  gulp.src('./app/*.html')
    .pipe(connect.reload());
});
 
gulp.task('stylus', function () {
  gulp.src('./app/stylus/*.styl')
    .pipe(stylus())
    .pipe(gulp.dest('./app/css'))
    .pipe(connect.reload());
});
 
gulp.task('watch', function () {
  gulp.watch(['./app/*.html'], ['html']);
  gulp.watch(['./app/stylus/*.styl'], ['stylus']);
});
 
gulp.task('default', ['connectDist', 'connectDev', 'watch']);
~~~
