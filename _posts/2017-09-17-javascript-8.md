---
layout: post
title:  "复习JavaScript--JavaScript中的BOM"
categories: Javascript
date:   2017-09-17 17:48:05
author: Logan
tags:  Javascript
---

* content
{:toc}

**BOM(Browser Object Model)浏览器对象模型**

# window

> window 既是通过JavaScript访问浏览器窗口的一个借口，又是ECMAScript规定的Global对象

> 所有 JavaScript 全局对象、函数以及变量均自动成为 window 对象的成员

**全局变量是 window 对象的属性**

```js
//下面两种表达方式等价
var age = 23;
window.age = 23;
```

**全局函数是 window 对象的方法**

```js
//下面两种表达方式等价
function myFun(){
	...
}
window.myFun = function(){
	...
}
```

**HTML DOM 的 document 也是 window 对象的属性之一**

```js
//下面两种表达方式等价
window.document.getElementById("header");
document.getElementById("header");
```





## window 对象的方法

**alert**

语法：`window.alert('content');`

功能：显示一个带有一段消息和一个确认按钮的警告框

**confirm**

语法：`window.confirm('content');`

功能：显示一个带有指定消息和确认及取消按钮的对话框

**prompt**

语法：`window.prompt('text'[,'defaultText']);`

功能：显示一个带有指定消息[和默认输入内容]的输入框

**open**

语法：`window.open(url,name,features);`

功能：打开一个新的浏览器窗口或者查找一个已命名的窗口

**close**

语法：`window.close();`

功能：关闭浏览器窗口

## 超时调用

**设置超时调用**

语法：`setTimeout(code,millisec);`

功能：在指定的毫秒数后调用函数或计算表达式

**取消超时调用**

语法：`clearTimeout(id_of_timeout);`

```js
var timer = setTimeout(function{
	console.log('123');
	},2000);
clearTimeout(timer);
```

## 间歇调用

**设置间歇调用**

语法：`setInterval(code,millisec);`

功能：在指定的毫秒数后调用函数或计算表达式

**取消间歇调用**

语法：`clearInterval(id_of_interval);`

```js
var timer = setInterval(function{
	console.log('123');
	},2000);
clearInterval(timer);
```

# location

提供了与当前窗口中加载的文档有关信息，还提供了一些导航的功能

> location既是window对象的属性，也是document对象的属性

**location.href**

语法：`location.href;`

功能：返回当前页面的完整URL

**location.hash**

语法：`location.hash;`

功能：返回URL中的hash(`#`后跟零个或多个字符)，不包含则返回空字符串

**location.host**

语法：`location.host;`

功能：返回服务器名称和端口号（如果有）

**location.hostname**

语法：`location.hostname;`

功能：返回不带端口号的服务器名称

**location.pathname**

语法：`location.pathname;`

功能：返回URL中的目录和（或）文件名

**location.port**

语法：`location.port;`

功能：返回URL中指定的端口号，如果没有返回空字符串

**location.protocol**

语法：`location.protocol;`

功能：返回页面使用的协议

**location.search**

语法：`location.search;`

功能：返回URL中的查询字符串，这个字符串以`?`开头

**location.replace()**

语法：`location.replace(url);`

功能：重新定向url

> 使用location.replace()不会在历史记录中生成新记录

**location.assign()**

语法：`location.assign(url);`

功能：加载一个新的文档

> 使用location.assign()会在历史记录中生成新记录

**location.reload()**

语法：`location.reload([bForceGet]);`

功能：重新加载当前显示的页面

参数： bForceGet， 可选参数， 默认为 false，从客户端缓存里取当前页。true, 则以 GET 方式，从服务端取最新的页面, 相当于客户端点击 F5("刷新") 

## 分析下面的URL

 > http://www.example.com:8080/test.php?user=admin&pwd=admin#login <br>
想得到整个如上的完整url，我们用：location.href; <br>
得到传输协议http:,我们用：location.protocol; <br>
得到主机名连同端口www.example.com:8080，我们用：location.host; <br>
得到主机名www.example.com，我们用：location.hostname; <br>
得到主机后部分不包括问号?后部分的/test.php，就用我们刚才讲的：location.pathname;<br> 
得到url中问号?之后井号#之前的部分?user=admin&pwd=admin，我们就用： location.search; <br>
得到#之前的部分#login，我们就用location.hash

# history

history对象保存了用户在浏览器中访问页面的历史记录

**history.back()**

语法：`history.back()`

功能：回到历史记录上一步

说明：相当于使用了`history.go(-1)`

**history.forward()**

语法：`history.forward()`

功能：回到历史记录下一步

说明：相当于使用了`history.go(1)`

**history.go()**

语法：`history.go(-n)`

功能：回到历史记录上n步

语法：`history.go(n)`

功能：回到历史记录下n步

# screen

**screen.availWidth**

语法：`screen.availWidth`

功能：返回可用的屏幕宽度

**screen.availHeight**

语法：`screen.availHeight`

功能：返回可用的屏幕高度

# navigator

window.navigator 对象包含有关访问者浏览器的信息

**navigator.userAgent**

语法：`navigator.userAgent`

功能：用来识别浏览器名称、版本、引擎以及操作系统等信息的内容