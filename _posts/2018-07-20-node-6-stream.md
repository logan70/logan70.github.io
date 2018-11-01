---
layout: post
title:  "Node.js入门-6-stream(流)"
categories: Node
date:   2018-07-20 11:48:05
author: Logan
tags:  Stream
---

* content
{:toc}

# Stream（流）

流（stream）是一种在 Node.js 中处理流式数据的抽象接口。流可以是可读的、可写的、或是可读写的。

Node.js 中有四种基本的流类型：

- `Writable` :  可写入数据的流（例如 `fs.createWriteStream()`）。
- `Readable` :  可读取数据的流（例如 `fs.createReadStream()`）。
- `Duplex` :  可读又可写的流（例如 `net.Socket`）。
- `Transform` :  在读写过程中可以修改或转换数据的 Duplex 流（例如 `zlib.createDeflate()`）。





所有的流都是 `EventEmitter` 的实例。常用的事件有：

- `data` : 当有数据可读时触发。

- `end` : 没有更多的数据可读时触发。

- `error` : 在接收和写入过程中发生错误时触发。

- `finish` : 所有数据已被写入到底层系统时触发。

## 可读流`fs.createReadStream()`

```js
let txt = ''
// 创建可读流
let rs = fs.createReadStream(path.join(__dirname, '1.txt'))

// 设置编码为 utf8
rs.setEncoding('UTF8')

// 处理流事件 --> data, end, and error
rs.on('data', (data) => {
   txt += data
})

rs.on('end', () => {
    console.log(txt)
})
```

## 可写流`fs.createWriteStream()`

```js
// 创建可写流
let ws = fs.createWriteStream(path.join(__dirname, '1.txt'))

// 写入数据
ws.write('第一次通过 writable.write() 写入的内容\n', 'UTF8', () => {
    console.log('第一次写入内容')
})
ws.write('第二次通过 writable.write() 写入的内容', 'UTF8', () => {
    console.log('第二次写入内容')
})

// 标记文件末尾
ws.end()

// 处理流事件 --> data, end, and error
ws.on('finish', () => {
   console.log('写入数据完成')
})
```

## 管道流`readerStream.pipe(writerStream)`

管道提供了一个输出流到输入流的机制。通常我们用于从一个流中获取数据并将数据传递到另外一个流中。

```js
// 创建一个可读流
let readerStream = fs.createReadStream(path.join(__dirname, '1.mp4'))

// 创建一个可写流
let writerStream = fs.createWriteStream(path.join(__dirname, '1-copy.mp4'))

// 管道读写操作
// 读取 1.mp4 文件内容，并将内容写入到 1-copy.mp4 文件中
readerStream.pipe(writerStream)
```

**默认情况下，当源可读流（the source Readable stream）触发 'end' 事件时，目标流也会调用 stream.end() 方法从而结束写入。要禁用这一默认行为， end 选项应该指定为 false， 这将使目标流保持打开**

```js
reader.pipe(writer, { end: false })
```

## 链式流

`readable.pipe()` 方法返回 目标流 的引用，这样就可以对流进行链式地管道操作

**通过链式操作来压缩文件**

```js
fs.createReadStream(path.join(__dirname, 'node-1.js'))
    .pipe(zlib.createGzip())
    .pipe(fs.createWriteStream(path.join(__dirname, 'node-1.js.gz')))
console.log('通过链式操作压缩node-1.js完成')
```

**通过链式操作来解压文件**

```js
fs.createReadStream(path.join(__dirname, 'node-1.js.gz'))
  .pipe(zlib.createGunzip())
  .pipe(fs.createWriteStream(path.join(__dirname, 'node-1-decompress.js')))
console.log('通过链式操作解压node-1.js.gz完成')
```