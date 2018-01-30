---
layout: post
title:  "VUE项目发布到Github"
categories: VUE
date:   2018-01-30 19:48:05
author: Logan
tags:  vue git-pages
---

* content
{:toc}

# 1.vue项目的创建

```js
$ vue init webpack my-project
$ cd my-project
$ npm install
$ npm run dev
```

# 2.配置文件修改

使用`npm run build`进行打包，这个时候你直接打开`dist/`下的`index.html`,会发现文件可以打开，但是所有的js，css，img等路径有问题是指向根目录的，此时需要修改`config/index.js`里的`assetsPublicPath`的字段，初始项目是`/`他是指向项目根目录的也是为什么会出现错误，这时改为`./`

```js
build: {
  env: require('./prod.env'),
  index: path.resolve(__dirname, '../dist/index.html'),
  assetsRoot: path.resolve(__dirname, '../dist'),
  assetsSubDirectory: 'static',
  assetsPublicPath: './',
  productionSourceMap: true,
  // Gzip off by default as many popular static hosts such as
  // Surge or Netlify already gzip all static assets for you.
  // Before setting to `true`, make sure to:
  // npm install --save-dev compression-webpack-plugin
  productionGzip: false,
  productionGzipExtensions: ['js', 'css'],
  // Run the build command with an extra argument to
  // View the bundle analyzer report after build finishes:
  // `npm run build --report`
  // Set to `true` or `false` to always turn it on or off
  bundleAnalyzerReport: process.env.npm_config_report
}
```

# 3.打包

```js
$ npm run build
```

# 4.建立远成仓库、关联、推送

- 第1步：创建一个名为`vue-demo`的Repository

- 第2步：运行下方命令，进行关联：

```js
$ git init
$ git remote add origin git@github.com:logan70/vue-demo.git
```

- 第3步：通过如下命令进行代码合并

```js
$ git pull --rebase origin master
```

- 第4步：添加文件、提交文件、推送到远程库

```js
// 添加文件
$ git add -A
$ git add -f dist  // 强制添加dist文件夹，因为.gitignore文件中定义了忽略该文件

// 提交文件
$ git comit -m "my vue project"

// 推送到远程库
$ git push -u origin master
```

# 5.配置Github Pages

在项目中`Setting`内`Github Pages`选项内选择`master branch`并保存


![vue-gh-pages](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2018-1-30/vue-1.jpg "vue-gh-pages")

![vue-gh-pages](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2018-1-30/vue-2.jpg "vue-gh-pages")

访问网址为[https://logan70.github.io/vue-zhihuribao/dist/](https://logan70.github.io/vue-zhihuribao/dist/)

**`http://username.github.io/xxx/dist`**
