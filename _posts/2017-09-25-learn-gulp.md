---
layout: post
title:  "Gulp入门"
categories: Tools
date:   2017-09-25 20:48:05
author: Logan
tags:  gulp
---

* content
{:toc}

**安装nodejs -> 全局安装gulp -> 项目安装gulp以及gulp插件 -> 配置gulpfile.js -> 运行任务**

# 全局安装Gulp

`$ npm install gulp -g`

# 目录结构

└── src/            源码目录
	├── build/   .html 组件
	├── sass/     .scss .sass 文件
	├── css/       .css 文件
	├── js/         .js 文件
	└── img/     图片
└── dist/        输出目录
└── package.json
└── gulpfile.js

# 关于package.json

可以在项目上使用 npm init 配置，推荐直接新建并写入初始内容：

package.json
>{<br>
>  "name": "gulp-build",<br>
>  "version": "1.0.0",<br>
>  "description": "Gulp.js",<br>
>  "private": true<br>
>}

对于完整的 package.json (比如别人的开源项目), 只需对整个项目执行 npm install 即可安装 package.json 文件中声明的所有插件模块。





# 作为项目的开发依赖(devDependencies)安装

`$ npm install --save-dev gulp`

> `—save-dev` 这个参数会将插件信息自动更新到 package.json 里，其中的 devDependencies 属性会随插件依赖的安装卸载而改变。

# 给项目目录安装常用的插件

`$ npm install gulp del gulp-cached gulp-uglify gulp-rename gulp-concat gulp-notify gulp-filter gulp-jshint gulp-ruby-sass gulp-rev-append gulp-cssnano gulp-replace gulp-imagemin browser-sync gulp-file-include gulp-autoprefixer --save-dev
`

- `del`插件： 文件删除
- `gulp-ruby-sass`插件： sass 编译
- `gulp-cached`插件： 缓存当前任务中的文件，只让已修改的文件通过管道
- `gulp-uglify`插件： js 压缩
- `gulp-rename`插件： 重命名
- `gulp-concat`插件： 合并文件
- `gulp-notify`插件： 相当于 `console.log()`
- `gulp-filter`插件： 过滤筛选指定文件
- `gulp-jshint`插件： js 语法校验
- `gulp-rev-append`插件： 插入文件指纹（MD5）
- `gulp-cssnano`插件： CSS 压缩
- `gulp-imagemin`插件： 图片优化
- `browser-sync`插件： 保存自动刷新
- `gulp-file-include`插件： 可以 `include` html 文件
- `gulp-autoprefixer`插件： 添加 CSS 浏览器前缀

#  `gulpfile.js`配置及说明

