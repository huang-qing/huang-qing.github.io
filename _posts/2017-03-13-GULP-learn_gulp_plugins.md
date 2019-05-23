---
layout:     post
title:      Glup Plugings
subtitle:   学习使用 Glup 插件
date:       2017-03-13
author:     huangqing
header-img: img/post-bg-gulp.png
catalog: true
categories: [Gulp]
tags:
    - gulp
---


## [gulp-load-plugins](https://www.npmjs.com/package/gulp-load-plugins) 

Automatically load any gulp plugins in your package.json

Install

~~~javascript
$ npm install --save-dev gulp-load-plugins
~~~

Given a package.json file that has some dependencies within:

~~~json
{
    "dependencies": {
        "gulp-jshint": "*",
        "gulp-concat": "*"
    }
}
~~~

Adding this into your Gulpfile.js:

~~~javascript
var gulp = require('gulp');
var plugins = require('gulp-load-plugins')();
~~~

Will result in the following happening (roughly, plugins are lazy loaded but in practice you won't notice any difference):

~~~javascript
plugins.jshint = require('gulp-jshint');
plugins.concat = require('gulp-concat');
~~~

## [gulp-sequence](https://www.npmjs.com/package/gulp-sequence/)

 Run a series of gulp tasks in order.

Install

~~~javascript
npm install --save-dev gulp-sequence
~~~

Usage

~~~javascript
var gulp = require('gulp')
var gulpSequence = require('gulp-sequence')
 
gulp.task('a', function (cb) {
  //... cb() 
})
 
gulp.task('b', function (cb) {
  //... cb() 
})
 
gulp.task('c', function (cb) {
  //... cb() 
})
 
gulp.task('d', function (cb) {
  //... cb() 
})
 
gulp.task('e', function (cb) {
  //... cb() 
})
 
gulp.task('f', function () {
  // return stream 
  return gulp.src('*.js')
})
 
// usage 1, recommend 
// 1. run 'a', 'b' in parallel; 
// 2. run 'c' after 'a' and 'b'; 
// 3. run 'd', 'e' in parallel after 'c'; 
// 3. run 'f' after 'd' and 'e'. 
gulp.task('sequence-1', gulpSequence(['a', 'b'], 'c', ['d', 'e'], 'f'))
 
// usage 2 
gulp.task('sequence-2', function (cb) {
  gulpSequence(['a', 'b'], 'c', ['d', 'e'], 'f', cb)
})
 
// usage 3 
gulp.task('sequence-3', function (cb) {
  gulpSequence(['a', 'b'], 'c', ['d', 'e'], 'f')(cb)
})
 
gulp.task('gulp-sequence', gulpSequence('sequence-1', 'sequence-2', 'sequence-3'))
~~~

with `gulp.watch`:

~~~javascript
gulp.watch('src/**/*.js', function (event) {
  gulpSequence('a', 'b')(function (err) {
    if (err) console.log(err)
  })
})
~~~


## [gulp-eslint](https://www.npmjs.com/package/gulp-eslint/)

A gulp plugin for processing files with ESLint

Installation
Use npm.

~~~javascript
npm install gulp-eslint
~~~

Usage

~~~javascript
const gulp = require('gulp');
const eslint = require('gulp-eslint');
 
gulp.task('lint', () => {
    // ESLint ignores files with "node_modules" paths. 
    // So, it's best to have gulp ignore the directory as well. 
    // Also, Be sure to return the stream from the task; 
    // Otherwise, the task may end before the stream has finished. 
    return gulp.src(['**/*.js','!node_modules/**'])
        // eslint() attaches the lint output to the "eslint" property 
        // of the file object so it can be used by other modules. 
        .pipe(eslint())
        // eslint.format() outputs the lint results to the console. 
        // Alternatively use eslint.formatEach() (see Docs). 
        .pipe(eslint.format())
        // To have the process exit with an error code (1) on 
        // lint error, return the stream and pipe to failAfterError last. 
        .pipe(eslint.failAfterError());
});
 
gulp.task('default', ['lint'], function () {
    // This will only run if the lint task is successful... 
});

~~~

Or use the plugin API to do things like:

~~~javascript

gulp.src(['**/*.js','!node_modules/**'])
    .pipe(eslint({
        rules: {
            'my-custom-rule': 1,
            'strict': 2
        },
        globals: [
            'jQuery',
            '$'
        ],
        envs: [
            'browser'
        ]
    }))
    .pipe(eslint.formatEach('compact', process.stderr));

~~~

## [gulp-htmlmin](https://www.npmjs.com/package/gulp-htmlmin/) 

gulp plugin to minify HTML.

Install with npm

~~~javascript
npm i gulp-htmlmin --save-dev
~~~

Usage

~~~javascript
var gulp = require('gulp');
var htmlmin = require('gulp-htmlmin');
 
gulp.task('minify', function() {
  return gulp.src('src/*.html')
    .pipe(htmlmin({collapseWhitespace: true}))
    .pipe(gulp.dest('dist'));
});
~~~
----

## [gulp-clean](https://www.npmjs.com/package/gulp-clean)

A gulp plugin for removing files and folders.

Install
Install with npm.

~~~javascript
npm install --save-dev gulp-clean
~~~

Examples

~~~javascript
var gulp = require('gulp');
var clean = require('gulp-clean');
 
gulp.task('default', function () {
    return gulp.src('app/tmp', {read: false})
        .pipe(clean());
});
~~~

----

Install

~~~javascript
$ npm install --save-dev gulp-autoprefixer
~~~

Usage

~~~javascript
const gulp = require('gulp');
const autoprefixer = require('gulp-autoprefixer');

gulp.task('default', () =>
    gulp.src('src/app.css')
        .pipe(autoprefixer({
            browsers: ['last 2 versions'],
            cascade: false
        }))
        .pipe(gulp.dest('dist'))
);
~~~

API

`autoprefixer([options])`

options

See the Autoprefixer [options](https://github.com/postcss/autoprefixer#options).

Source Maps

Use gulp-sourcemaps like this:

~~~javascript
const gulp = require('gulp');
const sourcemaps = require('gulp-sourcemaps');
const autoprefixer = require('gulp-autoprefixer');
const concat = require('gulp-concat');

gulp.task('default', () =>
    gulp.src('src/**/*.css')
        .pipe(sourcemaps.init())
        .pipe(autoprefixer())
        .pipe(concat('all.css'))
        .pipe(sourcemaps.write('.'))
        .pipe(gulp.dest('dist'))
);
~~~
----

## [gulp-sass](https://www.npmjs.com/package/gulp-sass) 

 Gulp plugin for sass

Install

~~~javascript
npm install gulp-sass --save-dev
~~~

Basic Usage

Something like this will compile your Sass files:

~~~javascript
'use strict';
 
var gulp = require('gulp');
var sass = require('gulp-sass');
 
gulp.task('sass', function () {
  return gulp.src('./sass/**/*.scss')
    .pipe(sass().on('error', sass.logError))
    .pipe(gulp.dest('./css'));
});
 
gulp.task('sass:watch', function () {
  gulp.watch('./sass/**/*.scss', ['sass']);
});
~~~

You can also compile synchronously, doing something like this:

~~~javascript
'use strict';
 
var gulp = require('gulp');
var sass = require('gulp-sass');
 
gulp.task('sass', function () {
  return gulp.src('./sass/**/*.scss')
    .pipe(sass.sync().on('error', sass.logError))
    .pipe(gulp.dest('./css'));
});
 
gulp.task('sass:watch', function () {
  gulp.watch('./sass/**/*.scss', ['sass']);
});
~~~

Options

Pass in options just like you would for node-sass; they will be passed along just as if you were using node-sass. Except for the data option which is used by gulp-sass internally. Using the file option is also unsupported and results in undefined behaviour that may change without notice.

For example:

~~~javascript
gulp.task('sass', function () {
 return gulp.src('./sass/**/*.scss')
   .pipe(sass({outputStyle: 'compressed'}).on('error', sass.logError))
   .pipe(gulp.dest('./css'));
});
~~~

Source Maps

gulp-sass can be used in tandem with gulp-sourcemaps to generate source maps for the Sass to CSS compilation. You will need to initialize gulp-sourcemaps prior to running gulp-sass and write the source maps after.

~~~javascript
var sourcemaps = require('gulp-sourcemaps');
 
gulp.task('sass', function () {
 return gulp.src('./sass/**/*.scss')
  .pipe(sourcemaps.init())
  .pipe(sass().on('error', sass.logError))
  .pipe(sourcemaps.write())
  .pipe(gulp.dest('./css'));
});
~~~

By default, gulp-sourcemaps writes the source maps inline in the compiled CSS files. To write them to a separate file, specify a path relative to the gulp.dest() destination in the sourcemaps.write() function.

~~~javascript
var sourcemaps = require('gulp-sourcemaps');
gulp.task('sass', function () {
 return gulp.src('./sass/**/*.scss')
  .pipe(sourcemaps.init())
  .pipe(sass().on('error', sass.logError))
  .pipe(sourcemaps.write('./maps'))
  .pipe(gulp.dest('./css'));
});
~~~

----

## [gulp-if](https://www.npmjs.com/package/gulp-if) 

Conditionally run a task

A ternary gulp plugin: conditionally control the flow of vinyl objects.

Note: Badly behaved plugins can often get worse when used with gulp-if. Typically the fix is not in gulp-if.

Note: Works great with lazypipe, see below

Usage

1: Conditionally filter content

~~~javascript
var gulpif = require('gulp-if');
var uglify = require('gulp-uglify');
 
var condition = true; // TODO: add business logic 
 
gulp.task('task', function() {
  gulp.src('./src/*.js')
    .pipe(gulpif(condition, uglify()))
    .pipe(gulp.dest('./dist/'));
});
~~~

Only uglify the content if the condition is true, but send all the files to the dist folder

2: Ternary filter

~~~javascript
var gulpif = require('gulp-if');
var uglify = require('gulp-uglify');
var beautify = require('gulp-beautify');
 
var condition = function (file) {
  // TODO: add business logic 
  return true;
}
 
gulp.task('task', function() {
  gulp.src('./src/*.js')
    .pipe(gulpif(condition, uglify(), beautify()))
    .pipe(gulp.dest('./dist/'));
});
~~~

If condition returns true, uglify else beautify, then send everything to the dist folder

3: Remove things from the stream

~~~javascript
var gulpIgnore = require('gulp-ignore');
var uglify = require('gulp-uglify');
var jshint = require('gulp-jshint');
 
var condition = './gulpfile.js';
 
gulp.task('task', function() {
  gulp.src('./*.js')
    .pipe(jshint())
    .pipe(gulpIgnore.exclude(condition))
    .pipe(uglify())
    .pipe(gulp.dest('./dist/'));
});
~~~

Run JSHint on everything, remove gulpfile from the stream, then uglify and write everything else.

4: Exclude things from the stream

~~~javascript
var uglify = require('gulp-uglify');
 
gulp.task('task', function() {
  gulp.src(['./*.js', '!./node_modules/**'])
    .pipe(uglify())
    .pipe(gulp.dest('./dist/'));
});
~~~

Grab all JavaScript files that aren't in the node_modules folder, uglify them, and write them. This is fastest because nothing in node_modules ever leaves gulp.src()

works great with lazypipe

Lazypipe creates a function that initializes the pipe chain on use. This allows you to create a chain of events inside the gulp-if condition. This scenario will run jshint analysis and reporter only if the linting flag is true.

~~~javascript
var gulpif = require('gulp-if');
var jshint = require('gulp-jshint');
var uglify = require('gulp-uglify');
var lazypipe = require('lazypipe');
 
var linting = false;
var compressing = false;
 
var jshintChannel = lazypipe()
  // adding a pipeline step 
  .pipe(jshint) // notice the stream function has not been called! 
  .pipe(jshint.reporter)
  // adding a step with an argument 
  .pipe(jshint.reporter, 'fail');
 
gulp.task('scripts', function () {
  return gulp.src(paths.scripts.src)
    .pipe(gulpif(linting, jshintChannel()))
    .pipe(gulpif(compressing, uglify()))
    .pipe(gulp.dest(paths.scripts.dest));
});
~~~

works great inside lazypipe

Lazypipe assumes that all function parameters are static, gulp-if arguments need to be instantiated inside each lazypipe. This difference can be easily solved by passing a function on the lazypipe step

~~~javascript
var gulpif = require('gulp-if');
var jshint = require('gulp-jshint');
var uglify = require('gulp-uglify');
var lazypipe = require('lazypipe');
 
var compressing = false;
 
var jsChannel = lazypipe()
  // adding a pipeline step 
  .pipe(jshint) // notice the stream function has not been called! 
  .pipe(jshint.reporter)
  // adding a step with an argument 
  .pipe(jshint.reporter, 'fail')
  // you can't say: .pipe(gulpif, compressing, uglify) 
  // because uglify needs to be instantiated separately in each lazypipe instance 
  // you can say this instead: 
  .pipe(function () {
    return gulpif(compressing, uglify());
  });
  // why does this work? lazypipe calls the function, passing in the no arguments to it, 
  // it instantiates a new gulp-if pipe and returns it to lazypipe. 
 
gulp.task('scripts', function () {
  return gulp.src(paths.scripts.src)
    .pipe(jsChannel())
    .pipe(gulp.dest(paths.scripts.dest));
});
~~~
----

## [gulp-clean-css](https://www.npmjs.com/package/gulp-clean-css) 

 Minify css with clean-css.

Regarding Issues

This is just a simple gulp plugin, which means it's nothing more than a thin wrapper around clean-css. If it looks like you are having CSS related issues, please contact clean-css. Only create a new issue if it looks like you're having a problem with the plugin itself.

Install

~~~javascript
npm install gulp-clean-css --save-dev
~~~

API

`cleanCSS([options], [callback])`

options

See the CleanCSS options.

~~~javascript
var gulp = require('gulp');
var cleanCSS = require('gulp-clean-css');
 
gulp.task('minify-css', function() {
  return gulp.src('styles/*.css')
    .pipe(cleanCSS({compatibility: 'ie8'}))
    .pipe(gulp.dest('dist'));
});
~~~

callback

Useful for returning details from the underlying minify() call. An example use case could include logging stats of the minified file. In addition to the default object, gulp-clean-css provides the file name and path for further analysis.

~~~javascript
var gulp = require('gulp');
var cleanCSS = require('gulp-clean-css');
 
gulp.task('minify-css', function() {
    return gulp.src('styles/*.css')
        .pipe(cleanCSS({debug: true}, function(details) {
            console.log(details.name + ': ' + details.stats.originalSize);
            console.log(details.name + ': ' + details.stats.minifiedSize);
        }))
        .pipe(gulp.dest('dist'));
});
~~~

Source Maps can be generated by using gulp-sourcemaps.

~~~javascript
var gulp = require('gulp');
var cleanCSS = require('gulp-clean-css');
var sourcemaps = require('gulp-sourcemaps');
 
gulp.task('minify-css', function() {
    return gulp.src('./src/*.css')
        .pipe(sourcemaps.init())
        .pipe(cleanCSS())
        .pipe(sourcemaps.write())
        .pipe(gulp.dest('dist'));
    });
});
~~~

