---
layout: post
title:  "通过数据库自动反向生成MyBatis文件"
categories: JAVA
tags: ssm
---

* content
{:toc}




做一个项目、如果选择MyBatis框架的话，少不了写xml、dao、model。
Mybatis虽好、但是表多了的话、就要重复写xml、dao、model，对于程序猿来说这工作完全就是`ctrl + c `,`ctrl + V` ，在这方面的操作就浪费了大把时间。

> 于是出现了自动生成工具，这工具会通过数据库自动反向生成对应的`dao`,`model`,`xml`，能够大大提示开发效率。

下载地址：[https://github.com/Jandaes/mybatis-mapper/tree/master](https://github.com/Jandaes/mybatis-mapper/tree/master)

<!--more-->

## 连接数据库
### 创建数据库
![@本地数据库|center](http://i1.piimg.com/567571/b0a2373817a6b8d8.jpg)

连接的本地数据库、demodb数据库有两个表


### 加载驱动jar
支持大部分关系型数据库、只要加载对应的驱动就行，换成本地jar的路径
`<classPathEntry location="D:\xxx\mysql-connector-java-5.1.38.jar" />`

### 设置数据库连接
driverClass ：不同数据库不同驱动
connectionURL：数据库连接地址
userId：数据库用户名
password：数据库密码
`<jdbcConnection driverClass="xx" connectionURL="xxx" userId="xxx" password="xxx">`
		
## 生成模型[model]的包名和位置
targetPackage：在项目中的包名
targetProject：存在本地的路径
`<javaModelGenerator targetPackage="xxx" targetProject="xxx">`

## 生成的配置文件[xml]包名和位置
targetPackage：在项目中的包名
targetProject：存在本地的路径
`<sqlMapGenerator targetPackage="xxx" targetProject="xxx">`

## 生成接口[dao]的包名和位置
targetPackage：在项目中的包名
targetProject：存在本地的路径
`<javaClientGenerator type="XMLMAPPER" targetPackage="coxxx" targetProject="xxx">`

## 要生成哪些表
我数据库只有两个表、只要写两个配置就行，如果有表关联，也会自动生成关联.
tableName：表名
domainObjectName：在项目中Model类名

只要更改这两个就可以了
`<table tableName="scene" domainObjectName="Scene" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false" />`

## 执行生成
进入工具根目录、根目录有一个执行jar、以及一个txt说明文本
>如果dos路径是在工具根目录可直接执行当前命令，否则需要输入jar绝对路径！

`java -jar mybatis-generator-core-1.3.2.jar -configfile generator.xml -overwrite`

结果：
![@生成成功|center](http://p1.bqimg.com/567571/1d998300e6cb45e8.jpg)


> 我打包的generator.xml 、内置我默认配置，可改成你对应的配置就行！

**大功告成！**
