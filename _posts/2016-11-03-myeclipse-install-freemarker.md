---
layout: post
title:  "MyEclipse 如何安装FreeMarker 插件"
categories: JAVA MyEclipse
tags:  freemarker 
---

* content
{:toc}



### freemarker-ide 插件

>MyEclipce 安装FreeMarker插件这绝对是最简单的了，比安装Velocity插件还简单。
>
[Velocity插件安装教程](http://blog.csdn.net/janda2011/article/details/43051469)
        
### 下载插件
到[https://sourceforge.net/projects/freemarker-ide/files/](https://sourceforge.net/projects/freemarker-ide/files/) 下载插件、可自行下载对应的版本，我下载的是目前最新的版本：**freemarker-ide-0.9.14**


<!--more-->

### 解压安装
将下载下来的[freemarker-ide-0.9.14.zip](https://sourceforge.net/projects/freemarker-ide/files/freemarker-ide/0.9.14/freemarker-ide-0.9.14.zip) 解压，解压出的文件夹放入到MyEclipse安装目录dropins 下
`dropins根据自己安装Myeclipse的路径选择 `，例：
![插件安装目录](/images/2016-11-03/01.jpg)

### 安装成功
重启Myeclipse，会自动提示成功安装该插件，如图：
![安装成功提示](/images/2016-11-03/02.jpg)

如果安装成功,则在 window --> Preferences 左边的树形栏里出现FreeMarker Editor一项新的内容

### 设置默认打开
设置工具中`*.ftl`文件都是用Freemarker Editor 打开：
![ftl默认打开格式](/images/2016-11-03/03.jpg)

### 自动高亮

>如果以上步骤顺利，打开`.ftl`文件，代码会自动高亮，还有自动提示功能！
>`.ftl`文件的icon也会改变

![代码高亮](/images/2016-11-03/04.jpg)

博客：[http://liujilu.com/](http://liujilu.com/)