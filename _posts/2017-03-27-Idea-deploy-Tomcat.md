---
layout: post
title:  "Intellij IDEA 中如何部署项目到 Tomcat？"
categories: JAVA
tags: 编辑器
description: Intellij IDEA 中如何部署项目到 Tomcat
---

* content
{:toc}


## 配置项目
- - -
### 添加Project Structure
打开项目设置，快捷键`ctrl + shift + alt`
![ProjectStructure|center](/images/2017-03-27/01.png)
 1. 设置Modules
 2. 设置`classer` 编译文件目录
 >  `提示`：**WEB-INF**  默认没有**classes**目录，手动在 WEB-INF 目录中添加 classes 文件夹

 3.  把`classes`文件夹设置为`Excluded`
 
 <!--more-->
- - -
#### 设置`Paths`
 ![path设置|center](/images/2017-03-27/02.png)
 
 设置部署输出路径、路径为`WEB-INF`下新建的`classes`目录
 - - - 

#### 添加需要部署的`lib`
![lib|center](/images/2017-03-27/03.png)

> 选中 `Dependencies` 右边添加`lib`,出现一个下拉框、选择`java`

会弹出现有的`lib`、选择 `Tomcat 7.0.23`
![Tomcatlib|center](/images/2017-03-27/04.png)
- - - 
### 添加 Tomcat
IDEA 右上角点击图片红圈位置

![TomcatEdit|center](/images/2017-03-27/05.png)
- - -

##### 添加一个本地Tomcat 
![addLocalTomcat|center](/images/2017-03-27/06.png)
- - -

##### 配置Tomcat信息
![@图七|center](/images/2017-03-27/07.png)

1. 给这个Tomcat 取个名字吧
2. 选择Tomcat目录
3. 启动完Tomcat之后会自动用选定的浏览器打开项目
4. 给Tomcat一个访问地址
5. 选择JDK
6. Tomcat 端口 *默认即可*

- - - 
##### 添加部署项目名
![添加部署项目名|center](/images/2017-03-27/08.png)

项目部署名浏览器访问项目的名字

与`图七`中第`4`点搭配使用，如改图片的`项目部署名`是`/`，那么访问地址是：
>  http://localhost:8081/

假如把`项目部署名`更改为`hello`,那么访问地址应该是：
> http://localhost:8081/hello

- - - 

#### 启动Tomcat
点击Tomact小绿点、启动Tomcat<br/>
![StartTomcat|center](/images/2017-03-27/09.png)

启动成功：
![DeploySuccess|center](/images/2017-03-27/10.png)
- - -
#### 浏览器访问
输入地址：`http://localhost:8081/`
> **提示**：如果你`图七`中第`3`个步骤勾选了、浏览器会自动打开这个地址，否则需要手动输入地址！


- - - 

效果图：

![](/images/2017-03-27/11.png)
