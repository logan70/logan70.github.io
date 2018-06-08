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

- Promise 是一个代理对象，它和原先要进行的操作并无关系
- Promise 通过引入一个回调，避免更多的回调

**Promise 的状态**

- pending [待定]初始状态
- fulfilled [实现]操作成功
- rejected [被否决]操作失败

> Promise状态发生改变，就会触发`.then()`里的响应函数处理后续步骤 <br/>
> Promise 状态一经改变，不会再变

## `.then()`

- 状态响应函数可以返回新的Promise，或其他值，或者不返回值
- 如果返回新的Promise，那么下一级`.then()`会在新Promise状态改变之后执行
- 如果返回其他任何值或不返回值，则会立刻执行下一级`.then()`

## 小测验

假设`doSomething`与`doSomethingElse`及`finalHandler`函数都返回一个Promise实例，那么下面四中Promise的区别是什么

```js
// #1
doSomeThing().then(function() {
  return doSomethingElse()
}).then(finalHandler)
/**
 * doSomething ->
 * doSomethingElse(undefined) ->
 * finalHandler(resultOfDoSomethingElse)
 **/

// #2
doSomething().then(function() {
  doSomethingElse()
}).then(finalHandler)
/**
 * doSomething ->
 * doSomethingElse(undefined) + finalHandler(undefined) 几乎同时执行
 **/

// #3
doSomething().then(doSomethingElse()).then(finalHandler)
/**
 * doSomething + doSomethingElse(undefined) 几乎同时执行 ->
 * finalHandler(resultOfDoSomething) 
 **/

// #4
doSomething().then(doSomethingElse).then(finalHandler)
/**
 * doSomething ->
 * doSomethingElse(resultOfDoSomething) ->
 * finalHandler(resultOfDoSomethingElse)
 **/
```

## `.catch()`

建议在所有队列最后都加上`.catch()`，以避免漏掉错误处理造成意想不到的问题

## `Promise.all()`

- **批量执行**：用于将多个Promise实例，包装成一个新的Promise实例
- `Promise.all([p1,p2,p3,...])`
- 数组里可以是Promise对象，也可以是别的值，只有Promise会等待状态改变
- 当所有子Promise都完成，该Promise完成，返回值是全部值得数组
- 有任何一个失败，该Promise失败，返回值是第一个失败的子Promise的结果

```js
console.log('here we go')
Promise.all([1, 2, 3])
    .then( all => {
        console.log('1：', all)
        return Promise.all([ function () {
            console.log('ooxx')
        }, 'xxoo', false])
    })
    .then( all => {
        console.log('2：', all)
        let p1 = new Promise( resolve => {
            setTimeout(() => {
                resolve('I\'m P1')
            }, 1500)
        })
        let p2 = new Promise( (resolve, reject) => {
            setTimeout(() => {
                resolve('I\'m P2')
            }, 1000)
        })
        let p3 = new Promise( (resolve, reject) => {
            setTimeout(() => {
                reject('I\'m P3')
            }, 3000)
        })
        return Promise.all([p1, p2, p3])
    })
    .then( all => {
        console.log('all', all)
    })
    .catch( err => {
        console.log('Catch：', err)
    })
// here we go
// 1： [ 1, 2, 3 ]
// 2： [ [Function], 'xxoo', false ]
// Catch： I'm P3
```