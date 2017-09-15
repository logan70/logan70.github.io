---
layout: post
title:  "复习JavaScript--JavaScript字符串"
categories: Javascript
date:   2017-09-15 19:48:05
author: Logan
tags:  Javascript
---

* content
{:toc}

# 字符串String的方法

## charAt()

语法：`string.charAt(index)`

作用：返回string中index位置的字符

## charCodeAt()

语法：`string.charCodeAt(index)`

作用：返回string中index位置字符的字符编码

> ECMAScript5中可使用`string[i]`来访问字符串中的特定的字符，但是IE7及更早的浏览器会返回`undefined`

```js
var str = "hello";
str.charAt(0);    //h
```




## indexOf()

语法：`string.indexOf(searchvalue)`

作用：从一个字符串中搜索给定的子字符串

返回值：子字符串在字符串中的位置，如果没有找到则返回`-1`

> - `searchvalue`：必需。需要查找的子字符串

```js
var str = "hello world";
str.indexOf("world");    //6
str.indexOf("x");        //-1
```

## lastIndexOf()

语法：`string.lastIndexOf(searchvalue)`

作用：从一个字符串中搜索给定的子字符串,倒序搜索

返回值：子字符串在字符串中的位置，如果没有找到则返回`-1`

> - `searchvalue`：必需。需要查找的子字符串

```js
var str = "hello world";
str.lastIndexOf("world");    //6
str.lastIndexOf("o");        //7
```

## split()

语法：`string.split(separator)`

作用：把一个字符串分割成数组

返回值：数组

> - `separator`：必需。分隔符

```js
var str = "welcome-to-china";
str.split("-");         // ["welcome", "to", "china"]
```

## replace()

语法：`string.replace(regexp/substr,replacement)`

作用：把字符串中的指定子字符串或与正则表达式相匹配的子字符串替换

返回值：替换后的字符串

> - `regexp/substr`：必需。规定要替换的子字符串或正则表达式
> - `replacement`：必需。替换字符串或函数

```js
var str = "hello world";
str.replace("world","logan");   // "hello logan"
```

## toUpperCase()

语法：`string.toUpperCase()`

作用：将字符串转换为大写

返回值：替换后的字符串

> 生成的是一个副本，不会修改原字符串

```js
var str = "hello world";
str.toUpperCase();   // "HELLO WORLD"
```

## toLowerCase()

语法：`string.toLowerCase()`

作用：将字符串转换为大写

返回值：替换后的字符串

> 生成的是一个副本，不会修改原字符串

```js
var str = "HELLO WORLD";
str.toLowerCase();   // "hello world"
```

# 字符串对象的截取方法

## slice()

语法：`string.slice(start[,end])`

作用：截取子字符串

返回值：子字符串

> - `start`：必需。指定子字符串开始的位置
> - `end`：可选。指定子字符串结束的位置，省略时截取至字符串末尾

```js
var str = "helloworld";
str.slice(1,3);         // "el"
str.slice(7);           // "rld"
```

> - **参数为负**，其值转换为为字符串长度+该负数

```js
var str = "helloworld";
// 一个参数，与字符串长度相加即为slice(7)
str.slice(-3);   // "rld"
// 两个参数，与字符串长度相加即为slice(3,6)
str.slice(3,-4); // "low"
```

> - **第二个参数比第一个参数小**，返回空字符串

```js
var str = "helloworld";
str.slice(5,3);    // ""
```

## substring()

语法：`string.slice(start[,end])`

作用：截取子字符串

返回值：子字符串

> - 传递参数为正值情况：与`slice()`方法行为相同的

```js
var str = "helloworld";
str.substring(1,3);         // "el"
str.substring(7);           // "rld"
```

> - **参数为负**，会被转化为0
> - 会将较小的数作为开始位置，将较大的数作为结束位置

```js
var str = "helloworld";
// 两个参数，-4会转换为0，相当于substring(3,0) -->即为 substring(0,3)
str.substring(3,-4);    // "hel"
```

## substr()

语法：`string.substr(start[,len])`

作用：截取子字符串

返回值：子字符串

> - `start`：必需。指定子字符串开始的位置
> - `len`：可选。截取的字符数，省略时截取至字符串末尾

```js
var str = "helloworld";
// 一个参数，则将字符串长度作为结束位置
str.substr(3); // "loworld"
// 两个参数，从位置3开始截取后面7个字符
str.substr(3,3); // "low"
```

> - **参数为负**，第一个`start`转换为为字符串长度+该负数，第二个`len`转换为0

```js
var str = "helloworld";
// 将第一个负的参数加上字符串的长度--->
//即为：substr(7,5) ，从位置7开始向后截取5个字符
str.substr(-3,2); // "rl"
// 将第二个参数转换为0
// 即为：substr(3,0)，即从位置3截取0个字符串，则返回空
str.substr(3,-2); // ""
```

# 字符串方法综合应用

## 获取扩展名

```js
//获取扩展名
function getFileFormat(url){
    var dot = url.lastIndexOf(".");
    return url.slice(dot);
}
var url = "http://www.baidu.com/index.mp4";
console.log(getFileFormat(url));     //".mp4"
```

## `-`连接的字符串改为驼峰形式

```js
function camel(str){
	//将str拆分为数组
	var arr = str.split("-"),
	    newstr = arr[0];
	for(var i = 1,len = arr.length;i<len;i++){
		newstr += arr[i].charAt(0).toUpperCase() + arr[i].slice(1)
	}
	return newstr;
}
camel("border-left-color");   //"borderLeftColor"
```