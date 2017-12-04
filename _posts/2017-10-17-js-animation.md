---
layout: post
title:  "JS 动画效果"
categories: Javascript
date:   2017-10-17 16:48:05
author: Logan
tags:  JSAnimation
---

* content
{:toc}

# JS 速度动画

```js
// 设置定时器，获取需要移动的元素
var timer = null,
	box = document.getElementById('box');
function moveAnimation(target) {
	// 函数开始清除定时器，防止定时器叠加
    clearInterval(timer);
    // 定义速度
    var speed = 0;
    // 根据当前位置与目标位置的大小关系来确定速度的正负值
    if (target > box.offsetLeft) {
        speed = 1;
    } else {
        speed = -1;
    }
    timer = setInterval(function(){
        if (box.offsetLeft == target) {
        	// 当运动到目标位置时清除定时器
            clearInterval(timer);
        } else {
        	// 设置位置的变化
            box.style.left = box.offsetLeft + speed + 'px';
        }
    },3)
}
```




# JS 透明度动画

实现方式与`速度动画`大致相同。

```js
// 设置定时器，获取需要移动的元素，设置透明度初始值
var timer = null,
	box = document.getElementById('box'),
	op = 30;
function moveAnimation(target) {
	// 函数开始清除定时器，防止定时器叠加
    clearInterval(timer);
    // 定义透明度改变速度
    var speed = 0;
    // 根据当前透明度与目标透明度的大小关系来确定速度的正负值
    if (target > op) {
        speed = 1;
    } else {
        speed = -1;
    }
    timer = setInterval(function(){
        if (op == target) {
        	// 当达到目标透明度时清除定时器
            clearInterval(timer);
        } else {
        	// 设置透明度的变化
            op += speed;
            box.style.opacity = op/100;
        }
    },3)
}
```

# JS 缓冲动画

```js
// 设置定时器，获取需要移动的元素
var timer = null,
	box = document.getElementById('box');
function moveAnimation(target) {
	// 函数开始清除定时器，防止定时器叠加
    clearInterval(timer);
    // 定义速度
    var speed = 0;
    timer = setInterval(function(){
    	// 距离目标位置越近，速度越小
    	speed = (target - box.offsetLeft)/40;
    	// 为防止速度过小而无效，对其取整
        speed = speed>0 ? Math.ceil(speed) : Math.floor(speed);
        if (box.offsetLeft == target) {
        	// 当运动到目标位置时清除定时器
            clearInterval(timer);
        } else {
        	// 设置位置的变化
            box.style.left = box.offsetLeft + speed + 'px';
        }
    },3)
}
```

# JS 多物体动画

核心思想是通过设置相应元素的属性，为不同元素设置相应的定时器及参数

`lis[i].timer = null;`

```js
window.onload = function(){
	// 获取到所有需要加动画的元素
    var lis = document.getElementsByTagName('li');
    // 遍历所有需要加动画的元素
    for (var i = 0; i < lis.length; i++) {
    	// 为每个元素设置相应的定时器
        lis[i].timer = null;
        lis[i].onmouseover = function(){
        	// 传入两个参数，第一个参数为了确保获取到相应元素的属性
            changeWidth(this,400);
        }
        lis[i].onmouseout = function(){
            changeWidth(this,200);
        }
    }
}
function changeWidth(obj,target) {
	// 清除对应的定时器
    clearInterval(obj.timer);
    obj.timer = setInterval(function(){
    	// 定义并设置速度的值
        var speed = 0;
        if (target>obj.offsetWidth) {
            speed = Math.ceil((target-obj.offsetWidth)/8);
        } else {
            speed = Math.floor((target-obj.offsetWidth)/8);
        }
        if (obj.offsetWidth == target) {
        	// 达到目标位置时清除定时器
            clearInterval(obj.timer);
        } else {
        	// 改变元素属性
            obj.style.width = obj.offsetWidth+speed+'px';
        }
    },30)
    return false;
}
```

# JS 获取样式

`obj.offsetWidth`属性可以返回对象的`padding+border+width`属性值之和

`objstyle.width`返回值就是**行内样式**定义的width属性值

为防止出现偏差，我们封装一个`getStyle`的函数

```js
function getStyle(obj,attr){
	if(obj.currentStyle){
		return obj.currentStyle[attr];
	}else{
		return getComputedStyle(obj,false)[attr];
	}
}
```

整体的JS代码如下

