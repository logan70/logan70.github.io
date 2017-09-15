---
layout: post
title:  "复习JavaScript--JavaScript函数"
categories: Javascript
date:   2017-09-15 16:48:05
author: Logan
tags:  Javascript
---

* content
{:toc}

# 函数

**作用**

通过函数可以封装任意多条语句，而且可以在任何地方、任何时候调用执行

**语法**

```js
function functionName([argument1,argument2...]){
  这里是要执行的代码
}
```

**实例**

```js
function myFunction(name,job){
  alert("Welcome " + name + ", the " + job);
}
```

**函数调用**

```js
functionName([argument1,argument2...]);
```

**带有返回值的函数**

```js
function add(num1,num2){
  var sum = num1 + num2;
  return sum;
}
document.write(add(2,5));
```




> 函数会在执行完`return`语句之后停止并立即退出

> `return`语句也可以不带任何的返回值，用于提前停止函数执行又不需要返回值的情况

```js
function double(num){
  if(isNaN(num)) return;
  return num*2;
}
```

# 函数的参数`arguments`

ECMAScript中的参数在内部用一个数组来表示，在函数体内通过`arguments`对象来访问这个数组参数

> 1. `arguments`对象只是与数组类似，并不是Array的实例
> 2. `arguments[i]`来访问它的每一个元素
> 3. `arguments.length`是传递参数的个数

**实例：求任意一组数的平均值**

```js
function getAverage(){
  var sum = 0;
  for(var i = 0, len = arguments.length; i < len; i++){
    sum += arguments[i];
  }
  return sum/len;
}
console.log(getAverage(1,5,6,8,45,14));
```