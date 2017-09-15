---
layout: post
title:  "复习JavaScript--JavaScript数组"
categories: Javascript
date:   2017-09-15 18:48:05
author: Logan
tags:  Javascript
---

* content
{:toc}

# 数组Array

**创建数组**

两种基本方式

```js
//使用Array构造函数
var arr1 = new Array();           //空数组
var arr1 = new Array(4);          //包含四个项目的数组
var arr1 = new Array(1,3,6,9);    //包含1,3,6,9四个项目的数组

//使用数组字面量表示法
var arr2 = [];                    //空数组
var arr2 = [1,3,"red","blue"];
```

**数组元素的读写**

读取和设置值时，使用方括号`[]`并提供相应的索引

索引是从零开始的正整数

```js
var arr = [1,3,"red","blue"];
arr[0];    //1
arr[1];    //3
arr[2];    //"red"
arr[3];    //"blue"
```




**数组长度**

`array.length`

> 可以通过设置`array.length`来从数组末尾移出项或向数组中添加项

**数组的遍历**

```js
for(var i = 0, len = array.length;i < len;i++){
  console.log(array[i]);
}
```

## 数组的方法

### push()

语法：`array.push(newele1,newele2...)`

作用：将新的参数按顺序添加到array的**尾部**

返回值：把指定的值添加到数组尾部后的**新长度**

```js
var arr = [1,3];
arr.push(6,9);    //4
arr;              //[1,3,6,9]
```

### unshift()

语法：`array.unshift(newele1,newele2...)`

作用：将新的参数按顺序添加到array的**头部**

返回值：把指定的值添加到数组头部后的**新长度**

```js
var arr = [1,3];
arr.unshift(6,9);    //4
arr;                 //[6,9,1,3]
```

### pop()

语法：`array.pop()`

作用：删除array的**最后一个元素**

返回值：被删除的元素

```js
var arr = [1,2,3];
arr.pop();    //3
arr;          //[1,2]
```

### shift()

语法：`array.shift()`

作用：删除array的**第一个元素**

返回值：被删除的元素

```js
var arr = [1,2,3];
arr.shift();    //1
arr;          //[2,3]
```

### join()

语法：`array.join(separator)`

作用：用于把数组中的所有元素放入一个字符串

返回值：字符串

`separator`：分隔符，默认逗号`,`

```js
var arr = [1,2,3];
arr.join();    //"1,2,3"
```

### reverse()

语法：`array.reverse()`

作用：用于颠倒数组中元素的顺序

返回值：数组

```js
var arr = [1,2,3];
arr.reverse();    //[3,2,1]
```

### sort()

语法：`array.sort(sortby)`

作用：用于对数组中的元素进行排序

返回值：数组

`sortby`：规定排序顺序，必须是函数，默认按字符串排序

```js
//不设比较函数
var arr = [1,23,45,18,215];
arr.sort();    //[1,18,215,23,45]
//设置比较函数，升序排列
var arr = [1,23,45,18,215];
arr.sort(function(a,b){return a - b;});    //[1, 18, 23, 45, 215]
//设置比较函数，降序排列
var arr = [1,23,45,18,215];
arr.sort(function(a,b){return b - a;});    //[215, 45, 23, 18, 1]
```

### concat()

语法：`array.concat(arr1,arr2...)`

作用：用于合并两个或者多个数组

返回值：数组

```js
var arr1 = [1,2,3];
var arr2 = [4,5,6];
arr1.concat(arr2);    //[1, 2, 3, 4, 5, 6]
```

### slice()

语法：`array.slice(start[,end])`

作用：从某个已有的数组返回选定的元素

返回值：新数组

> 不设置`end`则从`start`开始截取到数组结束

```js
var arr1 = [1,2,3,4,5,6];
arr1.slice(1,4);    //[2, 3, 4]
```

### splice()

语法：`array.splice(index,count,item1,.....,itemX)`

作用：添加/删除/替换 数组中项目

返回值：被删除的项目

> - `index`：必需。整数，规定添加/删除项目的位置，使用负数可从数组结尾处规定位置
> - `count`：必需。要删除的项目数量。如果设置为 0，则不会删除项目
> - `itemX`：可选。向数组添加的新项目

```js
//添加
var arr1 = [1,2,3,4,5,6];
arr1.splice(1,0,18,28);    //[]
arr1;                      //[1, 18, 28, 2, 3, 4, 5, 6]
//删除
var arr1 = [1,2,3,4,5,6];
arr1.splice(1,2);          //[2, 3]
arr1;                      //[1, 4, 5, 6]
//替换
var arr1 = [1,2,3,4,5,6];
arr1.splice(1,2,18,28);    //[2, 3]
arr1;                      //[1, 18, 28, 4, 5, 6]
```

> `indexOf()`和`lastIndexOf()`方法是ES5新增，兼容性要求`IE9+`

### indexOf()

语法：`array.indexOf(searchvalue,startindex)`

作用：查找某个指定的数组项在数组中首次出现的位置

返回值：查找项在数组中的索引，没有找到返回`-1`

> - `searchvalue`：必需。查找的项
> - `startindex`：可选。开始检索的位置
> - 如果要检索的数组项没有出现，则该方法返回 -1

```js
var arr1 = [1,2,3,4,5,6,3];
arr1.indexOf(3);          //2
arr1.indexOf(3,3);        //6
arr1.indexOf(8);          //-1
```

### lastIndexOf()

语法：`array.lastIndexOf(searchvalue,startindex)`

作用：从数组的末尾开始查找

返回值：查找项在数组中的索引，没有找到返回`-1`

> - `searchvalue`：必需。查找的项
> - `startindex`：可选。开始检索的位置
> - 如果要检索的数组项没有出现，则该方法返回 -1

```js
var arr1 = [1,2,3,4,5,6,3];
arr1.lastIndexOf(3);          //6
arr1.lastIndexOf(3,5);        //2
arr1.lastIndexOf(3,6);        //6
arr1.lastIndexOf(8);          //-1
```

## 实例：拷贝一个数组

`var a = [1,3,5,7],b;`，实现b数组对a数组的拷贝

```js
var a = [1,3,5,7],
    b;
//push()方法
b = new Array();
for(i in a){
  b.push(a[i]);
}
//concat()方法
b = [].concat(a);
//slice()方法
b = a.slice(0);
```

> 对于webkit, 使用concat() <br> 其他浏览器, 使用slice()

## 封装实现`indexOf()`方法

```js
function arrIndexOf(arr,value){
  for(var i = 0, len = arr.length;i < len;i++){
    if(arr[i] === value){
      return i;
    }
  }
  return -1;
}
```