```js
window.onload = function(){
    var lis = document.getElementsByTagName('li');
    for (var i = 0; i < lis.length; i++) {
    	// 为元素设置对应的定时器
        lis[i].timer = null;
        lis[i].onmouseover = function(){
            changeWidth(this,400);
        }
        lis[i].onmouseout = function(){
            changeWidth(this,200);
        }
    }
}
function changeWidth(obj,target) {
    clearInterval(obj.timer);
    obj.timer = setInterval(function(){
        var speed = 0,
            icur = parseInt(getStyle(obj,'width'));
        if (target>icur) {
            speed = Math.ceil((target-icur)/10);
        } else {
            speed = Math.floor((target-icur)/10);
        }
        if (icur == target) {
            clearInterval(obj.timer);
        } else {
        	// 调用getStyle函数时注意属性使用引号括起来
            obj.style.width = icur+speed+'px';
        }
    },30)
    return false;
}
// 获得属性的函数
function getStyle(obj,attr){
    if(obj.currentStyle){
        return obj.currentStyle[attr];
    }else{
        return getComputedStyle(obj,false)[attr];
    }
}
```

# JS 改变任意属性值

主要是区别透明度与其他属性值的不同

```js
function changeStyle(obj,target,attr) {
    clearInterval(obj.timer);
    obj.timer = setInterval(function(){
        var icur = 0;
        if (attr == 'opacity') {
        	// 透明度取小数再乘一百
            icur = Math.round(parseFloat(getStyle(obj,attr))*100);
        } else {
            icur = parseInt(getStyle(obj,attr));
        }
        var speed = (target-icur)/10;
        speed = speed>0?Math.ceil(speed):Math.floor(speed);
        if (icur == target) {
            clearInterval(obj.timer);
        } else {
            if (attr == 'opacity') {
            	// 设置透明度
                obj.style[attr] = (icur+speed)/100;
            } else {
                obj.style[attr] = icur+speed+'px';
            }
        }
    },30)
    return false;
}
```

# JS链式动画

元素运动达到目标位置之后，再次调用`getStyle`函数,注意`this`的变化

```js
// 传入第四个参数
function changeStyle(obj,target,attr,fn) {
    clearInterval(obj.timer);
    obj.timer = setInterval(function(){
        var icur = 0;
        if (attr == 'opacity') {
            icur = Math.round(parseFloat(getStyle(obj,attr))*100);
        } else {
            icur = parseInt(getStyle(obj,attr));
        }
        var speed = (target-icur)/10;
        speed = speed>0?Math.ceil(speed):Math.floor(speed);
        if (icur == target) {
            clearInterval(obj.timer);
            // 调用第四个参数
            if (fn) {
            	// 改变this指向
                fn.call(obj);
            }
        } else {
            if (attr == 'opacity') {
                obj.style[attr] = (icur+speed)/100;
            } else {
                obj.style[attr] = icur+speed+'px';
            }
        }
    },30)
    return false;
}
// 调用方法
lis[0].onmouseover = function(){
    changeStyle(this,400,'width',function(){
        changeStyle(this,200,'height')
    });
}
```

# 最终运动框架

主要完成同时运动功能，运用`JSON`数据来实现

```js
function changeStyle(obj,json,fn) {
    clearInterval(obj.timer);
    obj.timer = setInterval(function(){
    	// 定义检测动画是否完成的参数
        var flag = true;
        // 遍历元素样式
        for(var attr in json){
        	// 获得样式的值
            var icur = 0;
            if (attr == 'opacity') {
                icur = Math.round(parseFloat(getStyle(obj,attr))*100);
            } else {
                icur = parseInt(getStyle(obj,attr));
            }
            // 获得变化速度
            var speed = (json[attr]-icur)/10;
            speed = speed>0?Math.ceil(speed):Math.floor(speed);
            if (icur != json[attr]) {
            	// 若样式数值未达到目标值，则flag为false
                flag = false;
            }
            // 改变样式数值
            if (attr == 'opacity') {
                obj.style[attr] = (icur+speed)/100;
            } else {
                obj.style[attr] = icur+speed+'px';
            }
        }
        // 动画完成则清除定时器并执行链式动画
        if (flag) {
            clearInterval(obj.timer);
            if (fn) {
                fn.call(obj);
            }
        }
    },30)
    return false;
}
// 调用方法
lis[0].onmouseover = function(){
    changeStyle(this,{width:205,height:200},function(){
        changeStyle(this,{opacity:30})
    });
}
```