---
layout: post
title:  java中文乱码常见解决方式
categories: java
tags: 编码
description: java常见编码设置
excerpt: 本文记录java乱码、常见解决方案
---

* content
{:toc}

## 说明
  项目出现中文乱码现象、常见编码解决方法如下。
## 项目乱码
### 项目工作空间
在 `Windows -> Prefenrences -> General -> Workspace` 中进行设置
> 在创建项目工作空间的时候、优先设置编码，在该工作空间下创建的项目默认遵循工作框架配置

### 项目编码
在 `Project -> Resource`中设置
> 创建项目的时候、设置编码，则项目下文件都将会和项目统一

### 页面文件编码
文件右键 `Properties -> Resource`

### 文件头编码
文件头一般是HTML、JSP标签头部添加编码  
JSP：  
> <%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
 
HTML：
> 添加在`<head>`标签里面  
> `<meta http-equiv="Content-Type" content="text/html; charset=utf-8"> `

## 编辑器编码设置
### NotePad++
编辑器打开一个文件时候乱码
在 `菜单 -> 格式`

### 记事本
存储时，保存为`UTF-8`格式
 
## 服务器乱码
### SpringMVC
在`web.xml`添加
```java
<filter>
	<description>字符集过滤器</description>
	<filter-name>encodingFilter</filter-name>
	<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
	<init-param>
		<description>字符集编码</description>
		<param-name>encoding</param-name>
		<param-value>UTF-8</param-value>
	</init-param>
</filter>
<filter-mapping>
	<filter-name>encodingFilter</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>
```
### Tomcat编码
Tomcat 的 `conf/server.conf`设置编码、设置为：
```xml
<Connector port="8080" protocol="HTTP/1.1"
	connectionTimeout="20000" 
	URIEncoding="UTF-8"
	redirectPort="8443" />
```

添加：`URIEncoding="UTF-8"` 属性

### 请求响应编码
设置请求、响应编码
```java
//设置获取请求的编码
request.setCharacterEncoding("utf-8")
//设置服务器端的编码
response.setCharacterEncoding("utf-8");
//通知浏览器服务器发送的数据格式
response.setContentType("text/html;charset=utf-8");
```
### 字符串编码
```java
String oldStr = "编码设置";
String newStr = new String(oldStr.getBytes(), "UTF-8");  
System.out.println("UTF-8编码：" + newStr);
```
### JDBC 连接指定编码
```xml
url=jdbc:mysql://127.0.0.1/database?characterEncoding=UTF-8
```
### 数据库设置编码
编码可选：
```java
mysql> set character_set_client=utf8;
mysql> set character_set_connection=utf8;
mysql> set character_set_database=utf8;
mysql> set character_set_results=utf8;
mysql> set character_set_server=utf8;
mysql> set character_set_system=utf8;
mysql> set collation_connection=utf8;
mysql> set collation_database=utf8;
mysql> set collation_server=utf8;
```
### 数据库表设置编码
创建表的时候、指定编码：`DEFAULT CHARSET=UTF8;`
```sql
CREATE TABLE `type` ( 
	`id` int(10) unsigned NOT NULL auto_increment,
	`type_name` varchar(50) character set utf8 NOT NULL default '',
	PRIMARY KEY (`id`) 
)  DEFAULT CHARSET=UTF8; 
```

## 补充
如果出现乱码现象、可对应文章修改。  
更多编码设置、后续补充。