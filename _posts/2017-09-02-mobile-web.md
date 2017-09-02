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

**像素概念**

>- **px:** css pixels 逻辑像素，浏览器使用的抽象单位
- **dp/dt:** device independent pixels 设备无关的像素
- **dpr:** devicePixelsRatio 设备像素缩放比
- **计算公式:** 1px = (dpr)<sup>2</sup> * dp
- iphone5分辨率：640dp * 1136dp
为什么iphone5是320px * 568px？
iphone5的drp=2，1px=4dp，所以长度上1px=2dp，640dp => 320px；1136dp => 568px

**像素密度概念**

>- **PPI:** 屏幕每英寸的像素数量，即单位英寸内的像素密度
- **DPI:** 打印机每英寸可以喷的墨汁点
- **计算公式:** 以iPhone5为例：√(1336<sup>2</sup>+640<sup>2</sup>)/4=326ppi

**像素密度分级**

|  /  |ldpi  |mdpi  |hdpi  |xhdpi  |
|-----|------|------|------|-------|
|ppi  |120   |160   |240   |320    |
|默认缩放比|0.75|1.0|1.5   |2.0    |

>**Retina(高清屏)**dpr都大于等于2.

**以iPhone5为例子的流程图**

```flow
设备分辨率1136*640 dp => √(1336<sup>2</sup>+640<sup>2</sup>)/4=326ppi
=> 326ppi属于Retina屏幕,dpr=2 => 1px = (dpr)<sup>2</sup> * dp
=> iPhone5的屏幕为320*568px
```

## Viewport视图

**手机浏览器默认为我们做了两件事情**

1. 页面渲染在一个980px(iox)的Viewport里
2. 缩放
	>渲染在Viewport里是为了排版正确

**两个Viewport**

- layout viewport ： 布局 viewpoint ，在底层进行页面渲染布局。
	>不是原页面的大小，如ios默认viewpoint是980px。
- visual viewpoint ： 度量/视口 viewpoint ，进行窗口缩放`scale`，展示页面内容。

**为什么不使用默认的980px的布局viewpoint

- 宽度不可控制，不同系统不同设备的默认值都可能不同。
- 页面缩小显示，交互不友好。
- 链接不可点。
- 有缩放，缩放后又有滚动。

>简单来说就是整个页面的设计都是不规范，用户交互不友好。