---
layout: post
title:  "Node.js入门-7-数据库MongoDB"
categories: Node
date:   2018-07-26 11:48:05
author: Logan
tags:  MongoDB
---

* content
{:toc}

# 一、 数据库

## 1. 基本概念

数据库就是按照一定的数据结构来组织、存储和管理数据的仓库

> 我们写的程序都是在内存中运行的，一旦程序运行结束或者计算机断电，内存中的数据都会全部丢失。所以我们就需要将一些程序运行的数据持久化到硬盘之中，以确保数据的安全性。

数据库是大批量数据持久化的普遍选择。**优点**如下：

- 1、数据库是有结构的，数据与数据之间可以建立各种关系，类似网状拓扑图。
- 2、数据库能够提供各种接口，让数据的处理（增删改查操作）变得快捷简单。
- 3、给各种语言（PHP、jsp、.net等）提供了完善的接口





## 2. 数据库分类

### 第一类：关系型数据库（RDBMS）

关系型数据库通过一张张表来建立关联

基本上都是用SQL(structure query language)语言来管理数据库

### 第二类：非关系型数据库（NoSQL）

**Not only SQL**

- 没有行、列的概念；用JSON来存储数据。集合就相当于“表”，文档就相当于“行”。
- 关系型数据库和非关系型数据库就是标准化和非标准化的摩擦，标准化限制创新，非标准化不能统一。

**分类**

- 键值(Key-Value)存储数据库
- 列存储数据库
- 文档型数据库
- 图形(Graph)数据库

### 区分

- 关系型数据库比较结构化，操作不是很灵活
- 非关系型数据库操作灵活，但是不适合大型数据存储，比较适合微架构
- 两者是相互辅助的关系

**非关系型数据库的适用场景**

- 1、数据模型比较简单
- 2、需要灵活性更强的后台系统
- 3、对数据库性能要求较高
- 4、不需要高度的数据一致性

# 二、MongoDB

## 1. 简介

- MongoDB是为快速开发互联网Web应用而设计的数据库系统。
- MongoDB的设计目标是极简、灵活，经常在Web应用栈的业务层被运用。
- MongoDB的数据模型是面向文档的，类似于JSON的结构。MongoDB这个数据库中存的是各种各样的BSON

## 2. 安装

