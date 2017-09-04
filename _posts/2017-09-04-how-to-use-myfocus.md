---
layout: post
title:  "焦点图插件myFocus使用心得"
date:   2017-09-04 20:44:05
categories: Tools
author: Logan
tags:  Tools JavaScript
---

* content
{:toc}

**觉得有用的可以去下面的源码地址Fork或者Clone到本地使用，希望给个Star支持一下**

[myFocus源码](https://github.com/logan70/myfocus "myFocus源码")

[myFocus效果展示](https://logan70.github.io/myfocus/ "myFocus效果展示")

[myFocus使用教程](https://logan70.github.io/2017/09/04/how-to-use-myfocus/ "myFocus使用教程")

# myFocus使用步骤

## Step 1. 在html的标签内引入相关文件

```js
<script type="text/javascript" src="js/myfocus-2.0.0.min.js"></script><!--引入myFocus库-->
<script type="text/javascript" src="js/mf-pattern/slide3D.js"></script><!--引入风格js文件-->
<link href="js/mf-pattern/slide3D.css" type="text/css" /><!--引入风格css文件-->
```




## Step 2. 创建myFocus标准的html结构，并填充你的内容

```html
<div id="boxID"><!--焦点图盒子-->
  <div class="loading"><img src="img/loading.gif" alt="请稍候..." /></div><!--载入画面(可删除)-->
  <div class="pic"><!--内容列表(li数目可随意增减)-->
  	<ul>
        <li><a href="#"><img src="img/1.jpg" thumb="" alt="标题1" text="详细描述1" /></a></li>
        <li><a href="#"><img src="img/2.jpg" thumb="" alt="标题2" text="详细描述2" /></a></li>
        <li><a href="#"><img src="img/3.jpg" thumb="" alt="标题3" text="详细描述3" /></a></li>
        <li><a href="#"><img src="img/4.jpg" thumb="" alt="标题4" text="详细描述4" /></a></li>
        <li><a href="#"><img src="img/5.jpg" thumb="" alt="标题5" text="详细描述5" /></a></li>
  	</ul>
  </div>
</div>
```

>IMG标签的属性说明：
>src : 图片地址；
>thumb : 图片的略缩图地址(需要风格支持，可以省略，如果省略即把大图地址作为它的地址)；
>alt : 图片的描述文字；
>text : 图片更详细的描述文字(需要风格支持，可以省略)

>boxID可以自定义，loading和pic的类名为指定类名，不可修改

## Step 3. 在step1代码之后的任意一个位置调用(建议在head标签结束前调用)

**简单调用**

```js
//你可以简单的调用---只设置它的盒子id，其它参数全部默认设置：
<script type="text/javascript">
	myFocus.set({id:'boxID'});
</script>
```

**参数调用**

```js
//或详细一点的参数调用：
<script type="text/javascript">
	myFocus.set({
	    id:'boxID',//焦点图盒子ID
	    pattern:'mF_fancy',//风格应用的名称
	    time:3,//切换时间间隔(秒)
	    trigger:'click',//触发切换模式:'click'(点击)/'mouseover'(悬停)
	    width:450,//设置图片区域宽度(像素)
	    height:296,//设置图片区域高度(像素)
	    txtHeight:'default'//文字层高度设置(像素),'default'为默认高度，0为隐藏
	});
</script>
```

>经过这3步，你应该可以在浏览器欣赏到你的杰作了。enjoy it~

# myFocus的文件结构与自动引入风格文件机制

## 自动引入风格文件机制

事实上，风格文件是不需要在使用时手动引入，myFocus会根据你的pattern设置，寻找myFocus库文件目录下的mf-pattern目录，当找到相应的风格文件后，自动引入。

这样，你只需要把你的风格文件放在myFocus库文件目录下的mf-pattern目录内，即可实现自动引入机制。

例如，你的myFocus-2.0.0.min.js文件放在js目录，那么，只需在js目录内建立一个mf-pattern的子目录，这个子目录便是myFocus程序可以识别的存放风格文件的目录。

在mf-pattern目录中，也存在一个img的子目录，它是存放某些风格的图片文件，虽然并不是每款风格都会有图片文件。

建议把所有的风格文件都存放在这个mf-pattern目录，这样你就可以随意切换你的风格了，而且它是按需加载，并不会引入其它多余的文件。myFocus的整个加载量(主库+风格)平均只有12KB左右。

## 不会重复引入风格文件

另外需要说明的是，这个自动引入机制已经做的足够智能，它并不会重复引入风格文件，例如当你已经手动引入风格文件，又或者干脆把某风格的js代码写在页面上，这时myFocus并不会再次寻找引入风格文件，而是直接读取页面上的。