## [gulp-inject](https://www.npmjs.com/package/gulp-inject)
A javascript, stylesheet and webcomponent injection plugin for Gulp, i.e. inject file references into your index.html

Installation

Install gulp-inject as a development dependency:

~~~javascript
npm install --save-dev gulp-inject
~~~

Basic usage

The target file src/index.html:

Each pair of comments are the injection placeholders (aka. tags, see options.starttag and options.endtag).

~~~html
<!DOCTYPE html>
<html>
<head>
  <title>My index</title>
  <!-- inject:css -->
  <!-- endinject -->
</head>
<body>
 
  <!-- inject:js -->
  <!-- endinject -->
</body>
</html>
~~~

The gulpfile.js:

~~~javascript
var gulp = require('gulp');
var inject = require('gulp-inject');
 
gulp.task('index', function () {
  var target = gulp.src('./src/index.html');
  // It's not necessary to read the files (will speed up things), we're only after their paths: 
  var sources = gulp.src(['./src/**/*.js', './src/**/*.css'], {read: false});
 
  return target.pipe(inject(sources))
    .pipe(gulp.dest('./src'));
});
~~~

src/index.html after running gulp index:

~~~html
<!DOCTYPE html>
<html>
<head>
  <title>My index</title>
  <!-- inject:css -->
  <link rel="stylesheet" href="/src/style1.css">
  <link rel="stylesheet" href="/src/style2.css">
  <!-- endinject -->