```js
/*!
 * gulp
 * $ npm install gulp del gulp-cached gulp-uglify gulp-rename gulp-concat gulp-notify gulp-filter gulp-jshint gulp-ruby-sass gulp-rev-append gulp-cssnano gulp-replace gulp-imagemin browser-sync gulp-file-include gulp-autoprefixer --save-dev
 */
 
// Load plugins
var gulp = require('gulp'), // 必须先引入gulp插件
    del = require('del'),  // 文件删除
    cached = require('gulp-cached'), // 缓存当前任务中的文件，只让已修改的文件通过管道
    uglify = require('gulp-uglify'), // js 压缩
    rename = require('gulp-rename'), // 重命名
    concat = require('gulp-concat'), // 合并文件
    notify = require('gulp-notify'), // 相当于 console.log()
    filter = require('gulp-filter'), // 过滤筛选指定文件
    jshint = require('gulp-jshint'), // js 语法校验
    sass = require('gulp-ruby-sass'), // sass 编译
    rev = require('gulp-rev-append'), // 插入文件指纹（MD5）
    cssnano = require('gulp-cssnano'), // CSS 压缩
    replace = require('gulp-replace'), // 批量替换字符串
    imagemin = require('gulp-imagemin'), // 图片优化
    browserSync = require('browser-sync'), // 保存自动刷新
    fileinclude = require('gulp-file-include'), // 可以 include html 文件
    autoprefixer = require('gulp-autoprefixer'); // 添加 CSS 浏览器前缀
 
// sass
gulp.task('sass', function() {
  return sass('src/sass/**/*.scss', {style: 'expanded'})  // 传入 sass 目录及子目录下的所有 .scss 文件生成文件流通过管道并设置输出格式
    .pipe(cached('sass'))  // 缓存传入文件，只让已修改的文件通过管道（第一次执行是全部通过，因为还没有记录缓存）
    .pipe(autoprefixer('last 6 version')) // 添加 CSS 浏览器前缀，兼容最新的5个版本
    .pipe(gulp.dest('dist/css')) // 输出到 dist/css 目录下（不影响此时管道里的文件流）
    .pipe(rename({suffix: '.min'})) // 对管道里的文件流添加 .min 的重命名
    .pipe(cssnano()) // 压缩 CSS
    .pipe(gulp.dest('dist/css')) // 输出到 dist/css 目录下，此时每个文件都有压缩（*.min.css）和未压缩(*.css)两个版本
});
 
// css （拷贝 *.min.css，常规 CSS 则输出压缩与未压缩两个版本）
gulp.task('css', function() {
  return gulp.src('src/css/**/*.css')
    .pipe(cached('css'))
    .pipe(gulp.dest('dist/css')) // 把管道里的所有文件输出到 dist/css 目录
    .pipe(filter(['**/*', '!**/*.min.css'])) // 筛选出管道中的非 *.min.css 文件
    .pipe(autoprefixer('last 6 version'))
    .pipe(gulp.dest('dist/css')) // 把处理过的 css 输出到 dist/css 目录
    .pipe(rename({suffix: '.min'}))
    .pipe(cssnano())
    .pipe(gulp.dest('dist/css'))
});
 
// styleReload （结合 watch 任务，无刷新CSS注入）
gulp.task('styleReload', ['sass', 'css'], function() {
  return gulp.src(['dist/css/**/*.css'])
    .pipe(cached('style'))
    .pipe(browserSync.reload({stream: true})); // 使用无刷新 browserSync 注入 CSS
});
 
// script （拷贝 *.min.js，常规 js 则输出压缩与未压缩两个版本）
gulp.task('script', function() {
  return gulp.src(['src/js/**/*.js'])
    .pipe(cached('script'))
    .pipe(gulp.dest('dist/js'))
    .pipe(filter(['**/*', '!**/*.min.js'])) // 筛选出管道中的非 *.min.js 文件
    // .pipe(jshint('.jshintrc')) // js的校验与合并，根据需要开启
    // .pipe(jshint.reporter('default'))
    // .pipe(concat('main.js'))
    // .pipe(gulp.dest('dist/js'))
    .pipe(rename({suffix: '.min'}))
    .pipe(uglify())
    .pipe(gulp.dest('dist/js'))
});
 
// image
gulp.task('image', function() {
  return gulp.src('src/img/**/*.{jpg,jpeg,png,gif}')
    .pipe(cached('image'))
    .pipe(imagemin({optimizationLevel: 3, progressive: true, interlaced: true, multipass: true}))
    // 取值范围：0-7（优化等级）,是否无损压缩jpg图片，是否隔行扫描gif进行渲染，是否多次优化svg直到完全优化
    .pipe(gulp.dest('dist/img'))
});
 
// html 编译 html 文件并复制字体
gulp.task('html', function () {
  return gulp.src('src/*.html')
    .pipe(fileinclude()) // include html
    .pipe(rev()) // 生成并插入 MD5
    .pipe(gulp.dest('dist/'))
});
 
// clean 清空 dist 目录
gulp.task('clean', function() {
  return del('dist/**/*');
});
 
// build，关连执行全部编译任务
gulp.task('build', ['sass', 'css', 'script', 'image'], function () {
  gulp.start('html');
});
 
// init 任务，重新执行全部编译
gulp.task('init', ['clean'], function() {
  gulp.start('build');
});
 
// default 默认任务，执行全部编译并启动监听
gulp.task('default', ['init'], function() {
  gulp.start('watch');
});
 
// watch 开启本地服务器并监听
gulp.task('watch', function() {
  browserSync.init({
    server: {
      baseDir: 'dist' // 在 dist 目录下启动本地服务器环境，自动启动默认浏览器
    },
    open: 'external' // 使用本机IP地址打开浏览器，方便局域网调试
  });
 
  // 监控 SASS 文件，有变动则执行CSS注入
  gulp.watch('src/sass/**/*.scss', ['styleReload']);
  // 监控 CSS 文件，有变动则执行CSS注入
  gulp.watch('src/css/**/*.css', ['styleReload']);
  // 监控 js 文件，有变动则执行 script 任务并自动刷新
  gulp.watch('src/js/**/*.js', ['script', browserSync.reload]);
  // 监控图片文件，有变动则执行 image 任务并自动刷新
  gulp.watch('src/img/**/*', ['image', browserSync.reload]);
  // 监控 html 文件，有变动则执行 html 任务并自动刷新
  gulp.watch('src/**/*.html', ['html', browserSync.reload]);
 
});
```

# 使用 gulp 的方法

执行gulp任务

`gulp taskname` 

如 `gulp sass`，不指定 `taskname` 则默认执行 `default` 任务