---
layout: post
title:  "ckeditor 与 ckfinder基于Struts2 集成实现在线编辑以及文件上传(JAVA版)"
categories: JAVA
tags: 编辑器
description: ckeditor 与 ckfinder基于Struts2
---

* content
{:toc}


  　FCKeditor（ckeditor ）是目前最优秀的可见即可得网页编辑器之一，它采用JavaScript编写。具备功能强大、配置容易、跨浏览器、支持多种编程语言、开源等特点。它非常流行，互联网上很容易找到相关技术文档，国内许多WEB项目和大型网站均采用了FCKeditor。
 
　　CKFinder是一个强大而易于使用的Web浏览器的Ajax文件管理器。其简单的界面使得它直观，快速学习的各类用户，从高级人才到互联网初学者。
 
 <!--more-->


## 下载所需文件

> **ckeditor**：[http://ckeditor.com/download](http://ckeditor.com/download)

![下载页面](http://img.blog.csdn.net/20150228152532644?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamFuZGEyMDEx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

建议Full Package，工具齐全；

以及下载所需JAR包
![Full Package](http://img.blog.csdn.net/20150228152522504?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamFuZGEyMDEx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

> **CKFinder**：[http://cksource.com/ckfinder/download#java-download](http://cksource.com/ckfinder/download#java-download)

![java](http://img.blog.csdn.net/20150228152536245?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamFuZGEyMDEx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

> **注意：请选择JAVA版下载**



 
 
## 配置ckeditor与ckfinder环境

解压下载的ckeditor 以及ckfinder,将解压了的文件夹  拷贝，放置在项目WebRoot目录下

<img src="http://img.blog.csdn.net/20150228152527837?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamFuZGEyMDEx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" align="center" alt="环境" />


将解压好的相关的JAR包加入lib目录下..

> **ckeditor 与ckfinder整合在一起(注意目录)**


修改ckeditor下面的`config.js`配置

添加以下信息：
```java
config.language =  "zh-cn" ;
config.image_previewText = ' ';
config.filebrowserBrowseUrl =  '/testCK/ckfinder/ckfinder.html' ;  
config.filebrowserImageBrowseUrl =  '/testCK/ckfinder/ckfinder.html?type=Images' ;  
config.filebrowserFlashBrowseUrl =  '/testCK/ckfinder/ckfinder.html?type=Flash' ;  
config.filebrowserUploadUrl =  '/testCK/ckfinder/core/connector/java/connector.java?  command=QuickUpload&type=Files' ;  
config.filebrowserImageUploadUrl =  '/testCK/ckfinder/core/connector/java/connector.java?command=QuickUpload&type=Images' ;  
config.filebrowserFlashUploadUrl =  '/testCK/ckfinder/core/connector/java/connector.java?command=QuickUpload&type=Flash' ;  
config.filebrowserWindowWidth = '1000';  
config.filebrowserWindowHeight = '700';
```
 
 
## 引用ckeditor与ckfinder
页面引入对应的JS，以及给textarea添加样式与提示语
![添加提示语](http://img.blog.csdn.net/20150228152459322?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamFuZGEyMDEx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


最终展示效果：

![最终展示效果](http://img.blog.csdn.net/20150228152451117?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamFuZGEyMDEx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

ckfinder整合在web.xml加入对应的servlet具体加入内容可参考ckfinder解压的web.xml
![解压](http://img.blog.csdn.net/20150228152502377?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamFuZGEyMDEx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

同时将`config.xml`拷贝只项目WEB-INF下面，修改config.xml：设置ckfinder为启动，且设置上传文件的目录
 
![config.xml](http://img.blog.csdn.net/20150228152441723?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamFuZGEyMDEx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


> **PS**： Struts2在web.xml配置有设置为`/*`，所以也会拦截ckfinder的上传路径！

```
解决方案：修改struts2将/*改为*.action
struts2 2.3.4以下添加拦截器，继承FilterDispatcher
struts2 2.3.4以上（包含），在struts.xml添加
<constant name="struts.action.excludePattern" value="/ckfinder.*" />  
```

即可
效果展示：
图片上传：浏览服务器，可直接选择上传
![图片上传](http://img.blog.csdn.net/20150228152410401?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamFuZGEyMDEx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
![](http://img.blog.csdn.net/20150228152415952?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamFuZGEyMDEx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

服务器已存在上传的图片