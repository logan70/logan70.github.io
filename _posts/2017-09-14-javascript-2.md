---
layout: post
title:  "复习JavaScript--JavaScript语句"
categories: Javascript
date:   2017-09-14 16:48:05
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
  x="Today is Sunday";
  break;
case 1:
  x="Today is Monday";
  break;
case 2:
  x="Today is Tuesday";
  break;
case 3:
  x="Today is Wednesday";
  break;
case 4:
  x="Today is Thursday";
  break;
case 5:
  x="Today is Friday";
  break;
case 6:
  x="Today is Saturday";
  break;
}
document.write(x);
```

> case有几个都行，default可以没有。当case1~n都不满足的时候，则default。default并不一定要在最后<br>在case所执行的语句后添加上一个break语句。否则就直接继续执行下面的case中的语句

# for 循环语句

## for 循环

**语法：**

```js
for(循环初值;循环条件;步长){
	被执行的代码块
}
```

**实例：**

```js
//1+2+....+99的值
var sum = 0;
for(var i = 0;i < 100;i++){
	sum += i;
}
```

**循环嵌套规则**

- 外层为假时内层不执行
- 先执行外层再执行内层，直至内层条件为假时再返回外层去执行

## for/in 循环

> 用来遍历对象的属性

**实例：**

```js
var x;
var txt="";
var person={fname:"Bill",lname:"Gates",age:56}; 

for (x in person)
{
txt += person[x];
}

document.write(txt);   //BillGates56
```

# while 循环语句

## while 循环

> While 循环会在指定条件为真时循环执行代码块

**语法：**

```js
while (条件){
  需要执行的代码
}
```

**实例：**

```js
var i = 0;
while (i<5){
  document.write(i);    //01234
  i++;
}
```

> 如果忘记改变条件中所用变量的值，该循环永远不会结束，可能导致浏览器崩溃

## do/while 循环

**语法：**

```js
do{
  需要执行的代码
}while (条件);
```

**实例：**

```js
var i = 0;
do{
  document.write(i);    //01234
  i++;
}while (i<5);
```

> `do/while`循环至少会执行一次，即使条件是 false

> **`for`循环**适合已知循环次数的循环体<br>
> **`while`循环**适合未知循环次数的循环体

# break 退出循环

```js
for (i=0;i<10;i++){
  if (i==3){
    break;
  }
  document.write(i);   //012
}
```

# continue 退出本次循环

```js
for (i=0;i<10;i++){
  if (i==3){
    continue;
  }
  document.write(i);   //012456789
}
```