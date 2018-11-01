---
layout: post
title:  "Node.js入门-4-Buffer(缓冲器)"
categories: Node
date:   2018-07-04 11:48:05
author: Logan
tags:  Buffer
---

* content
{:toc}

# 一、概述

在 ES6 引入 [TypedArray](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray) 之前，JavaScript 语言没有读取或操作二进制数据流的机制。 Buffer 类被引入作为 NodeJS API 的一部分，使其可以在 TCP 流或文件系统操作等场景中处理二进制数据流。Buffer 属于 Global 对象，使用时不需引入，且 Buffer 的大小在创建时确定，无法调整。

**注意：**

- Buffer的结构和数组很像，操作的方法也和数组类似
- Buffer中是以二进制的方式存储数据的
- Buffer是Node自带的，不需要引入，直接使用即可
- Buffer 一旦被创建，长度将保持不变。

# 二、创建 Buffer 的方法

在v6.0之前创建Buffer对象直接使用`new Buffer()`构造函数来创建对象实例，但是Buffer对内存的权限操作相比很大，可以直接捕获一些敏感信息，所以在v6.0以后，官方文档里面建议使用 `Buffer.from()` 接口去创建Buffer对象。





## 1. Buffer.from(string[, encoding])

新建一个包含所给的JavaScript字符串string的Buffer。encoding参数指定string的字符编码，默认为utf-8。

**Buffer.from 支持三种传参方式，另外两种传参方式为：**

- 传入一个数组，数组的每一项会以十六进制存储为 Buffer 的每一项。
- 传入一个 Buffer，会将 Buffer 的每一项作为新返回 Buffer 的每一项。

```js
const buf1 = Buffer.from('this is a tést')
// 输出: this is a tést
console.log(buf1.toString())
// 输出: this is a tC)st
console.log(buf1.toString('ascii'))

const buf2 = Buffer.from('7468697320697320612074c3a97374', 'hex')
// 输出: this is a tést
console.log(buf2.toString())

const buf3 = Buffer.from([1, 2, 3])
// 输出：<Buffer 01 02 03>
console.log(buf3)

const buf4 = Buffer.from(buf1)
// 输出：this is a tést
console.log(buf4)
```

## 2. Buffer.alloc(size[, fill[, encoding]])

分配一个大小为 size 字节的新建的 Buffer 。 如果 fill 为 undefined ，则该 Buffer 会用 0 填充。

```js
const buf = Buffer.alloc(5)
// 输出: <Buffer 00 00 00 00 00>
console.log(buf)
const buf2 = Buffer.alloc(5, 'a')
// 输出: <Buffer 61 61 61 61 61>
console.log(buf2)
```
## 3. Buffer.allocUnsafe(size)

分配一个大小为 size 字节的新建的 Buffer 。 创建的 Buffer 并没有经过初始化，在内存中只要有闲置的 Buffer 就直接 “抓过来” 使用。这使得创建 Buffer 使得内存的分配非常快，但已分配的内存段可能包含潜在的敏感数据，有明显性能优势的同时又是不安全的，所以使用需格外 “小心”。

```js
const buf = Buffer.allocUnsafe(10)
// 输出: (内容可能不同): <Buffer a0 8b 28 3f 01 00 00 00 50 32>
console.log(buf)
buf.fill(0)
// 输出: <Buffer 00 00 00 00 00 00 00 00 00 00>
console.log(buf)
```

# 三、Buffer 的常用方法

## 1. buf.fill(value[, offset[, end]][, encoding])

使用`value`填充buffer，`offset`为开始位置，`end`为结束位置，`encoding`为字符编码，默认utf-8。

此方法返回对buffer的引用，这个简化使得一个 Buffer 的创建与填充可以在一行内完成。

```js
const buf = Buffer.allocUnsafe(10).fill('abc')
//输出： abcabcabca
console.log(buf.toString())
```

## 2. buf.slice([start[, end]])

返回一个指向相同原始内存的新建的 Buffer，但做了偏移且通过 start 和 end 索引进行裁剪。

**注意**：修改新建的Buffer切片或者修改原始Buffer，都会影响另外一个，因为这两个对象所分配的内存是重叠的

```js
let buf1 = Buffer.from('abc')
let buf2 = buf1.slice(0, 2)

// 输出：ab
console.log(buf2.toString())
// 99 是字母c的十进制 ASCII 值 
buf1[1] = 99
// 输出： ac
console.log(buf2.toString())
```

## 3. buf.indexOf(value[, byteOffset][, encoding])


返回buf 中 `value` 首次出现的索引，如果 buf 没包含 `value` 则返回 -1

`byteOffset`为开始搜索的位置，`encoding`是`value`为字符串时的字符编码

`lastIndexOf(value[, byteOffset][, encoding])`查询最后一次出现的索引。

```js
const buf = Buffer.from('this is a buffer')

// 输出: 0
console.log(buf.indexOf('this'))

// 输出: 8
console.log(buf.indexOf(Buffer.from('a buffer')))

// 输出: 8
// (97 是 'a' 的十进制 ASCII 值)
console.log(buf.indexOf(97))
```

## 4. buf.copy(target[, targetStart[, sourceStart[, sourceEnd]]])

- **target** 要拷贝进的 Buffer 或 Uint8Array。
- **targetStart** : target 中开始拷贝进的偏移量。 默认: 0
- **sourceStart** : buf 中开始拷贝的偏移量。 默认: 0
- **sourceEnd** : buf 中结束拷贝的偏移量（不包含）。 默认: buf.length

```js
const buf1 = Buffer.allocUnsafe(26)
const buf2 = Buffer.allocUnsafe(26).fill('!')

for (let i = 0; i < 26; i++) {
  // 97 是 'a' 的十进制 ASCII 值
  buf1[i] = i + 97;
}

buf1.copy(buf2, 8, 16, 20)

// 输出: !!!!!!!!qrst!!!!!!!!!!!!!
console.log(buf2.toString('ascii', 0, 25))
```

## 5. Buffer.concat(list[, totalLength])

返回一个合并了 list 中所有 Buffer 实例的新建的 Buffer 。

- `list`: 要合并的Buffer实例的数组
- `totalLength`: 指定合并后新Buffer实例的长度，默认为list中每个Buffer实例的长度总和。

```js
const buf1 = Buffer.from('你')
const buf2 = Buffer.from('好')

const buf3 = Buffer.concat([buf1, buf2])
// 输出：'你好'
console.log(buf3.toString())

const buf4 = Buffer.concat([buf1, buf2], 3)
// 输出：'你'
console.log(buf4.toString())
```

## 6. Buffer.isBuffer(obj)

如果 obj 是一个 Buffer 则返回 true ，否则返回 false 。

```js
const buf1 = Buffer.alloc(4)
const str = 'This is a string'

// 输出 true
console.log(Buffer.isBuffer(buf1))
// 输出 false
console.log(Buffer.isBuffer(str))
```
