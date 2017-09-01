---
layout: post
title:  Ubuntu如何安装JDK ？
categories: JAVA
description: Ubuntu如何安装JDK,Ubuntu安装java
---

* content
{:toc}


## JDK 是什么
JDK 是Java开发工具包 (Java Development Kit ) 的缩写。它是一种用于构建在 Java 平台上发布的应用程序、applet 和组件的开发环境。其中包括了Java编译器、JVM、大量的Java工具以及Java基础API里面是Java类库和Java的语言规范，同时Java语言的任何改进都应当加到其中，作为后续版本发布。要成为一名JAVA程序员，JDK是一种最基本的工具。
<!--more-->
## 获取JDK
> 本地安装是 `JDK1.7 64位`,Ubuntu 操作系统也是64位、请下载对应位数系统软件.

* 通过官网下载安装包到本地、然后安装。
* 通过 `wget` 方式、直接下载。
* 通过第三方平台获取。
* 通过Ubuntu工具安装：`apt-get install default-jdk`

我这已经上传到百度云盘，可通过百度云盘下载:
[https://pan.baidu.com/s/1nvuO9lR](https://pan.baidu.com/s/1nvuO9lR)

## 安装JDK
我这通过 `wget` 方式安装，网卡的可通过上面`第二种`方法
> wget http://download.oracle.com/otn/java/jdk/7u80-b15/jdk-7u80-linux-x64.tar.gz

## 配置环境变量
* 修改 `/etc/profile`
> vim /etc/profile
> 提示 : 需要使用 `ROOT` 权限操作
* 添加变量
```
   export JAVA_HOME=/usr/local/jdk1.7.0_79 
   export JRE_HOME=${JAVA_HOME}/jre  
   export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
   export PATH=${JAVA_HOME}/bin:$PATH
```
> JAVA_HOME 是你jdk目录、此处你只要修改一个这路径即可，其它可不动。

* 配置立即生效
 > sources /etc/profile

* 检验是否成功
> 输入java -version 出现如下代码表示安装成功

```
root@48c403ddc825:/usr/local/jdk1.7.0_79# java -version
java version "1.7.0_79"
Java(TM) SE Runtime Environment (build 1.7.0_79-b15)
Java HotSpot(TM) 64-Bit Server VM (build 24.79-b02, mixed mode)
```

至此、JDK安装完毕！