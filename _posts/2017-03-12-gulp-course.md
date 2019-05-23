---
layout:     post
title:      Glup 简明使用教程
subtitle:   
date:       2017-03-12
author:     huangqing
header-img: img/post-bg-gulp.png
catalog: true
categories: [Gulp]
tags:
    - gulp
---


# Glup 简明使用教程

## 安装：
~~~sql
$ npm install gulp -g
$ npm install gulp --save-dev
~~~

## 安装gulp插件

+ 编译Sass([gulp-sass](https://github.com/dlmanning/gulp-sass)) (gulp-ruby-sass)
+ 编译Less([gulp-less](https://github.com/plus3network/gulp-less))
+ Autoprefixer (gulp-autoprefixer)
+ 缩小化(minify)CSS (gulp-minify-css)
+ JSHint (gulp-jshint)
+ 拼接 (gulp-concat)
+ 丑化(Uglify) (gulp-uglify)
+ 图片压缩 (gulp-imagemin)
+ html压缩 ([gulp-htmlmin](https://github.com/jonschlinkert/gulp-htmlmin))
+ 即时重整(LiveReload) (gulp-livereload)
+ 清理档案 (gulp-clean)
+ 图片快取，只有更改过得图片会进行压缩 (gulp-cache)
+ 更动通知 (gulp-notify)
+ sourcemaps([gulp-sourcemaps](https://www.npmjs.com/package/gulp-sourcemaps))

~~~sql
$ npm install gulp-ruby-sass gulp-sass gulp-htmlmin gulp-autoprefixer gulp-minify-css gulp-jshint gulp-concat gulp-uglify gulp-imagemin gulp-clean gulp-notify gulp-rename gulp-livereload gulp-cache --save-dev
~~~

## 载入插件
建立一个gulpfile.js档案，并且载入这些插件：

~~~javascript
var gulp = require('gulp'),  
    sass = require('gulp-ruby-sass'),
    autoprefixer = require('gulp-autoprefixer'),
    minifycss = require('gulp-minify-css'),
    jshint = require('gulp-jshint'),
    uglify = require('gulp-uglify'),
    imagemin = require('gulp-imagemin'),
    rename = require('gulp-rename'),
    clean = require('gulp-clean'),
    concat = require('gulp-concat'),
    notify = require('gulp-notify'),
    cache = require('gulp-cache'),
    livereload = require('gulp-livereload');
~~~

## 建立任务
任务的最基本形态，在gulpfile.js文件中

~~~javascript
var gulp = require('gulp');

gulp.task('default', function() {
  // 将你的默认的任务代码放在这
});
~~~

可以通过如下命令来执行任务：
~~~sql
$ gulp default
~~~

设置编译Sass。我们将编译Sass、接著通过Autoprefixer，最后储存结果到我们的目的地。
接著产生一个缩小化的.min版本、自动重新整理页面及通知任务已经完成：

~~~javascript
gulp.task('styles', function() {  
  return gulp.src('src/styles/main.scss')
    .pipe(sass({ style: 'expanded' }))
    .pipe(autoprefixer('last 2 version', 'safari 5', 'ie 8', 'ie 9', 'opera 12.1', 'ios 6', 'android 4'))
    .pipe(gulp.dest('dist/assets/css'))
    .pipe(rename({suffix: '.min'}))
    .pipe(minifycss())
    .pipe(gulp.dest('dist/assets/css'))
    .pipe(notify({ message: 'Styles task complete' }));
});
~~~

## gulp基本API

~~~javascript
gulp.src(globs[, options])
gulp.dest(path[, options])
gulp.task(name[, deps], fn)
gulp.watch(glob [, opts], tasks)
~~~

`gulp.src(globs[, options])`

输出（Emits）符合所提供的匹配模式（glob）或者匹配模式的数组（array of globs）的文件。 将返回一个 Vinyl files 的 stream 它可以被 piped 到别的插件中。

`gulp.dest(path[, options])`

能被 pipe 进来，并且将会写文件。并且重新输出（emits）所有数据，因此你可以将它 pipe 到多个文件夹。如果某文件夹不存在，将会自动创建它。

~~~javascript
gulp.src('./client/templates/*.jade')
  .pipe(jade())
  .pipe(gulp.dest('./build/templates'))
  .pipe(minify())
  .pipe(gulp.dest('./build/minified_templates'));
~~~

文件被写入的路径是以所给的相对路径根据所给的目标目录计算而来。类似的，相对路径也可以根据所给的 base 来计算。 请查看上述的 gulp.src 来了解更多信息。

gulp.task(name[, deps], fn)
gulp.watch(glob [, opts], tasks) 
或 gulp.watch(glob [, opts, cb])

监视文件，并且可以在文件发生改动时候做一些事情。它总会返回一个 EventEmitter 来发射（emit） change 事件。
例如：需要在文件变动后执行的一个或者多个通过 gulp.task() 创建的 task 的名字

~~~javascript
var watcher = gulp.watch('js/**/*.js', ['uglify','reload']);
watcher.on('change', function(event) {
  console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
});
gulp.watch('js/**/*.js', function(event) {
  console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
});
~~~

## 使用例子：

### 编译Sass，Autoprefix及缩小化

~~~javascript
gulp.task('styles', function() {  
return gulp.src('src/styles/main.scss')
 .pipe(sass({ style: 'expanded' }))
 .pipe(autoprefixer('last 2 version', 'safari 5', 'ie 8', 'ie 9', 'opera 12.1', 'ios 6', 'android 4'))
 .pipe(gulp.dest('dist/assets/css'))
 .pipe(rename({suffix: '.min'}))
 .pipe(minifycss())
 .pipe(gulp.dest('dist/assets/css'))
 .pipe(notify({ message: 'Styles task complete' }));
});
~~~

### JSHint，拼接及缩小化JavaScript

~~~javascript
gulp.task('scripts', function() {  
return gulp.src('src/scripts/**/*.js')
 .pipe(jshint('.jshintrc'))
 .pipe(jshint.reporter('default'))
 .pipe(concat('main.js'))
 .pipe(gulp.dest('dist/assets/js'))
 .pipe(rename({suffix: '.min'}))
 .pipe(uglify())
 .pipe(gulp.dest('dist/assets/js'))
 .pipe(notify({ message: 'Scripts task complete' }));
});
~~~

### 图片压缩
~~~javascript
gulp.task('images', function() {  
return gulp.src('src/images/**/*')
 .pipe(imagemin({ optimizationLevel: 3, progressive: true, interlaced: true }))
 .pipe(gulp.dest('dist/assets/img'))
 .pipe(notify({ message: 'Images task complete' }));
});
~~~
### 收拾乾淨!

佈署之前，清除目的地目录并重建档案是一个好主意–以防万一任何已经被删除的来源档案遗留在目的地目录之中:
可以传入一个目录(或档案)阵列到gulp.src。因为不需要读取已经被删除的档案，加入`read: false`选项来防止gulp读取档案内容–让它快一些。

~~~javascript
gulp.task('clean', function() {  
return gulp.src(['dist/assets/css', 'dist/assets/js', 'dist/assets/img'], {read: false})
 .pipe(clean());
});
~~~

### 预设任务

建立一个预设任务，当只输入$ gulp指令时执行的任务，这裡执行三个我们所建立的任务:
注意额外传入gulp.task的阵列。这裡我们可以定义任务相依(task dependencies)。在这个范例中，gulp.start开始任务前会先执行清理(clean)任务。Gulp中所有的任务都是并行(concurrently)执行，并没有先后顺序哪个任务会先完成，所以我们需要确保clean任务在其他任务开始前完成。

~~~javascript
gulp.task('default', ['clean'], function() {  
 gulp.start('styles', 'scripts', 'images');
});
~~~

### 看守

为了能够看守档案，并在更动发生后执行相关任务，首先需要建立一个新的任务，使用`gulp.watch` API来看守档案:

~~~javascript
gulp.task('watch', function() {
 // 看守所有.scss档
 gulp.watch('src/styles/**/*.scss', ['styles']);
 // 看守所有.js档
 gulp.watch('src/scripts/**/*.js', ['scripts']);
 // 看守所有图片档
 gulp.watch('src/images/**/*', ['images']);
});
~~~

### 即时重整(LiveReload)

Gulp也可以处理档案更动后，自动重新整理页面。我们需要修改watch任务，设置即时重整伺服器。

~~~javascript
// 载入外挂
var gulp = require('gulp'),  
 sass = require('gulp-ruby-sass'),
 autoprefixer = require('gulp-autoprefixer'),
 minifycss = require('gulp-minify-css'),
 jshint = require('gulp-jshint'),
 uglify = require('gulp-uglify'),
 imagemin = require('gulp-imagemin'),
 rename = require('gulp-rename'),
 clean = require('gulp-clean'),
 concat = require('gulp-concat'),
 notify = require('gulp-notify'),
 cache = require('gulp-cache'),
 livereload = require('gulp-livereload');
// 样式
gulp.task('styles', function() {  
return gulp.src('src/styles/main.scss')
 .pipe(sass({ style: 'expanded', }))
 .pipe(autoprefixer('last 2 version', 'safari 5', 'ie 8', 'ie 9', 'opera 12.1', 'ios 6', 'android 4'))
 .pipe(gulp.dest('dist/styles'))
 .pipe(rename({ suffix: '.min' }))
 .pipe(minifycss())
 .pipe(gulp.dest('dist/styles'))
 .pipe(notify({ message: 'Styles task complete' }));
});
// 脚本
gulp.task('scripts', function() {  
return gulp.src('src/scripts/**/*.js')
 .pipe(jshint('.jshintrc'))
 .pipe(jshint.reporter('default'))
 .pipe(concat('main.js'))
 .pipe(gulp.dest('dist/scripts'))
 .pipe(rename({ suffix: '.min' }))
 .pipe(uglify())
 .pipe(gulp.dest('dist/scripts'))
 .pipe(notify({ message: 'Scripts task complete' }));
});
// 图片
gulp.task('images', function() {  
return gulp.src('src/images/**/*')
 .pipe(cache(imagemin({ optimizationLevel: 3, progressive: true, interlaced: true })))
 .pipe(gulp.dest('dist/images'))
 .pipe(notify({ message: 'Images task complete' }));
});
// 清理
gulp.task('clean', function() {  
return gulp.src(['dist/styles', 'dist/scripts', 'dist/images'], {read: false})
 .pipe(clean());
});
// 预设任务
gulp.task('default', ['clean'], function() {  
 gulp.start('styles', 'scripts', 'images');
});
// 看手
gulp.task('watch', function() {
// 看守所有.scss档
gulp.watch('src/styles/**/*.scss', ['styles']);
// 看守所有.js档
gulp.watch('src/scripts/**/*.js', ['scripts']);
// 看守所有图片档
gulp.watch('src/images/**/*', ['images']);
// 建立即时重整伺服器
var server = livereload();
// 看守所有位在 dist/  目录下的档案，一旦有更动，便进行重整
gulp.watch(['dist/**']).on('change', function(file) {
 server.changed(file.path);
});
});
~~~

# 常见错误

创建 gulp.task 方法时，一定要有`return`,否则会造成工作流顺序不正确

# gulp-platform-template

[使用gulp构建项目，实现项目工程化](https://github.com/huang-qing/gulp-platform-template)

## gulp plugins

+ [stream-series](https://www.npmjs.com/package/stream-series) : Waterfalls for streams

+ [merge-stream](https://www.npmjs.com/package/merge-stream) : erge (interleave) a bunch of streams.

+ [gulp-load-plugins](https://www.npmjs.com/package/gulp-load-plugins) : Automatically load any gulp plugins in your package.json

+ [gulp-sequence](https://www.npmjs.com/package/gulp-sequence/): Run a series of gulp tasks in order.

+ [gulp-eslint](https://www.npmjs.com/package/gulp-eslint/): A gulp plugin for processing files with ESLint

+ [gulp-htmlmin](https://www.npmjs.com/package/gulp-htmlmin/) : gulp plugin to minify HTML.

+ [gulp-clean](https://www.npmjs.com/package/gulp-clean): A gulp plugin for removing files and folders.

+ [gulp-autoprefixer](https://github.com/sindresorhus/gulp-autoprefixer) : Prefix CSS with Autoprefixer

+ [gulp-sass](https://www.npmjs.com/package/gulp-sass) :  Gulp plugin for sass

+ **not support sourcemaps! fuck!**  ~~[gulp-clean-css](https://www.npmjs.com/package/gulp-clean-css) : Minify css with clean-css.~~
+ [gulp-csso](https://www.npmjs.com/package/gulp-csso) : Minify CSS with CSSO.

+ [gulp-uglify](https://www.npmjs.com/package/gulp-uglify) : Minify files with UglifyJS.

+ [gulp-sourcemaps](https://www.npmjs.com/package/gulp-sourcemaps) : Source map support for Gulp.js .[Plugins with gulp sourcemaps support](https://github.com/floridoo/gulp-sourcemaps/wiki/Plugins-with-gulp-sourcemaps-support)

+ [gulp-inject](https://www.npmjs.com/package/gulp-inject)
A javascript, stylesheet and webcomponent injection plugin for Gulp, i.e. inject file references into your index.html

+ [gulp-rename](https://www.npmjs.com/package/gulp-rename) : gulp-rename is a gulp plugin to rename files easily.

+ [gulp-concat](https://www.npmjs.com/package/gulp-concat) : Concatenates files

+ [gulp-csslint](https://www.npmjs.com/package/gulp-csslint/) : CSSLint plugin for gulp

+ [gulp-plumber](https://www.npmjs.com/package/gulp-plumber) : Prevent pipe breaking caused by errors from gulp plugins

+ [gulp-filter](https://www.npmjs.com/package/gulp-filter) : Filter files in a Vinyl stream

+ [gulp.spritesmith](https://www.npmjs.com/package/gulp.spritesmith) :Convert a set of images into a spritesheet and CSS variables via gulp

+ [gulp-imagemin](https://www.npmjs.com/package/gulp-imagemin) Minify PNG, JPEG, GIF and SVG images with imagemin

+ [gulp-iconfont](https://www.npmjs.com/package/gulp-iconfont) : Create SVG/TTF/EOT/WOFF/WOFF2 fonts from several SVG icons with Gulp.

+ [gulp-svg-sprite](https://github.com/jkphl/gulp-svg-sprite) : SVG sprites & stacks galore — Gulp plugin wrapping around svg-sprite that reads in a bunch of SVG files, optimizes them and creates SVG sprites and CSS resources in various flavours

+ [gulp-svgstore](https://www.npmjs.com/package/gulp-svgstore) Combine svg files into one with <symbol> elements.

+ [gulp-svgmin](https://www.npmjs.com/package/gulp-svgmin) Minify SVG with SVGO.

+ [gulp-iconfont-css](https://www.npmjs.com/package/gulp-iconfont-css) Generate (S)CSS file for icon font created with Gulp


## 重点使用说明

### 雪碧图：

关于png、iconfont、svg的对比[可参考这篇文章](https://github.com/yalishizhude/sprite-demo)

[gulp.spritesmith](https://www.npmjs.com/package/gulp.spritesmith)用于制作png雪碧图

[gulp-iconfont](https://www.npmjs.com/package/gulp-iconfont)用于制作字体图标

[gulp-svg-sprites](https://www.npmjs.com/package/gulp-svg-sprites)用于制作svg图标


[svg4everybody](https://github.com/jonathantneal/svg4everybody) 这个shim解决所有的IE浏览器(包括IE11)不支持获得外链SVG文件某个元件



## 执行debug模式下的项目创建

~~~
gulp build-debug
~~~