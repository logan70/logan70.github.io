---
layout: post
title:  "Linux安装Redis服务"
categories: JAVA
tags: redis
description: Linux安装Redis服务，设置Redis远程访问操作
---

* content
{:toc}


 &nbsp; &nbsp;通常来说，生产环境下的**Redis**服务器只设置为仅本机访问（**Redis**默认也只允许本机访问 ）。有时候我们也许需要使Redi能被远程访问。

#### 安装Redis服务
@(Linux)[Ubuntu,Centos]
- Ubuntu
> apt-get install redis-server

- Centos
> yum install redis-server

安装完成后系统会自动启动Redis服务

<!--more-->

#### 配置Redis服务
Redis 默认只能本机连接、不能远程连接，通过修改配置可远程连接**Redis**服务

修改**Redis**配置文件/etc/redis/redis.conf，找到**bind**那行配置：
> bind 127.0.0.1

修改为：
> bind 0.0.0.0

关于bind配置的含义，配置文件里的注释是这样说的：

> `#` By default Redis listens for connections from all the network interfaces<br/>
> `#` available on the server. It is possible to listen to just one or multiple<br/>
> `#` interfaces using the "bind" configuration directive, followed by one or<br/>
> `#` more IP addresses.<br/>
> `#`<br/>
> `#` Examples:<br/>
> `#`<br/>
> `#` bind 192.168.1.100 10.0.0.1<br/>

大概意思是说Redis端口侦听所有的网络接口连接，服务器上可以只听一个或者多个，使用`bind` 指定命令。

#### 远程连接Redis
配置好Redis服务并重启服务后。就可以使用客户端远程连接**Redis**服务了。命令格式如下：
> redis-cli -h {redis_host} -p {redis_port}

`{redis_host}`  远程Redis服务所在的服务器地址，`{redis_port}` Redis 服务端口(默认 : *6379*)

例如：
> redis-cli -h 192.168.1.100 -p 6379<br/>
> 192.168.1.100:6379> ping<br/>
>PONG

`ping` 测试是否连接成功、如果出现**PONG**，就代表连接成功！

接下来就尽情的操作Redis吧！
