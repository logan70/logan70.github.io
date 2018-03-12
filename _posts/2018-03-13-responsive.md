---
layout: post
title:  "响应式布局详解"
categories: HTML
date:   2018-03-13 11:48:05
author: Logan
tags:  responsive
---

* content
{:toc}

# 响应式设计的步骤

## 题外话：低版本浏览器支持html及css3设置

```html
<!--[if lt IE 9]>     
<script src="http://css3-mediaqueries-js.googlecode.com/svn/trunk/css3-mediaqueries.js"></script>
<script src="http://html5shim.googlecode.com/svn/trunk/html5.js"></script>
<![endif]-->
```

## 布局及meta标签

- 先写非响应式布局，页面固定宽度大小

- 添加媒体查询（Media Query)和响应式代码

> 设置屏幕按1：1的尺寸显示，在 iPhone 和其他智能手机的浏览器提供网站全视图浏览，并禁止用户缩放页面。

```html
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
<meta name="HandheldFriendly" content="true">
```





## 通过媒体查询来设置样式media query

media query 是响应式设计的核心，它能够和浏览器进行沟通，告诉浏览器页面如何呈现

```css
/**ipad**/
@media only screen and (min-width:768px) and (max-width:1024px){}
/**iphone**/
@media only screen and (width:320px) and (width:768px){}
```

## 字体设置

`rem` 单位，他们转化为像素大小取决于页根元素的字体大小，即 `html`元素的字体大小。根元素字体大小乘以你 `rem` 值。

**使用rem单位，就按以下三个步骤来计算：**

- 确定基数：一般10px

- `html {font-size:百分数;}`    百分数=基数/16  

>基数10    百分数62.5% <br>
>基数14    百分数87.5%

> 根 html 元素将继承浏览器中设置的字体大小，除非显式设置固定值去覆盖。

- px换算rem   rem数值=px值/基数

> 字号14px，换算成rem就是14px/10=1.4rem

> 使用 rem 单位的主要目的应该是确保无论用户如何设置自己的浏览器，我们的布局都能调整到合适大小。

**em是相对于使用em单位的元素的字体大小**

元素的字体大小可以影响 em 值，但这种情况的发生，纯粹是因为继承

> 使用 em 单位的唯一原因是缩放没有默认字体大小的元素。

## 响应式设计需要注意的问题

- 响应式设计需要注意的问题

- 可以通过css样式max-width来进行控制图片的最大宽度

```css
#wrap img{
  max-width:100%;
  height:auto;
}
#log a{display:block;
  width:100%;
  height:40px;
  text-indent:55rem;
  background-img:url(logo.png);
  background-repeat:no-repeat;
  background-size:100% 100%;
}
```

# rem适配不同屏幕的移动设备

## 先设置header里面的meta标签

```html
<meta name="viewport" content="initial-scale=1,maximum-scale=1, minimum-scale=1">
```

## 在header写上`<script>`标签

```js
document.documentElement.style.fontSize = document.documentElement.clientWidth / 640*100 + 'px';
```

设计稿宽度是640px, 这样设置，html根元素的字体大小就是100px， 也就是1rem=100px

写入样式时，rem相对于px缩进两位，简单便捷

