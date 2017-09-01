---
layout: post
title:  ubuntu 如何安装 gitblit？
categories: JAVA
tags: gitblit
description: ubuntu 如何安装 gitblit
---

* content
{:toc}


## 什么是gitblit
Gitblit是一个用于管理，查看和提供Git存储库的开源纯Java堆栈。 它主要设计为希望托管集中式存储库的小型工作组的工具。
> 作用：把它当作是Github网站使用.

## 获取gitblit
官网：[http://gitblit.com](http://gitblit.com)
*  通过官网下载安装包
*  在线安装、通过 `wget` 方法

目前最新版本：
Current Release 1.8.0 (2016-06-22)
下载地址：[http://dl.bintray.com/gitblit/releases/gitblit-1.8.0.tar.gz](http://dl.bintray.com/gitblit/releases/gitblit-1.8.0.tar.gz)

<!--more-->

## 安装过程
> 由于 gitblit 是基于java运行，所以必须要有java环境，如何安装java、请参考：
[http://liujilu.com/2017/04/25/Ubuntu-Install-JDK/](http://liujilu.com/2017/04/25/Ubuntu-Install-JDK/)

本次使用 `wget` 方式安装、命令如下：
> wget http://dl.bintray.com/gitblit/releases/gitblit-1.8.0.tar.gz

解压`gitblit-1.8.0.tar.gz` 进入其目录
> tar -zxvf gitblit-1.8.0.tar.gz

安装目录最好是消除空格和中文。

## 修改配置
修改 `gitblit-1.8.0/data/defaults.properties` 文件
常用参数说明：
> `git.sshPort` :ssh更新代码端口，默认为：`29418`  
> `server.httpPort` :http端口、供网页访问仓库,默认为：`0`  
> `server.httpsPort`:https端口、供网页访问仓库,默认为：`8443`  
> `git.packedGitLimit`:设置大于最大存储库的大小,默认为：`10m`


## 启动gitblit
* 通过java命令启动gitblit：
> java -jar gitblit.jar --baseFolder data  

* 直接启动gitblit.sh 执行文件
> sh gitblit.sh

如果出现如图所示、那就是代表启动成功：
![](/images/2017-04-25/01.jpg)

通过网页访问：

我这设置`server.httpPort`为：`8446`, 那么网页访问地址为：[http://ip:8446](http://ip:8446)  
进入首页、默认帐号密码：`admin\admin`
> 注意：确保更改管理员用户名/密码！

![](/images/2017-04-25/02.jpg)

至此、就完成了gitblit.sh 操作！
完全可以把这个当作github使用！