</head>
<body>
 
  <!-- inject:js -->
  <script src="/src/lib1.js"></script> 
  <script src="/src/lib2.js"></script> 
  <!-- endinject -->
</body>
</html>
~~~

## [gulp-uglify](https://www.npmjs.com/package/gulp-uglify) 

 Minify files with UglifyJS.

Installation

Install package with NPM and add it to your development dependencies:

~~~javascript
npm install --save-dev gulp-uglify
~~~

Usage

~~~javascript
var gulp = require('gulp');
var uglify = require('gulp-uglify');
var pump = require('pump');
 
gulp.task('compress', function (cb) {
  pump([
        gulp.src('lib/*.js'),
        uglify(),
        gulp.dest('dist')
    ],
    cb
  );
});
~~~
----

## [gulp-sourcemaps](https://www.npmjs.com/package/gulp-sourcemaps) 

Source map support for Gulp.js

Usage

Write inline source maps
Inline source maps are embedded in the source file.

Example:

~~~javascript
var gulp = require('gulp');
var plugin1 = require('gulp-plugin1');
var plugin2 = require('gulp-plugin2');
var sourcemaps = require('gulp-sourcemaps');
 
gulp.task('javascript', function() {
  gulp.src('src/**/*.js')
    .pipe(sourcemaps.init())
      .pipe(plugin1())
      .pipe(plugin2())
    .pipe(sourcemaps.write())
    .pipe(gulp.dest('dist'));
});
~~~

