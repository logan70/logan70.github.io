---
layout: post
title:  "前端PS切图"
date:   2017-09-06 23:44:05
categories: Tools
author: Logan
tags:  Tools Photoshop
---

* content
{:toc}

# 快捷键

- 移动工具 `V`
- 选取工具 `M`
- 套索工具 `L`
- 魔棒工具 `W`
- 裁剪工具 `C`
- 吸管工具 `I`
- 移动 `Space + MouseMove`
- 合并拷贝图层 `Ctrl + Shift + C`
- 快速选择相应区域并局部放大 `H + MouseMove`
- 隐藏所有参考线 `Ctrl + H`
- 合并图层 `Ctrl + E`
- 前景色填充 `Alt + Delete`
- 背景色填充 `Ctrl + Delete`
- 自由变换 `Ctrl + T`
- 取消选区 `Ctrl + D`
- 盖印所选图层 `Ctrl + Alt + E`
- 盖印所有可见图层 `Ctrl + Alt + Shift + E`
	>盖印就是在你将处理图片的时候将处理后的效果盖印到新的图层上，功能和合并图层差不多，不过比合并图层更好用!因为盖印是重新生成一个新的图层而一点都不会影响你之前所处理的图层，这样做的好处就是，如果你觉得之前处理的效果不太满意，你可以删除盖印图层，之前做效果的图层依然还在。极大程度上方便我们处理图片，也可以节省时间。
- 打开/关闭标尺 `Ctrl+R`
- 存储为Web所用格式 `Ctrl + Alt + Shift + S`
- 反选 `Ctrl + Shift + I`




# Adobe Photoshop CC 2017 下载及破解

[Adobe Photoshop CC 2017 下载地址](https://pan.baidu.com/s/1jHAJXHW#list/path=%2F&parentPath=%2Fps%E4%B8%8B%E8%BD%BD%E5%9C%B0%E5%9D%80 "Adobe Photoshop CC 2017 下载地址")

[Adobe Photoshop CC 2017 破解器下载地址](https://down9.3987.com/2010/amtemulator.3987.com.rar "Adobe Photoshop CC 2017 破解器下载地址")

1. 进入**Adobe Photoshop CC 2017**下载地址并根据系统位数下载安装包
2. 下载完后解压，点击Setup.exe，根据提示步骤进行安装
3. 下载**Adobe Photoshop CC 2017破解器**并解压
4. 打开`amtemu.v0.9.2-painter.exe`并选择相应版本，点击`Install`(如果选择Adobe Photoshop CC 2017不行，就选择Adobe Photoshop CC 2015.5再试一次)
5. 选择安装目录,一般是`C:\Program Files\Adobe\Adobe Photoshop CC 2017`
6. 选择目录下载的amtlib.dll，点击打开来进行安装破解。如下图提示安装成功即说明破解成功
![Adobe Photoshop CC 2017](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-06/ps-setup.jpg "Adobe Photoshop CC 2017")


# 三种切图方法

## 图层切图

1. 选中图层或组
2. 然后右击图层，将图层转换为智能对象

	![PS切图](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-06/ps1.png "PS切图")

3. 选择选框工具，将你要切的图层圈起来

	![PS切图](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-06/ps2.png "PS切图")

4. 接着按`Ctrl+C`复制，再按`Ctrl+N`新建，注意背景颜色设置为透明

	![PS切图](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-06/ps3.png "PS切图")

5. 点击确定，再按`Ctrl + V`粘贴，我们就得到要切的图层了

	![PS切图](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-06/ps4.png "PS切图")

6. 再按`Ctrl + Alt + Shift + S`保存，记住背景图存为PNG24格式

	![PS切图](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-06/ps5.png "PS切图")

7. 存储到我们要存的文件夹下，就大功告成了

## 切片切图

1. 拉辅助线，如下

	![PS切图](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-06/ps6.png "PS切图")

2. 选择切片工具，将我们要切的所有图片区域，用切片工具选中

	![PS切图](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-06/ps7.png "PS切图")

	![PS切图](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-06/ps8.png "PS切图")

3. 按住`Ctrl + Alt + Shift + S`保存，保存的时候注意，保存为JPEG格式，选择为保存所有用户切片，这样子切出来的才是我们想要的图片

	![PS切图](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-06/ps9.png "PS切图")

4. 保存之后就完工了，切片切图的方法很方便，但是注意它只能切出形状规则的图片

## 自动切图（CC版本的PS才支持）

1. `Ctrl+K`出来首选项（编辑-->首选项），然后设置增效工具，勾选`启用生成器`

	![PS切图](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-06/ps10.png "PS切图")

2. 再设置 文件-->生成-->图像资源。 （这个设置了下次才生效，设置不上就重启ps）

	![PS切图](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-06/ps11.png "PS切图")

3. 在PSD内将图层或图层组的名称修改为`.png`或`.jpg`结尾，则在PSD文件同目录下的`assets`文件夹内自动生成导出图片（以图层名称及其约定的文件后缀名命名）

	![PS切图](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-06/ps12.png "PS切图")

	![PS切图](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-09-06/ps13.png "PS切图")

## 自动切图注意事项

- **Retina优化:**如果需要适配 retina 显示器，可以将图层命名为`200% 图层名字 @2x.后缀名`（注意需要添加空格，如：200% button @2x.jpg）自动导出的文件名不会出现200%的字样，但是文件尺寸变为2倍大小。
- 如果不想要导出的图片资源了，可以**取消图层和图层组的文件后缀名**，则导出的图片自动在assets文件夹内**消失**
- 还可以通过图层和图层组的文件后缀名来控制导出图片质量，例如`xxx.jpg8`则为导出品质为80%的jpg文件，如果不设置则默认为最佳品质
- **SVG生成：**改后缀名可自动生成SVG
- **复制CSS：**鼠标右键图层-->复制CSS