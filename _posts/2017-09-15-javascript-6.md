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

## 创建Date对象

语法：`new Date()`

作用：创建一个日期时间对象

返回值：不传参的情况下，返回当前的日期时间对象

```js
var dater = new Date();  // Sat Sep 16 2017 13:13:21 GMT+0800 (中国标准时间)
```

## 获取年月日时分秒及星期的方法

- `getFullYear()`：返回4位数的年份
- `getMonth()`：返回日期中的月份，返回值为0-11
- `getDate()`：返回月份中的天数
- `getDay()`：返回星期，返回值为0-6
- `getHours()`：返回小时
- `getMinutes()`：返回分钟
- `getSeconds()`：返回秒钟
- `getTime()`：返回表示日期的毫秒数

```js
var weeks = ["日","一","二","三","四","五","六"],
    today = new Date(),           // Sat Sep 16 2017 14:11:35 GMT+0800 (中国标准时间)  
    year = today.getFullYear(),   // 2017
    month = today.getMonth()+1,     // 8
    date = today.getDate(),       // 16
    week = weeks[today.getDay()],        // 6
    hours = today.getHours(),     // 14
    minutes = today.getMinutes(), // 11
    seconds = today.getSeconds(), // 35
    time = "现在是"+year+"年"+month+"月"+date+"日"+hours+"时"+minutes+"分"+seconds+"秒，星期"+week;
time;  // "现在是2017年9月16日14时10分23秒，星期6"
```

## 设置年月日时分秒及星期的方法

- `setFullYear()`：设置4位数的年份
- `setMonth()`：设置日期中的月份，返回值为0-11
- `setDate()`：设置月份中的天数
- `setDay()`：设置星期，返回值为0-6
- `setHours()`：设置小时
- `setMinutes()`：设置分钟
- `setSeconds()`：设置秒钟
- `setTime()`：设置表示日期的毫秒数