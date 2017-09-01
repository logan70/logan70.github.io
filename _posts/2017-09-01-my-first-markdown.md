---
layout: post
title:  "我的第一个Markdown文件"
date:   2017-09-01 16:44:05
categories: Markdown
author: Logan
tags:  Markdown
---

* content
{:toc}

## 搭建站点

昨天，在慕课网上学习了课程[版本控制入门-搬进Github](http://www.imooc.com/learn/390)，开始学习如何使用GitHub。

今天了解到用GitHubpages可以搭建自己的站点，就想着能不能搭建一个自己的博客，忙活了大半天终于搭建好了，虽然也只是Fork别人的项目，但是心里还是感觉美滋滋。然后简单的学习了一下[Markdown的语法](http://wowubuntu.com/markdown/basic.html)，就跑来写第一篇博客了。

这里要感谢博客主题的提供者[gaohaoyang](https://github.com/Gaohaoyang)。

***

## 定个小目标

接下来的目标就是把之前所学的前端的知识梳理一遍，都放进自己的博客里面，不仅对之前的知识算是一个复习，也能够与看到我的博客的那些前端初学者们共勉。

***




## Markdown语法测试

**标题**

>行首插入1到六个`#`，代表标题1到6阶。


**效果**

### 三级标题


**代码**

`### 三级标题`

***

**区块引用**

>使用`>`角括号


**效果**

> 区块引用内容


**代码**

`> 区块引用内容`

***

**强调内容**

>星号标记强调的区段


**效果**

段落中**需要强调的内容**和*斜体内容*等等。


**代码**

`段落中**需要强调的内容**和*斜体内容*等等。`

***

**无序列表**

>使用`*` `-` `+` 表示无序列表


**效果**

- 无序列表项1
- 无序列表项2
- 无序列表项3
- 无序列表项4


**代码**

```md
- 无序列表项1
- 无序列表项2
- 无序列表项3
- 无序列表项4
```

***

**有序列表**

>使用`1.` `2.` `3.` 等数字加点号表示


**效果**

1. 有无序列表项1
2. 有无序列表项2
3. 有无序列表项3
4. 有无序列表项4


**代码**

```md
1. 有无序列表项1
2. 有无序列表项2
3. 有无序列表项3
4. 有无序列表项4
```

***

**项目中插入多项内容**


**效果**

- 无序列表
	内容区域1
	内容区域2
	> 无序列表内的区块引用
	
- 无序列表
	内容区域1
	内容区域2
	> 无序列表内的区块引用


**代码**

```md
- 无序列表
	内容区域1
	内容区域2
	> 无序列表内的区块引用
	
- 无序列表
	内容区域1
	内容区域2
	> 无序列表内的区块引用
```
	
***

**链接**


**效果**

[我的博客](https://logan70.github.io/)


**代码**

`[我的博客](https://logan70.github.io/)`

***

**带title的链接**


**效果**

[我的博客](https://logan70.github.io/ "This is my blog")


**代码**

`[我的博客](https://logan70.github.io/ "This is my blog")`

***

**图片**


**效果**

![This is a image](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2016-11-05/01.jpg "image's title")


**代码**

`![This is a image](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2016-11-05/01.jpg "image's title")`

***

**代码**


**效果**

`<p>这是一个段落</p>`

	<p>这是一个段落</p>


**代码**

```md
`<p>这是一个段落</p>`

	<p>这是一个段落</p>
```

***

**代码块**


**效果**

```html
	<!DOCTYPE HTML>
	<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
		<title>下拉菜单</title>
	</head>
	<body> 
	</body>
	</html>
```


**代码**

```md
	```html
		<!DOCTYPE HTML>
		<html>
		<head>
			<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
			<title>下拉菜单</title>
		</head>
		<body> 
		</body>
		</html>
	```
```


