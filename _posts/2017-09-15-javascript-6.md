---
layout: post
title:  "复习JavaScript--JavaScript中的Math和Date"
categories: Javascript
date:   2017-09-15 19:48:05
author: Logan
tags:  Javascript
---

* content
{:toc}

# Math

## Math.min()

语法：`Math.min(num1,num2...numN)`

作用：求一组数中的最小值

## Math.max()

语法：`Math.max(num1,num2...numN)`

作用：求一组数中的最大值

> 一组数当中出现非数字，则返回NaN

```js
Math.min(5,-4,9,108,-55);    // -55
Math.max(5,-4,9,108,-55);    // 108
Math.max(5,-4,9,108,"a");    // NaN
```

## Math.ceil()

语法：`Math.ceil(num)`

作用：向上取整

```js
Math.ceil(5.3);   //6
```

## Math.floor()

语法：`Math.floor(num)`

作用：向下取整

```js
Math.floor(5.3);  //5
```

## Math.round()

语法：`Math.round(num)`

作用：四舍五入

```js
Math.round(5.49);   //5
Math.round(5.51);   //6
```

## Math.abs()

语法：`Math.abs(num)`

作用：返回num的绝对值

```js
Math.abs(5.5);    //5.5
Math.abs(-5.5);   //5.5
```

## Math.random()

语法：`Math.random(num)`

作用：返回大于等于0小于1的一个随机数

```js
Math.random(5.5);    //5.5
Math.random(-5.5);   //5.5
```

**求n到m之间的随机整数**

```js
var random = Math.floor(Math.random()*(m-n+1)+n);
```

# Date