下载地址 ：[https://www.mongodb.com/download-center?jmp=nav#community](https://www.mongodb.com/download-center?jmp=nav#community)

> MongoDB的偶数版本为稳定版，奇数版本为开发版<br/>
> MongoDB对于32位系统支持不佳，所以3.2版本以后没有再对32位系统的支持

**第一步** ： 安装MongoDB的数据库服务器

**第二步** ： 配置环境变量

- 我的电脑 -> 属性 -> 高级系统设置 -> 高级 -> 环境变量 -> 系统变量 -> Path -> 编辑 -> 新建
- 输入MongoDB安装目录，保存。例如`C:\Program Files\MongoDB\Server\4.0\bin`
- 确定、保存

**第三步** ： 在C盘根目录创建一个文件夹data，在data文件夹中创建一个文件夹db

**第四步** ： 打开命令行，输入`mongod`，启动MongoDB服务器

  > 32位的系统第一次启动，需要输入`mongod --storageEngine=mmapv1`

**第五步** ： 指定端口和路径
  - 1、在控制台启动MongoDB
  - 2、`mongod --dbpath 路径 --port 端口号`，例如 `mongod --dbpath C:\Users\lzh\data\db --port 123`

**第六步** ： 将MongoDB设置为系统服务

- 1、新建文件夹 `mkdir C:\data\log`
- 2、创建配置文件 `C:\Program Files\MongoDB\Server\4.0\mongod.cfg`
- 3、以管理员的身份打开命令行，在命令行内执行

  ```cmd
  sc.exe create MongoDB binPath="\"C:\Program Files\MongoDB\Server\4.0\bin\mongod.exe\" --service --config=\"C:\Program Files\MongoDB\Server\4.0\mongod.cfg\"" DisplayName="MongoDB" start="auto"
  ```

- 4、打开任务管理器 --> 服务 --> 启动MongoDB服务
- 5、如果服务启动失败，证明上面的操作有误

  - 控制台输入：`sc delete MongoDB` 删除之前配置的服务
  - 重新再来一次

## 3. MongoDB的使用

### 3.1 mongoDB的基本组成

- 1、数据库（database）：数据库是一个仓库，在仓库中可以存放集合
- 2、集合（collection）：集合类似于数组，在集合中可以存放文档。
- 3、文档（document）：文档数据库中的**最小单位**，我们存储和操作的内容都是文档。

### 3.2 mongoDB的基本指令

- `show dbs` : 显示当前所有的数据库
- `use <数据库名>` : 进入到指定数据库中（不存在则创建数据库）
- `db` : 显示当前所在的数据库
- `show collections` : 显示数据库中的所有集合

### 3.3 命令行进行CRUD(Create,Read,Update,Delete)

#### a. 插入文档

- `db.collection.insertOne()`

  ```
  db.student.insertOne({...})
  ```

- `db.collection.insertMany()`

  ```
  db.student.insertMany([
    {...},
    {...},
    {...}
  ])
  ```

- `db.collection.insert()`

  ```
  db.student.insert({...})
  db.student.insert([
    {...},
    {...},
    {...}
  ])
  ```

#### b. 查询文档

- `{ <field1>: { <operator1>: <value1> }, ... }`
- `in`运算符：值在集合中

  ```
  db.inventory.find( { status: { $in: [ "A", "D" ] } } )
  ```

  > 表示同一字段的可能值时，使用in运算符而不是or运算符

- `lt`运算符：值小于给定值

  ```
  db.inventory.find( { status: "A", qty: { $lt: 30 } } )
  ```

- `or`运算符：值至少与其中一个匹配

  ```
  db.inventory.find( { $or: [ { status: "A" }, { qty: { $lt: 30 } } ] } )
  ```

- 多个运算符结合使用

  ```
  db.inventory.find({
    status: {$in: ["A", "B"]},
    $or: [{qty: {$lt: 30}}, {item: /^p/}]
  })
  ```

#### c. 更新文档

- `db.collection.updateOne(<filter>, <update>, <options>)` : 更新`<filter>`匹配到的**首个**文档

  ```
  // <update>部分使用更新操作符来修改字段和值
  {
    <update operator>: { <field1>: <value1>, ... },
    <update operator>: { <field2>: <value2>, ... },
    ...
  }
  // For example
  db.inventory.updateOne(
    { item: "paper" },
    {
      $set: { "size.uom": "cm", status: "P" },
      $currentDate: { lastModified: true }
    }
  )
  ```

- `db.collection.updateMany(<filter>, <update>, <options>)` : 更新`<filter>`匹配到的**所有**文档

  ```
  // For example
  db.inventory.updateOne(
    { item: "paper" },
    {
      $set: { "size.uom": "cm", status: "P" },
      $currentDate: { lastModified: true }
    }
  )
  ```

- `db.collection.update(<filter>, <update>, <options>)` : 更新`<filter>`匹配到的文档

  ```
  // For example
  db.inventory.updateOne(
    { item: "paper" },
    {
      $set: { "size.uom": "cm", status: "P" },
      $currentDate: { lastModified: true }
    },
    {multi: true}
  )
  ```

  > 不设置`multi`参数则默认只更新匹配到的第一个文档，等同于`updateOne` <br/>
  > `multi`参数设置为`true`后，则修改所有匹配到的文档，等同于`updateMany`

- `db.collection.replaceOne(<filter>, <replacement>, <options>)` : 替换`<filter>`匹配到的**首个**文档

  ```
  // For example
  db.inventory.replaceOne(
    { item: "paper" },
    { item: "paper", instock: [ { warehouse: "A", qty: 60 }, { warehouse: "B", qty: 40 } ] }
  )
  ```

  > 替换文档的内容仅包含 字段&值 对，即不包含更新运算符表达式。<br/>
  > 替换文档中不必包含`_id`字段，如果包含的话，必须与替换前的一致。

#### d. 删除文档

- `db.collection.deleteMany(<filter>)` ：删除所有匹配到的文档

  ```
  // 要删除集合内所有文档，筛选参数传入空对象即可
  db.student.deleteMany({})

  // 要删除所有匹配到的文档，需传入筛选参数，参考查询文档时的筛选参数
  db.student.deleteMany({
    status: {$in: ["a", "b"]}
  })
  ```

- `db.collection.deleteOne(<filter>)` ：删除第一个匹配到的文档

  ```
  // 要删除所有匹配到的文档，需传入筛选参数，参考查询文档时的筛选参数
  db.student.deleteOne({
    status: {$in: ["a", "b"]}
  })
  ```

### 3.4 基础练习

```cmd
// 1. 创建并进入test数据库
mongo
use test
// 2. 想数据库的colleges集合中插入六个文档（Html5、Java、Python、区块链、K12、<PHP， “世界上最好的编程语言”>）
db.colleges.insertMany([
  
])

> 使用命令行进行CRUD非常麻烦，可以运用可视化工具进行操作

1. 安装可视化操作软件

mongodbmanagerpro下载地址: [https://www.mongodbmanager.com/download](https://www.mongodbmanager.com/download)