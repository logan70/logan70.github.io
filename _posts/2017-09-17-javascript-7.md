---
layout: post
title:  "复习JavaScript--JavaScript中的DOM"
categories: Javascript
date:   2017-09-17 10:48:05
author: Logan
tags:  Javascript
---

* content
{:toc}

# DOM基础

## DOM查找方法

- `document.getElementById('id')`
- `document.getElementsByTagName('tagName')`

## 设置元素样式

**语法**

`ele.style.styleName = styleValue`

> 要设置的样式名称需为**驼峰式**(如`fontSize`)

```js
var box = document.getElementById('box');
box.style.fontSize = '16px';
```




## innerHTML

- `ele.innerHTML`

返回ele元素开始和结束标签之间的HTML内容

- `ele.innerHTML = 'html'`

设置ele元素开始和结束标签之间的HTML内容为'html'

## className

- `ele.className`

返回ele元素的class属性

- `ele.className = 'cls'`

设置ele元素的class属性为'cls'

> 重新设置类名，替换原来的类名

## getAttribute

- `ele.getAttribute('attribute')`

返回ele元素的'attribute'属性

- `ele.setAttribute('attribute',value)`

设置ele元素的'attribute'属性为value

- `ele.removeAttribute('attribute')`

删除ele元素的'attribute'属性

# DOM事件

## DOM0级事件

### HTML中绑定事件

直接在HTML元素标签内添加事件，执行脚本。

**语法**

`<tag 事件="执行脚本">xxx</tag>`

**功能**

在HTML元素上绑定事件

**说明**

执行脚本可以是一个函数的调用

```js
<input type="button" value="弹出" onclick="myFun()">
```

### JS中绑定事件

**语法**

`ele.事件=执行脚本`

执行脚本可以是一个匿名函数。也可以是一个函数的调用

```js
var box = document.getElementById('box');
box.onclick = function(){
	alert('123');
}
```

## 事件类型

**鼠标事件**

- `onclick`：鼠标点击时触发
- `onmouseover`：鼠标滑过时触发
- `onmouseout`：鼠标离开时触发
- `onfocus`：获得焦点时触发
- `onblur`：失去焦点时触发
- `onchange`：域的内容发生改变时触发
- `onsubmit`：表单中的确认按钮被点击时触发
- `onmousedown`：鼠标按下时时触发
- `onmousemove`：鼠标移动时触发
- `onmouseup`：鼠标松开时触发

- `onload`：页面加载时触发
- `onresize`：当调整浏览器窗口大小时触发
- `onscoll`：滚动条滚动时触发

**键盘事件**

- `onkeydown`：键盘按键按下时触发
- `onkeyup`：键盘按键松开时触发
- `onkeypress`：键盘按键按下并松开时触发

> `keyCode`：返回`onkeydown`、`onkeyup`和`onkeypress`事件触发的键值的字符代码或键的代码

```js
document.onkeypress = function(event){
	console.log(event.keyCode);
}
```