All plugins between sourcemaps.init() and sourcemaps.write() need to have support for gulp-sourcemaps. You can find a list of such plugins in the wiki.

Write external source map files

To write external source map files, pass a path relative to the destination to sourcemaps.write().

Example:

~~~javascript
var gulp = require('gulp');
var plugin1 = require('gulp-plugin1');
var plugin2 = require('gulp-plugin2');
var sourcemaps = require('gulp-sourcemaps');
 
gulp.task('javascript', function() {
  gulp.src('src/**/*.js')
    .pipe(sourcemaps.init())
      .pipe(plugin1())
      .pipe(plugin2())
    .pipe(sourcemaps.write('../maps'))
    .pipe(gulp.dest('dist'));
});
~~~

Load existing source maps

To load existing source maps, pass the option loadMaps: true to sourcemaps.init().

Example:

~~~javascript
var gulp = require('gulp');
var plugin1 = require('gulp-plugin1');
var plugin2 = require('gulp-plugin2');
var sourcemaps = require('gulp-sourcemaps');
 
gulp.task('javascript', function() {
  gulp.src('src/**/*.js')
    .pipe(sourcemaps.init({loadMaps: true}))
      .pipe(plugin1())
      .pipe(plugin2())
    .pipe(sourcemaps.write())
    .pipe(gulp.dest('dist'));
});
~~~

