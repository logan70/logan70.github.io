---
layout: post
title:  "移动端滚动穿透问题解决方案"
categories: Mobile
date:   2018-10-19 11:48:05
author: Logan
tags:  Mobile
---

* content
{:toc}

# 一、 问题描述

移动端当position为fixed的弹窗弹出时，滑动弹窗，下层的页面会跟随滚动，这就叫做**滚动穿透**

# 二、 解决方案

## 1. `body` 设置`overflow: hidden;`

当弹窗弹出时，设置`body`元素的`overflow`属性为`hidden`，暴力去除滚动。

**缺点**：会丢失页面的滚动位置，需要使用js进行还原

```css
// CSS
.modal-open {
  overflow: hidden;
  height: 100%;
}
```

```js
// JS
let modalManager = (function() {
  return {
    scrollTop: 0,
    getScrollTop() {
      return document.body.scrollTop || document.documentElement.scrollTop
    },
    scrollTo(position) {
      document.body.scrollTop = document.documentElement.scrollTop = position
    },
    show(ele) {
      this.scrollTop = this.getScrollTop()
      document.querySelector(ele).style.display = 'block'
      document.body.classList.add('modal-open')
      this.scrollTo(this.scrollTop)
    },
    hide(ele) {
      document.querySelector(ele).style.display = 'none'
      document.body.classList.remove('modal-open')
      this.scrollTop = 0
    }
  }
})();
```





## 2. `body` 设置`position: fixed;`

当弹窗弹出时，设置`body`元素的`positon`属性为`fixed`，使其脱离文档流，去除滚动。

**缺点**：会丢失页面的滚动位置，需要使用js进行还原

```css
// CSS
.modal-open {
  position: fixed;
  width: 100%;
}
```

```js
// JS
let modalManager = (function() {
  return {
    scrollTop: 0,
    // 弹窗数量
    modalNum: 0,
    getScrollTop() {
      return document.body.scrollTop || document.documentElement.scrollTop
    },
    // body脱离文档流，通过设置其top属性来恢复滚动位置
    scrollTo(position) {
      document.body.style.top = -position + 'px'
    },
    show(ele) {
      // 与第一种方法不同的是，body脱离文档流时，读取不到scrollTop，所以同时弹出多个弹窗的时候，不用重新读取scrollTop
      if (this.modalNum <= 0) {
        this.scrollTop = this.getScrollTop()
        document.body.classList.add('modal-open')
        this.scrollTo(this.scrollTop)
      }
      document.querySelector(ele).style.display = 'block'
      this.modalNum++
    },
    hide(ele) {
      if (this.modalNum <= 1) {
        document.body.classList.remove('modal-open')
        document.body.scrollTop = document.documentElement.scrollTop = this.scrollTop
        this.scrollTop = 0
      }
      document.querySelector(ele).style.display = 'none'
      this.modalNum = this.modalNum >= 1 ? this.modalNum - 1 : 0
    }
  }
})();
```

## 3. 阻止弹窗的touchmove默认事件

**缺点**：导致弹窗内部也无法滚动，适用于弹窗内无滚动内容的情况

```js
document.querySelector('.modal').addEventListener('touchmove', e => {
  e = e || window.event
  e.preventDefault()
}, false)
```