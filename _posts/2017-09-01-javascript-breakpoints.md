---
layout: post
title:  "调试Javascript-断点设置"
date:   2017-09-01 23:44:05
categories: Javascript
author: Logan
tags:  Javascript Breakpoint
---

* content
{:toc}


## 如何设置断点

***

使用断点来暂停`JavaScript`代码，审查变量的值和在特定时刻所调用的堆栈。

一旦你设置了断点，你应该学习如何遍历你的代码，并审查你的变量和调用堆栈。

- 设置断点的最基本的方法是在特定的代码行上手动添加一个断点。您也可以将这些断点配置为仅在满足特定条件时触发。

- 您还可以设置在满足一般条件时触发的断点，例如事件，DOM更改或未捕获异常。




## 在代码特定行上设置断点

***

要在特定代码行上设置断点，首先打开`Sources`(源文件)面板，并在左侧的`File Navigator`(文件导航器)窗格中选择该脚本。如果找不到`File Navigator`(文件导航器)，按下`Toggle file navigator`(切换文件导航器)按钮（隐藏/显示文件导航器按钮）。

**提示**: 如果你使用缩略的代码，点击`pretty print`(代码美化)按钮`{}`使其可读。

在源代码的左侧，您可以看到行号。这个区域称为`line number gutter`(行号槽)。单击行号槽中的行号，就会在该行代码上添加一个断点。

**提示**: 如果一个表达式占了多行，并且你把一个行断点放在这个表达式的中间，`DevTools`会在下一个表达式上设置断点。

### 使一个行号断点有条件

右键单击尚未设置断点的行号，然后点击`Add conditional breakpoint`(添加条件断点)来创建一个条件断点。如果你已经在一行代码上添加了断点，并且希望使断点有条件，右键单击该断点，并点击`Edit breakpoint`(编辑断点)。

在文本字段中输入你的条件，并按 `Enter` 键。

### 删除或禁用一个行号断点

如果你想临时忽略一个断点，然后**禁用**它。右键单击`line number gutter`(行号槽)中该断点，并选择`Disable breakpoint`(禁用断点)。

如果你不再需要一个断点，然后**删除**它。右键单击`line number gutter`(行号槽)中该断点，并选择`Remove breakpoint`(删除断点)。

您还可以在`Sources`(源文件)面板上的`Breakpoints`(断点)窗格中**管理**所有脚本中的所有行号断点。

要从`Breakpoints`(断点)窗格界面中**删除**断点，右键单击该断点，并选择`Remove breakpoint`(删除断点)。

要从此窗格中**禁用**断点，请取消勾选其复选框。

要**禁用所有**断点，右键单击该窗格,并选择`Deactivate breakpoints`(停用断点)。这个产生的效果与`Disable All Breakpoints`(禁用所有断点)选项是相同的。

您也可以在`Sources`(源文件)面板上,通过点击`Deactivate breakpoints`(停用断点)按钮(停用断点按钮)来**禁用所有断点**。

## 设置监测DOM变化的断点

***

### 添加DOM change breakpoints(DOM变化断点)


1. 在需要检测的DOM元素上右键单击，然后选择`Inspect`(检查)。`DevTools`将这个节点突出显示为蓝色。您可以通过双击它来展开节点，以便您可以查看其内容。这样可以验证你在正确的节点上。

2. 右键单击突出显示的节点，然后选择`Break on>Subtree Modifications`(子树修改)。节点左侧的蓝色图标 DOM断点图标 表示在该节点上设置了DOM断点。

3. `DevTools`监测到变化时会暂停该页面，转到`Sources`(源文件)面板，并突出显示脚本中导致更改的代码行。

4. 点击`Resume script execution`(恢复脚本执行)按钮，可以恢复脚本执行。

要想**暂时关闭**这个断点：

1. 在`DevTools`中回到`Elements`(元素)面板。

2. 单击`DOM Breakpoints`(DOM断点)窗格。

3. 取消勾选`Subtree Modifications`(子树修改)旁边的**复选框**。


要想**删除**这个断点：

1. 转到`DOM Breakpoints`(DOM断点)窗格。

2. 右键单击要删除的断点，然后选择`Remove breakpoint`(删除断点)。

### 更多关于DOM change breakpoints(DOM变化断点)类型

以下是有关每种类型的`DOM change breakpoints`(DOM变化断点)具体触发时间和触发方式的详细信息：

1. `Subtree Modifications`(子树修改) - 当当前选定节点的子节点被删除，添加或子节点的内容发生更改时触发。当子节点属性改变时，或当前选择的节点发生任何改变，都不会触发该类型的断点。

2. `Attributes modifications`(属性修改) - 当在当前选定的节点上添加或删除属性时,或当属性值改变时触发。

3. `Node Removal`(节点删除) -当当前选定的节点被删除时触发。

## 在XHR上中断

有两种方法可以触发`XHR`上的断点：当任何`XHR`到达`XHR`生命周期的某个阶段时（`readystatechange`，`load`等），或者当`XHR`的`URL`与某个字符串匹配时。

如果你想在`XHR`生命周期的某个阶段时中断，请在事件侦听器断点窗格中查看`XHR`目录。

要在`XHR`的`URL`与某个字符串匹配时中断，请使用`Sources`(源文件)面板上的`XHR Breakpoints`(XHR断点)窗格。

点击“+”(加号)按钮添加一个新的断点模式。在文本字段中输入你的字符串，并按`Enter`键保存。

提示：点击“+”(加号)，然后立即按`Enter`键，可以在发送任何`XHR`之前触发断点。

## 当一个事件触发时中断

***

当某事件（例如，`click`(点击)）或事件类别（例如，任何`mouse`(鼠标)事件）被触发时，使用`Sources`(源文件)面板上的`Event Listener Breakpoints`(事件侦听器断点)窗格中断。

## 在未捕获的异常上中断

***

如果你的代码抛出异常，你不知道他们来自哪里，点击`Sources`(源文件)面板上的`pause on exception`(在异常上暂停)按钮（在异常上暂停按钮）。

`DevTools`在抛出异常的行自动中断。