Handle large files

To handle large files, pass the option largeFile: true to sourcemaps.init().

Example:

~~~javascript
var gulp = require('gulp');
var plugin1 = require('gulp-plugin1');
var plugin2 = require('gulp-plugin2');
var sourcemaps = require('gulp-sourcemaps');
 
gulp.task('javascript', function() {
  gulp.src('src/**/*.js')
    .pipe(sourcemaps.init({largeFile: true}))
      .pipe(plugin1())
      .pipe(plugin2())
    .pipe(sourcemaps.write())
    .pipe(gulp.dest('dist'));
});
~~~
----

## [gulp-rename](https://www.npmjs.com/package/gulp-rename) 


gulp-rename is a gulp plugin to rename files easily.

NPM

build status devDependency Status

Usage

gulp-rename provides simple file renaming methods.

~~~javascript

var rename = require("gulp-rename");
 
// rename via string 
gulp.src("./src/main/text/hello.txt")
  .pipe(rename("main/text/ciao/goodbye.md"))
  .pipe(gulp.dest("./dist")); // ./dist/main/text/ciao/goodbye.md 
 
// rename via function 
gulp.src("./src/**/hello.txt")
  .pipe(rename(function (path) {
    path.dirname += "/ciao";
    path.basename += "-goodbye";
    path.extname = ".md"
  }))
  .pipe(gulp.dest("./dist")); // ./dist/main/text/ciao/hello-goodbye.md 
 
