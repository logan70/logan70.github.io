---
layout: post
title:  "复习JavaScript--JavaScript语句"
categories: Javascript
date:   2017-09-13 16:48:05
author: Logan
tags:  Javascript
---

* content
{:toc}

# if分支语句

> if语句可以嵌套使用

**单分支控制**

语法：

```js
if(条件){
  只有当条件为 true 时执行的代码
}
```

实例

```js
var time = new Date().getHours();
if(time > 22){
	document.write('晚安');
}
```




***

**双分支控制**

语法：

```js
if (条件){
  当条件为 true 时执行的代码
}
else{
  当条件不为 true 时执行的代码
}
```

实例

```js
var time = new Date().getHours();
if(time > 22){
	document.write('晚安');
}else{
	document.write('还没到睡觉时间');
}
```

***

**多分支控制**

语法：

```js
if (条件 1){
  当条件 1 为 true 时执行的代码
}
else if (条件 2){
  当条件 2 为 true 时执行的代码
}
else{
  当条件 1 和 条件 2 都不为 true 时执行的代码
}
```

实例

```js
var score = 88;
if(score < 60){
	document.write('不及格');
}else if(score < 80){
	document.write('继续加油');
}else{
	document.write('成绩很好，保持');
}
```

# switch分支语句

**多分支控制**

语法：

```js
switch(表达式)
{
case 值1:
  执行代码块 1
  break;
case 值2:
  执行代码块 2
  break;
...
case 值n:
  执行代码块 n
  break;
default:
  与 case值1 、 case值2...case值n 不同时执行的代码
}
```

实例：

```js
var day=new Date().getDay(),
    x;
switch (day)
{
case 0:
  x="Today it's Sunday";
  break;
case 1:
  x="Today it's Monday";
  break;
case 2:
  x="Today it's Tuesday";
  break;
case 3:
  x="Today it's Wednesday";
  break;
case 4:
  x="Today it's Thursday";
  break;
case 5:
  x="Today it's Friday";
  break;
case 6:
  x="Today it's Saturday";
  break;
}
document.write(x);
```

> case有几个都行，default可以没有。当case1~n都不满足的时候，则default。default并不一定要在最后<br>在case所执行的语句后添加上一个break语句。否则就直接继续执行下面的case中的语句

# for循环语句