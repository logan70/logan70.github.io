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

# 移动WEB的基础知识

## pixels 像素基础

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

- layout viewport ： 布局 viewport ，在底层进行页面渲染布局。
	>不是原页面的大小，如ios默认viewport是980px。
- visual viewport ： 度量/视口 viewport ，进行窗口缩放`scale`，展示页面内容。

**为什么不使用默认的980px的布局viewport**

- 宽度不可控制，不同系统不同设备的默认值都可能不同。
- 页面缩小显示，交互不友好。
- 链接不可点。
- 有缩放，缩放后又有滚动。
>简单来说就是整个页面的设计都是不规范，用户交互不友好。

## Viewport - meta标签

**标签代码**

```html
<meta name="viewport" content="name=value, name=value">
```

**Content内可配置参数**

- width: 设置布局viewport的特定值("device-width")
	>width=device-width 是为了让**layout viewport**(布局viewport)等于**设备的尺寸**。
- initial-scale: 设置页面的初始缩放
	>initial-scale=1 是为了让**visual viewport**(度量viewport)等于**layout viewport**(布局viewport)。
- minimum-scale: 最小缩放
- maximum-scale: 最大缩放
- user-scalable: 用户可否缩放(no)

**移动WEB最佳Viewport设置**

- 布局viewport=设备宽度=度量viewport

- 代码如下
	```html
	<meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no">
	```

## Viewport - coding

**设计移动WEB**

- 方案1
	>根据设备的实际宽度来设计(常用)即布局viewport=设备宽度=度量viewport

	>手机是320px，就拿320px来设计

- 方案2
	>1px=1dp

	>缩放0.5。根据设备的物理像素dp等于抽象像素px来设计。1像素边框和高清图片都不需要额外处理。

***

# 高效的移动WEB布局

## Flexbox弹性盒子布局

**CSS属性设置**

1. 父元素设置display属性，表示使用弹性布局。
2. 子元素设置flex属性，表示占容器的比例
	>![Flexbox](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-02/flex.jpg "flex代码")

**flex示例**

- Flex等比划分
	>![Flexbox](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-02/Flex-1.jpg "flex等比划分")
- Flex混合划分
	>![Flexbox](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-02/flexboxmix.jpg "flex混合划分")
- CSS3不定宽高的水平垂直居中
	>![Flexbox](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-02/css3-center.jpg "CSS3不定宽高的水平垂直居中")
- Flex不定宽高的水平垂直居中
	>![Flexbox](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-02/flex-center.jpg "Flex不定宽高的水平垂直居中")
- Flex弹性盒模型
	>![Flexbox](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-02/flex-model.jpg "Flex弹性盒模型")
- Flex弹性盒模型完整版
![Flexbox](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-02/flex-model-all.bmp "Flex弹性盒模型完整版")

## 响应式设计

**Flexbox弹性盒子布局的兼容性**

- iOS可以使用最新的flex布局
- 安卓4.4以下，只能兼容旧版本的flexbox布局
- 安卓4.4及以上，可以使用最新的flex布局

**新旧flex版本属性替换**

![Flexbox](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-02/flex-version.jpg "新旧flex版本属性替换")

**媒体查询**

![响应式设计](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-02/responsive-media.jpg "响应式设计媒体查询")

**媒体查询类型及参数**

![响应式设计](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-02/responsive-media-type.jpg "响应式设计媒体查询类型及参数")

## 移动Web特别样式处理

**高清图片**

![特别样式处理](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-02/style-img.jpg "特别样式处理之高清图片")

**一像素边框**

![特别样式处理](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-02/style-border.jpg "特别样式处理-一像素边框")

**相对单位rem**

>根据html的font-size为相对单位

![特别样式处理](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-02/style-rem.jpg "特别样式处理-相对单位rem")

>一般来讲font-size是不应该使用rem等相对单位的。因为字体的大小是趋于阅读的实用性，并不适合排版布局。
>同理趋向于一些固定的元素的特性。我们不使用rem而是使用px去确保在不同屏幕上的表现一致(跟使用rem的目的相反)

**多行文本溢出**

- 单行文本溢出
	```html
	.inaline{
		overflow: hidden;
		white-space: nowrap;
		text-overflow: ellipsis;
	}
	```

- 多行文本溢出
	```html
	.inmorelines{
		display: -webkit-box !important;
		overflow: hidden;

		text-overflow: ellipsis;
		word-break: break-all;

		-webkit-box-orient: vertical;
		-webkit-line-clamp: 2;
	}

#终端交互优化

## Tap基础事件

**300ms的故事**

![Tap基础事件](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-02/mobile-300ms.jpg "Tap基础事件-300ms延迟")

**Tap基础事件**

![Tap基础事件](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-02/mobile-tap.jpg "Tap基础事件-自定义Tap事件原理")

**Tap“点透bug”**

点击遮罩层，会触发下面的按钮

![Tap基础事件](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-02/mobile-bug.jpg "Tap基础事件-Tap“点透bug”")

**Tap透传的解决方案**

![Tap基础事件](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-02/mobile-bug-fix.jpg "Tap基础事件-Tap透传的解决方案")

## Touch基础事件

**触摸Touch**

![Touch基础事件](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-02/mobile-touch.jpg "Touch基础事件")

**每个touch对象包含的属性**

![Touch基础事件](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-02/mobile-touch-object.jpg "Touch基础事件-每个touch对象包含的属性")

**Android的bug**

![Touch基础事件](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-02/mobile-android-bug.jpg "Touch基础事件-Android的bug")

**弹性滚动**

![Touch基础事件](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-02/mobile-scroll1.jpg "Touch基础事件-弹性滚动1")

![Touch基础事件](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-02/mobile-scroll2.jpg "Touch基础事件-弹性滚动2")

**下拉刷新**

>顶端下拉一小点距离，页面弹性滚动向下

**上拉加载**

>使用scroll事件，而不是touch事件(Android有bug)

# 课程总结

![课程总结](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-02/summary.jpg "课程总结")




