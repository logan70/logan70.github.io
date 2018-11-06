---
layout: post
title:  "理解函数节流Throttle"
categories: Javasctipt
date:   2018-11-2 11:48:05
author: Logan
tags:  Throttle
---

* content
{:toc}


# 一、函数为什么要节流

有如下代码

```js
let n = 1
window.onmousemove = () => {
  console.log(`第${n}次触发回调`)
  n++
}
```


当我们在PC端页面上滑动鼠标时，一秒可以可以触发约60次事件。大家也可以访问下面的在线例子进行测试。

<p data-height="365" data-theme-id="dark" data-slug-hash="gQpxZR" data-default-tab="result" data-user="logan70" data-pen-title="函数节流-监听鼠标移动触发次数测试" class="codepen">查看在线例子 <a href="https://codepen.io/logan70/pen/gQpxZR/">函数节流-监听鼠标移动回调触发次数测试</a> by Logan (<a href="https://codepen.io/logan70">@logan70</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

这里的回调函数只是打印字符串，如果回调函数更加复杂，可想而知浏览器的压力会非常大，可能降低用户体验。

`resize`、`scroll`或`mousemove`等事件的监听回调会被频繁触发，因此我们要对其进行限制。

# 二、实现思路

函数节流简单来说就是**对于连续的函数调用，每间隔一段时间，只让其执行一次**。初步的实现思路有两种：

## 1. 使用时间戳

设置一个对比时间戳，触发事件时，使用当前时间戳减去对比时间戳，如果差值大于设定的间隔时间，则执行函数，并用当前时间戳替换对比时间戳；如果差值小于设定的间隔时间，则不执行函数。

```js
function throttle(method, wait) {
  // 对比时间戳，初始化为0则首次触发立即执行，初始化为当前时间戳则wait毫秒后触发才会执行
  let previous = 0
  return function(...args) {
    let context = this
    let now = new Date().getTime()
    // 间隔大于wait则执行method并更新对比时间戳
    if (now - previous > wait) {
      method.apply(context, args)
      previous = now
    }
  }
}
```

<p data-height="265" data-theme-id="dark" data-slug-hash="JedvEV" data-default-tab="js,result" data-user="logan70" data-pen-title="函数节流-初步实现之时间戳" class="codepen">查看在线例子 <a href="https://codepen.io/logan70/pen/JedvEV/">函数节流-初步实现之时间戳</a> by Logan (<a href="https://codepen.io/logan70">@logan70</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## 2. 使用定时器

当首次触发事件时，设置定时器，wait毫秒后执行函数并将定时器置为`null`，之后触发事件时，如果定时器存在则不执行，如果定时器不存在则再次设置定时器。

```js
function throttle(method, wait) {
  let timeout
  return function(...args) {
    let context = this
    if (!timeout) {
      timeout = setTimeout(() => {
        timeout = null
        method.apply(context, args)
      }, wait)
    }
  }
}
```

<p data-height="265" data-theme-id="dark" data-slug-hash="zMGjRb" data-default-tab="css,result" data-user="logan70" data-pen-title="函数节流-初步实现之定时器" class="codepen">查看在线例子 <a href="https://codepen.io/logan70/pen/zMGjRb/">函数节流-初步实现之定时器</a> by Logan (<a href="https://codepen.io/logan70">@logan70</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## 3. 两种方法对比

- **第一种方法：使用时间戳**，如果对比时间戳设为0，则首次触发会立即执行。如果对比时间戳设为当前时间戳，则会在wait毫秒后触发才会执行。
- **第二种方法：使用定时器**，首次触发会在wait毫秒后执行。
- **第一种方法：使用时间戳**，停止触发后不会再执行。
- **第二种方法：使用定时器**，由于定时器的原因，停止触发后还会执行一次。

# 三、函数节流 Throttle 应用场景

- DOM 元素的拖拽功能实现（`mousemove`）
- 射击游戏的 `mousedown/keydown` 事件（单位时间只能发射一颗子弹）
- 计算鼠标移动的距离（`mousemove`）
- Canvas 模拟画板功能（`mousemove`）
- 搜索联想（`keyup`）
- 监听滚动事件判断是否到页面底部自动加载更多：给 `scroll` 加了 `debounce` 后，只有用户停止滚动后，才会判断是否到了页面底部；如果是 `throttle` 的话，只要页面滚动就会间隔一段时间判断一次


# 四、函数节流最终版

代码说话，有错恳请指出

```js
function throttle(method, wait, {leading = true, trailing = true} = {}) {
  // result 记录method的执行返回值
  let timeout, result
  // 对比时间戳
  let previous = 0
  // 记录上次回调时间
  let lastTriggeredTime = 0
  let throttled =  function(...args) {
    let context = this
    // 使用Promise，可以在触发回调时拿到原函数的返回值
    return new Promise(resolve => {
      let now = new Date().getTime()
      // 两次相邻触发的间隔
      let interval = now - lastTriggeredTime
      // 更新本次触发时间供下次使用
      lastTriggeredTime = now
      // 重置previous为now，remaining = wait > 0，假装刚执行过，实现禁止立即执行
      // 统一条件：leading为false
      // 加上以下条件之一
      // 1. 首次触发（此时previous为0）
      // 2. trailing为true时，停止触发时间超过wait，定时器内函数执行（previous被置为0），然后再次触发
      // 3. trailing为false时（不设定时器，previous不会被置为0），停止触发时间超过wait后再次触发（interval > wait）
      if (leading === false && (!previous || interval > wait)) {
        previous = now
        // 保险起见，清除定时器并置为null
        // 假装刚执行过要假装的彻底XD
        if (timeout) {
          clearTimeout(timeout)
          timeout = null
        }
      }
      // 距离下次执行原函数的间隔
      let remaining = wait - (now - previous)
      // 1. leading为true时，首次触发就立即执行
      // 2. 到达下次执行原函数时间
      // 3. 修改了系统时间
      if (remaining <= 0 || remaining > wait) {
        if (timeout) {
          clearTimeout(timeout)
          timeout = null
        }
        // 更新对比时间戳，执行函数并记录返回值，传给resolve
        previous = now
        result = method.apply(context, args)
        resolve(result)
        // 解除引用，防止内存泄漏
        // lastTriggeredTime置为null，下次的interval为13位数字（即下次触发时的时间），不影响逻辑
        if (!timeout) context = args = lastTriggeredTime = null
      } else if (!timeout && trailing !== false) {
        timeout = setTimeout(() => {
          // leading为false时将previous设为0的目的在于
          // 如果定时器触发后很长时间没有触发回调
          // 下次触发时的remaining为负，原函数会立即执行，违反了leading为false的设定
          previous = leading === false ? 0 : new Date().getTime()
          timeout = null
          result = method.apply(context, args)
          resolve(result)
          // 解除引用，防止内存泄漏
          if (!timeout) context = args = lastTriggeredTime = null
        }, remaining)
      }
    })
  }
  // 加入取消功能，使用方法如下
  // let throttledFn = throttle(otherFn)
  // throttledFn.cancel()
  throttled.cancel = function() {
    clearTimeout(timeout)
    previous = 0
    timeout = null
  }

  return throttled
}
```

调用节流后的函数的外层函数也需要使用Async/Await语法等待执行结果返回

使用方法见代码：

```js
function square(num) {
  return Math.pow(num, 2)
}

// let throttledFn = throttle(square, 1000)
// let throttledFn = throttle(square, 1000, {leading: false})
// let throttledFn = throttle(square, 1000, {trailing: false})
let throttledFn = throttle(square, 1000, {leading: false, trailing: false})

window.onmousemove = async () => {
  try {
    let val = await throttledFn(4)
    // 原函数不执行时val为undefined
    if (typeof val !== 'undefined') {
      console.log(`原函数返回值为${val}`)
    }
  } catch (err) {
    console.error(err)
  }
}

// 鼠标移动时，每间隔1S输出：
// 原函数的返回值为：16
```

<p data-height="265" data-theme-id="dark" data-slug-hash="VVLJYX" data-default-tab="js,result" data-user="logan70" data-pen-title="函数节流-优化第五版：处理原函数返回值" class="codepen">查看在线例子： <a href="https://codepen.io/logan70/pen/VVLJYX/">函数节流最终版</a> by Logan (<a href="https://codepen.io/logan70">@logan70</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

具体的实现步骤请往下看


# 五、函数节流 Throttle 的具体实现步骤

## 1. 优化第一版：融合两种实现方式

这样实现的效果是首次触发立即执行，停止触发后会再执行一次

```js
function throttle(method, wait) {
  let timeout
  let previous = 0
  return function(...args) {
    let context = this
    let now = new Date().getTime()
    // 距离下次函数执行的剩余时间
    let remaining = wait - (now - previous)
    // 如果无剩余时间或系统时间被修改
    if (remaining <= 0 || remaining > wait) {
      // 如果定时器还存在则清除并置为null
      if (timeout) {
        clearTimeout(timeout)
        timeout = null
      }
      // 更新对比时间戳并执行函数
      previous = now
      method.apply(context, args)
    } else if (!timeout) {
      // 如果有剩余时间但定时器不存在，则设置定时器
      // remaining毫秒后执行函数、更新对比时间戳
      // 并将定时器置为null
      timeout = setTimeout(() => {
        previous = new Date().getTime()
        timeout = null
        method.apply(context, args)
      }, remaining)
    }
  }
}
```

我们来捋一捋，假设连续触发回调：

1. 第一次触发：对比时间戳为0，剩余时间为负数，立即执行函数并更新对比时间戳
2. 第二次触发：剩余时间为正数，定时器不存在，设置定时器
3. 之后的触发：剩余时间为正数，定时器存在，不执行其他行为
4. 直至剩余时间小于等于0或定时器内函数执行（由于回调触发有间隔，且setTimeout有误差，故哪个先触发并不确定）
  - 若定时器内函数执行，更新对比时间戳，并将定时器置为null，下一次触发继续设定定时器
  - 若定时器内函数未执行，但剩余时间小于等于0，清除定时器并置为null，立即执行函数，更新时间戳，下一次触发继续设定定时器
5. 停止触发后：若非在上面所述的两个特殊时间点时停止触发，则会存在一个定时器，原函数还会被执行一次

<p data-height="265" data-theme-id="dark" data-slug-hash="JedZMb" data-default-tab="js,result" data-user="logan70" data-pen-title="优化第一版：融合两种实现方式" class="codepen">查看在线例子： <a href="https://codepen.io/logan70/pen/JedZMb/">优化第一版：融合两种实现方式</a> by Logan (<a href="https://codepen.io/logan70">@logan70</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## 2. 优化第二版：提供首次触发时是否立即执行的配置项

```js
// leading为控制首次触发时是否立即执行函数的配置项
function throttle(method, wait, leading = true) {
  let timeout
  let previous = 0
  return function(...args) {
    let context = this
    let now = new Date().getTime()
    // !previous代表首次触发或定时器触发后的首次触发，若不需要立即执行则将previous更新为now
    // 这样remaining = wait > 0，则不会立即执行，而是设定定时器
    if (!previous && leading === false) previous = now
    let remaining = wait - (now - previous)
    if (remaining <= 0 || remaining > wait) {
      if (timeout) {
        clearTimeout(timeout)
        timeout = null
      }
      previous = now
      method.apply(context, args)
    } else if (!timeout) {
      timeout = setTimeout(() => {
        // 如果leading为false，则将previous设为0，
        // 下次触发时会与下次触发时的now同步，达到首次触发（对于用户来说）不立即执行
        // 如果直接设为当前时间戳，若停止触发一段时间，下次触发时的remaining为负值，会立即执行
        previous = leading === false ? 0 : new Date().getTime()
        timeout = null
        method.apply(context, args)
      }, remaining)
    }
  }
}
```

<p data-height="265" data-theme-id="dark" data-slug-hash="QJbRpJ" data-default-tab="js,result" data-user="logan70" data-pen-title="函数节流-优化第二版：提供首次触发时是否立即执行的配置项" class="codepen">查看在线例子： <a href="https://codepen.io/logan70/pen/QJbRpJ/">函数节流-优化第二版：提供首次触发时是否立即执行的配置项</a> by Logan (<a href="https://codepen.io/logan70">@logan70</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## 3. 优化第三版：提供停止触发后是否还执行一次的配置项

```js
// trailing为控制停止触发后是否还执行一次的配置项
function throttle(method, wait, {leading = true, trailing = true} = {}) {
  let timeout
  let previous = 0
  return function(...args) {
    let context = this
    let now = new Date().getTime()
    if (!previous && leading === false) previous = now
    let remaining = wait - (now - previous)
    if (remaining <= 0 || remaining > wait) {
      if (timeout) {
        clearTimeout(timeout)
        timeout = null
      }
      previous = now
      method.apply(context, args)
    } else if (!timeout && trailing !== false) {
      // 如果有剩余时间但定时器不存在，且trailing不为false，则设置定时器
      // trailing为false时等同于只使用时间戳来实现节流
      timeout = setTimeout(() => {
        previous = leading === false ? 0 : new Date().getTime()
        timeout = null
        method.apply(context, args)
      }, remaining)
    }
  }
}
```

<p data-height="265" data-theme-id="dark" data-slug-hash="WYvBdL" data-default-tab="js,result" data-user="logan70" data-pen-title="函数节流-优化第三版：提供停止触发后是否还执行一次的配置项" class="codepen">查看在线例子 <a href="https://codepen.io/logan70/pen/WYvBdL/">函数节流-优化第三版：提供停止触发后是否还执行一次的配置项</a> by Logan (<a href="https://codepen.io/logan70">@logan70</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## 4. 优化第四版：提供取消功能

有些时候我们需要在不可触发的这段时间内能够手动取消节流，代码实现如下：

```js
function throttle(method, wait, {leading = true, trailing = true} = {}) {
  let timeout
  let previous = 0
  // 将返回的匿名函数赋值给throttled，以便在其上添加取消方法
  let throttled =  function(...args) {
    let context = this
    let now = new Date().getTime()
    if (!previous && leading === false) previous = now
    let remaining = wait - (now - previous)
    if (remaining <= 0 || remaining > wait) {
      if (timeout) {
        clearTimeout(timeout)
        timeout = null
      }
      previous = now
      method.apply(context, args)
    } else if (!timeout && trailing !== false) {
      timeout = setTimeout(() => {
        previous = leading === false ? 0 : new Date().getTime()
        timeout = null
        method.apply(context, args)
      }, remaining)
    }
  }

  // 加入取消功能，使用方法如下
  // let throttledFn = throttle(otherFn)
  // throttledFn.cancel()
  throttled.cancel = function() {
    clearTimeout(timeout)
    previous = 0
    timeout = null
  }

  // 将节流后函数返回
  return throttled
}
```

<p data-height="265" data-theme-id="dark" data-slug-hash="QJbRBP" data-default-tab="js,result" data-user="logan70" data-pen-title="函数节流-优化第四版：提供取消功能" class="codepen">查看在线例子 <a href="https://codepen.io/logan70/pen/QJbRBP/">函数节流-优化第四版：提供取消功能</a> by Logan (<a href="https://codepen.io/logan70">@logan70</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## 5. 优化第五版：处理原函数返回值

需要节流的函数可能是存在返回值的，我们要对这种情况进行处理，`underscore`的处理方法是将函数返回值在返回的`debounced`函数内再次返回，但是这样其实是有问题的。如果原函数执行在`setTimeout`内，则无法同步拿到返回值，我们使用Promise处理原函数返回值。

```js
function throttle(method, wait, {leading = true, trailing = true} = {}) {
  // result记录原函数执行结果
  let timeout, result
  let previous = 0
  let throttled =  function(...args) {
    let context = this
    // 返回一个Promise，以便可以使用then或者Async/Await语法拿到原函数返回值
    return new Promise(resolve => {
      let now = new Date().getTime()
      if (!previous && leading === false) previous = now
      let remaining = wait - (now - previous)
      if (remaining <= 0 || remaining > wait) {
        if (timeout) {
          clearTimeout(timeout)
          timeout = null
        }
        previous = now
        result = method.apply(context, args)
        // 将函数执行返回值传给resolve
        resolve(result)
      } else if (!timeout && trailing !== false) {
        timeout = setTimeout(() => {
          previous = leading === false ? 0 : new Date().getTime()
          timeout = null
          result = method.apply(context, args)
          // 将函数执行返回值传给resolve
          resolve(result)
        }, remaining)
      }
    })
  }

  throttled.cancel = function() {
    clearTimeout(timeout)
    previous = 0
    timeout = null
  }

  return throttled
}
```

**使用方法一**：在调用节流后的函数时，使用`then`拿到原函数的返回值

```js
function square(num) {
  return Math.pow(num, 2)
}

let throttledFn = throttle(square, 1000, false)

window.onmousemove = () => {
  throttledFn(4).then(val => {
    console.log(`原函数的返回值为：${val}`)
  })
}

// 鼠标移动时，每间隔1S后输出：
// 原函数的返回值为：16
```

**使用方法二**：调用节流后的函数的外层函数使用Async/Await语法等待执行结果返回

使用方法见代码：

```js
function square(num) {
  return Math.pow(num, 2)
}

let throttledFn = throttle(square, 1000)

window.onmousemove = async () => {
  try {
    let val = await throttledFn(4)
    // 原函数不执行时val为undefined
    if (typeof val !== 'undefined') {
      console.log(`原函数返回值为${val}`)
    }
  } catch (err) {
    console.error(err)
  }
}

// 鼠标移动时，每间隔1S输出：
// 原函数的返回值为：16
```

<p data-height="265" data-theme-id="dark" data-slug-hash="VVLJYX" data-default-tab="js,result" data-user="logan70" data-pen-title="函数节流-优化第五版：处理原函数返回值" class="codepen">查看在线例子： <a href="https://codepen.io/logan70/pen/VVLJYX/">函数节流-优化第五版：处理原函数返回值</a> by Logan (<a href="https://codepen.io/logan70">@logan70</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## 6. 优化第六版：可同时禁用立即执行和后置执行

模仿`underscore`实现的函数节流有一点美中不足，那就是 `leading：false` 和 `trailing: false` 不能同时设置。

如果同时设置的话，比如当你将鼠标移出的时候，因为 `trailing` 设置为 `false`，停止触发的时候不会设置定时器，所以只要再过了设置的时间，再移入的话，`remaining`为负数，就会立刻执行，就违反了 `leading: false`，这里我们优化的思路如下：

计算连续两次触发回调的时间间隔，如果大于设定的间隔值时，重置对比时间戳为当前时间戳，这样就相当于回到了首次触发，达到禁止首次触发（对用户来说）立即执行的效果，代码如下，有错恳请指出：

```js
function throttle(method, wait, {leading = true, trailing = true} = {}) {
  let timeout, result
  // 记录上次触发时间
  let lastTriggeredTime = 0
  let previous = 0
  let throttled =  function(...args) {
    let context = this
    return new Promise(resolve => {
      let now = new Date().getTime()
      // 两次触发的间隔
      let interval = now - lastTriggeredTime
      // 更新本次触发时间供下次使用
      lastTriggeredTime = now
      // 更改条件，两次间隔时间大于wait且leading为false时也重置previous，实现禁止立即执行
      if (leading === false && (!previous || interval > wait)) {
        previous = now
      }
      let remaining = wait - (now - previous)
      if (remaining <= 0 || remaining > wait) {
        if (timeout) {
          clearTimeout(timeout)
          timeout = null
        }
        previous = now
        result = method.apply(context, args)
        resolve(result)
        // 解除引用，防止内存泄漏
        if (!timeout) context = args = lastTriggeredTime = null
      } else if (!timeout && trailing !== false) {
        timeout = setTimeout(() => {
          previous = leading === false ? 0 : new Date().getTime()
          timeout = null
          result = method.apply(context, args)
          resolve(result)
          // 解除引用，防止内存泄漏
          if (!timeout) context = args = lastTriggeredTime = null
        }, remaining)
      }
    })
  }

  throttled.cancel = function() {
    clearTimeout(timeout)
    previous = 0
    timeout = null
  }

  return throttled
}
```

<p data-height="265" data-theme-id="dark" data-slug-hash="EOjBbb" data-default-tab="js,result" data-user="logan70" data-pen-title="函数节流-优化第六版：可同时禁用立即执行和后置执行" class="codepen"> <a href="https://codepen.io/logan70/pen/EOjBbb/">函数节流-优化第六版：可同时禁用立即执行和后置执行</a> by Logan (<a href="https://codepen.io/logan70">@logan70</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

# 六、参考文章

[JavaScript专题之跟着 underscore 学节流](https://juejin.im/post/5947312a61ff4b006cf66be9)

[underscore 函数节流的实现](https://github.com/hanzichi/underscore-analysis/issues/22)

如果有错误或者不严谨的地方，请务必给予指正，十分感谢。