// rename via hash 
gulp.src("./src/main/text/hello.txt", { base: process.cwd() })
  .pipe(rename({
    dirname: "main/text/ciao",
    basename: "aloha",
    prefix: "bonjour-",
    suffix: "-hola",
    extname: ".md"
  }))
  .pipe(gulp.dest("./dist")); // ./dist/main/text/ciao/bonjour-aloha-hola.md 
~~~


See test/rename.spec.js for more examples and test/path-parsing.spec.js for hairy details.

----

## [gulp-concat](https://www.npmjs.com/package/gulp-concat) 

 Concatenates files

Installation

Install package with NPM and add it to your development dependencies:

~~~javascript
npm install --save-dev gulp-concat
~~~

Usage

~~~javascript
var concat = require('gulp-concat');
 
gulp.task('scripts', function() {
  return gulp.src('./lib/*.js')
    .pipe(concat('all.js'))
    .pipe(gulp.dest('./dist/'));
});
~~~

This will concat files by your operating systems newLine. It will take the base directory from the first file that passes through it.

Files will be concatenated in the order that they are specified in the gulp.src function. For example, to concat ./lib/file3.js, ./lib/file1.js and ./lib/file2.js in that order, the following code will create a task to do that:

~~~javascript
var concat = require('gulp-concat');
 
gulp.task('scripts', function() {
  return gulp.src(['./lib/file3.js', './lib/file1.js', './lib/file2.js'])
    .pipe(concat('all.js'))
    .pipe(gulp.dest('./dist/'));
});
~~~

To change the newLine simply pass an object as the second argument to concat with newLine being whatever (\r\n if you want to support any OS to look at it)

For instance:

~~~javascript
.pipe(concat('main.js', {newLine: ';'}))
~~~

To specify cwd, path and other vinyl properties, gulp-concat accepts Object as first argument:

~~~javascript
var concat = require('gulp-concat');
 
gulp.task('scripts', function() {
  return gulp.src(['./lib/file3.js', './lib/file1.js', './lib/file2.js'])
    .pipe(concat({ path: 'new.js', stat: { mode: 0666 }}))
    .pipe(gulp.dest('./dist'));
});
~~~

This will concat files into ./dist/new.js.

Source maps

Source maps can be generated by using gulp-sourcemaps:

~~~javascript
var gulp = require('gulp');
var concat = require('gulp-concat');
var sourcemaps = require('gulp-sourcemaps');
 
gulp.task('javascript', function() {
  return gulp.src('src/**/*.js')
    .pipe(sourcemaps.init())
      .pipe(concat('all.js'))
    .pipe(sourcemaps.write())
    .pipe(gulp.dest('dist'));
});
~~~
----

## [gulp-htmlmin](https://www.npmjs.com/package/gulp-htmlmin) 

gulp plugin to minify HTML.

Install with npm

~~~javascript
npm i gulp-htmlmin --save-dev
~~~

Usage

~~~javascript
var gulp = require('gulp');
var htmlmin = require('gulp-htmlmin');
 
gulp.task('minify', function() {
  return gulp.src('src/*.html')
    .pipe(htmlmin({collapseWhitespace: true}))
    .pipe(gulp.dest('dist'));
});
~~~
----

##［gulp-csslint](https://www.npmjs.com/package/gulp-csslint/) 

CSSLint plugin for gulp

Usage

First, install `gulp-csslint` as a development dependency:

~~~javascript
npm install --save-dev gulp-csslint
~~~

Then, add it to your gulpfile.js:

~~~javascript
var csslint = require('gulp-csslint');
 
gulp.task('css', function() {
  gulp.src('client/css/*.css')
    .pipe(csslint())
    .pipe(csslint.formatter());
});
~~~