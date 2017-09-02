---
layout: post
title:  "Hello!移动WEB 学习笔记"
date:   2017-09-02 10:44:05
categories: Mobile
author: Logan
tags:  Mobile
---

* content
{:toc}


## pixels 像素基础

***


>- **px:** css pixels 逻辑像素，浏览器使用的抽象单位
- **dp/dt:** device independent pixels 设备无关的像素
- **dpr:** devicePixelsRatio 设备像素缩放比
- **计算公式:** 1px = (dpr)<sup>2</sup> * dp
- iphone5分辨率：640dp*1136dp
为什么iphone5是320px*568px？
iphone5的drp=2，1px=4dp，所以长度上1px=2dp，640dp => 320px；1136dp => 568px

***

>- **PPI:** 屏幕每英寸的像素数量，即单位英寸内的像素密度
- **DPI:** 打印机每英寸可以喷的墨汁点
- **计算公式:** 以iPhone5为例：√(1336<sup>2</sup>+640<sup>2</sup>)/4=326ppi

>|  /  |ldpi  |mdpi  |hdpi  |xhdpi  |
>|-----|------|------|------|-------|
>|ppi  |120   |160   |240   |320    |
>|默认缩放比|0.75|1.0|1.5   |2.0    |
>**Retina(高清屏)**dpr都大于等于2.

以iPhone5为例子的流程图

```flow
设备分辨率1136*640 dp => √(1336<sup>2</sup>+640<sup>2</sup>)/4=326ppi
=> 326ppi属于Retina屏幕,dpr=2 => 1px = (dpr)<sup>2</sup> * dp
=> iPhone5的屏幕为320*568px
```