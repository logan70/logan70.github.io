---
layout: post
title:  "慕课ES6入门与Promise入门"
categories: Javascript
date:   2018-05-05 11:48:05
author: Logan
tags:  ES6 Promise
---

* content
{:toc}

# ES6入门

## 常量

### ES5中常量的写法

```js
Object.defineProperty(window, 'myPI', {
  value: 3.141592653,
  writable: false
})
```

### ES6中常量的写法

```js
const PI = 3.141592653
console.log(PI)
PI = 1 // error
```

## 作用域

块级作用域的存在

## 箭头函数

```js
() => {
  //***
}
```

- 小括号内的参数如果只有一个，则可以省略小括号
- 花括号内只存在返回值，则可以省略花括号和return

```js
// ES5写法
arr.map(function(val) {
  return val + 1
})

// ES6写法1
arr.map((val) => {
  return val + 1
})

// ES6写法2
arr.map(val => val + 1)
```





## 函数参数相关知识

### ES3/5中默认参数的写法

```js
function f(x, y, z) {
  x = x || 1
  y = y || 7
  z = z || 42
  return x + y + z
}
```

### ES6中默认参数的写法

```js
function f(x, y = 7, z = 42) {
  return x + y + z
}
```

### ES6中检查参数不为空

```js
function checkParam() {
  throw new Error('Param cannot be undefined')
}
function f(x = checkParam(), y = 7, z = 42) {
  return x + y + z
}
try {
  f()
} catch(e) {
  console.log(e)
}
```

### ES3/5 可变参数

```js
function f() {
  var a = Array.prototype.slice.call(arguments)
  var sum = 0
  a.forEach(function(val) {
    sum += val
  })
  return sum
}
console.log(f(1, 2, 3, 4))
```

### ES6 可变参数

```js
function f(...a) {
  let sum = 0
  a.forEach(val => {
    sum += val
  })
  return sum
}
console.log(f(1, 2, 3, 4))
```

### 合并数组

```js
// ES3/5
var arr1 = [1, 2, 3]
var arr2 = [4, 5].concat(arr1)

// ES6
let arr1 = [1, 2, 3]
let arr2 = [4, 5, ...arr1]
```

### ES3/5 数据保护

```js
// ES3
var Person = function() {
  var data = {
    name: 'es3',
    sex: 'male',
    age:15
  }
  this.get = function(key) {
    return data[key]
  }
  this.set = function(key, value) {
    if (key !== 'sex') {
      data[key] = value
    }
  }
}
var Person = new Person()
// ES5
var Person = {
  name: 'es5',
  age: 15
}
Object.defineProperty(Person, 'sex', {
  value: 'male',
  writable: false
})
```

### ES6 数据保护

```js
let Person = {
  name: 'es6',
  age: 15,
  sex: 'male'
}
let person = new Proxy(Person, {
  get(target, key) {
    return target[key]
  },
  set(target, key, value) {
    if(key !== 'sex') {
      target[key] = value
    }
  }
})
```

# Promise 入门

## Promise是什么

- 主要用于异步计算
- 可以将异步操作队列化，按照期望的顺序执行，返回符合预期的结果
- 可以在对象之间传递和操作Promise，帮助我们处理队列

## 回调的四个问题

- 嵌套层次很深，难以维护
- 无法正常使用return和throw
- 无法正常检索堆栈信息（函数先入栈先出栈，每一次回调都是在系统层面上的一个新的堆栈）
- 多个回调之间难以建立联系（一个回调一旦启用，我们就无法对其进行操作）

## Promise 简介

```js
new Promise(
  /* 执行器 executor */
  function(resolve, reject) {
    // 一段耗时很长的异步操作
    resolve() // 数据处理完成
    reject() // 数据处理出错
  }
).then(function A() {
  // 成功，下一步
}, function B() {
  // 失败，做相应处理
})
```

