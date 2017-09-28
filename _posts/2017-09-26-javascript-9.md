---
layout: post
title:  "JavaScript DOM 编程艺术 - 查漏补缺"
categories: Javascript
date:   2017-09-26 18:48:05
author: Logan
tags:  Javascript
---

* content
{:toc}


## 实现 getELementsByClassName()

```js
function getElementsByClassName(node,classname) {
	if(node.getElementsByClassName) {
		//如果方法存在，则使用现有的方法
		return node.getElementsByClassName(classname);
	}else {
		var results = new Array(),
			elems = node.getElementsByTagName('*');
		for(var i=0;i<elems.length;i++){
			if(elems[i].className.indexOf(classname) != -1){
				results.push(elems[i]);
			}
		}
		return results;
	}
}
```

## 获取元素的所有子元素

`element.childNodes`

`element.childNodes[0]` 等同于`element.firstChild`

`element.childNodes[element.childNodes.length-1]` 等同于`element.lastChild`

## nodeType属性

`node.nodeType`

- **元素节点**的nodeType属性值是1
- **属性节点**的nodeType属性值是2
- **文本节点**的nodeType属性值是3

## nodeValue属性

```js
// 改变元素内文本的内容
var box = document.getElementById('box');
box.childNodes[0].nodeValue = "hello world!";
```

## 共享onload事件

```js
function addLoadEvent(func) {
	var oldonload = window.onload;
	if(typeof window.onload != 'function'){
		window.onload = func;
	} else {
		window.onload = function() {
			oldonload();
			func();
		}
	}
}
```

## createTextNode()方法

```js
var text = document.createTextNode('hello world');
box.appendChild(text);
```

## insertBefore()方法

在已有元素前插入一个新元素

```js
// 语法
parent.insertBefore(newElement,targetElement);
// 使用
var newEle = document.createElement('p');
var box document.getElementById('box');
box.parentNode.insertBefore(newEle,box);
```

## 实现insertAfter()方法

重点是`element.nextSibling`

```js
function insertAfter(newElement,targetElement) {
	var parent = targetElement.parentNode;
	parent.insertBefore(newElement,targetElement.nextSibling);
}
```

## 关于节点

`firstChild`, `lastChild`, `nextSibling`, `previousSibling` 都会将空格或者换行当做节点处理,使用需谨慎

- `firstElementChild` ： 父元素的第一个元素节点
- `lastElementChild` ： 父元素的最后一个元素节点
- `nextElementSibling` ： 下一个元素节点
- `previousElementSibling` ： 上一个元素节点
- `childElementCount` ： 子元素节点数量

## Ajax

### XMLHttpRequest 发送请求

`open(method,url,saync)`

- `method`： 请求方式，`get/post`，不区分大小写。一般用大写
- `url`： 请求地址（相对/绝对）
- `async`： 异步/同步，`true/false`

`send(string)`

- `GET`方法时，不填或者填`null`，（实际没有主体，参数都拼在`url`中）
- `POST`方法时，一定要填

### readyState属性

- `0`：请求未初始化，`open`还没有调用
- `1`：服务器连接已建立，`open`已调用
- `2`：请求已接收，接收到头信息了
- `3`：请求处理中，接收到响应主体了
- `4`：请求已完成，响应已就绪

### XMLHttpRequest 取得的响应

- `responseText`：获得字符串形式的响应数据
- `responseXML`：获得XML形式的响应数据
- `status`和`statusText`：以数字和文本形式返回的HTTP状态码
- `getAllResponseHeader()`：获取所有的响应报头
- `getResponseHeader(string)`：查询响应中的某个字段的值

### XMLHttpRequest 处理响应

通过`onreadystatechange`函数


```js
var request = new XMLHttpRequest();
request.open('GET','get.php',true);
request.onreadystatechange = function(){
	if(request.readyState == 4  && request.status == 200){
		console.log(request.responseText)
	}
}
```

## 