---
layout: post
title:  "复习JavaScript--JavaScript语法"
categories: Javascript
date:   2017-09-13 16:48:05
author: Logan
tags:  Javascript
---

* content
{:toc}

# JavaScript注释

- 单行注释`//`
- 多行注释`/**/`

> ECMAScript中的一切（变量、函数名和操作符）都区分大小写

***

# 标识符

**什么是标识符**

变量、函数、属性的名字、或者函数的参数

**标识符的命名规则**

- 由字母、数字、下划线`_`或美元符号`$`组成
- 不能以数字开头
- 不能使用关键字、保留字等作为标识符

***

# 变量

ECMAScript的变量是**松散类型**，每个变量仅仅是一个用于保存值得占位符而已

> 松散类型： 可以用来保存任何类型的数据




**变量声明**

```js
//先声明，再赋值

var myName;
    myName = "logan";

//声明同时赋值

var myName = "logan";

//一次声明多个变量

var myName = "logan",
    myAge = 24,
    address;
```

> 省略`var`声明的变量是全局变量

***

# JavaScript的数据类型

**`typeof`**

- 功能： 监测变量类型
- 语法： `typeof 变量`或者`typeof(变量)`
- 返回值： `string/number/boolean/object/undefined/function`

## undefined

undefined类型只有一个值，即特殊的undefined

## null

- null值表示一个空对象指针
- 如果定义的变量准备在将来用于保存对象，那么最好将变量初始化为`null`

> `undefined`值是派生自`null`值的，所以`undefined == null`的返回结果是`true`

## number

表示整数和浮点数

**`NaN`**

即非数值(Not a Number)，是一个特殊的数值

- 任何涉及NaN的操作，都会返回NaN
- NaN与任何值都不相等，包括NaN本身

**`isNaN()`**

- 语法： `isNaN(n)` 参数可以是任何类型
- 功能： 检测参数`n`是否是“非数值”
- 返回值： boolean

> `isNaN()`对接收的参数，先尝试转换为数值，再检测是否为非数值

```js
isNaN("258");   //false
isNaN("a" + 10);   //true
```

***

**数值转换**

有三个函数可以把非数值转换为数值

`Number()`

 > 参数可以是任何类型，强制转换为数值

 ```js
 Number("16");           //16
 typeof(Number("16"));   //number

 Number("16px");           //NaN
 typeof(Number("16px"));   //number
```

`parseInt()`

 > 用于把字符串转换为**整数**数值<br>
 > `parseInt()`会忽略字符串前的空格<br>
 > `parseInt()`会忽略前导的零<br>
 > `parseInt()`转换空字符串返回NaN <br>
 > `parseInt()`函数提供第二个参数，即转换时使用的基数（即多少进制2-36）

 ```js
 parseInt("28px");       //28
 parseInt("28.5px");     //28
 parseInt("   28px");    //28
 parseInt("a28px");      //NaN
 parseInt("");           //NaN
 parseInt("1f",16);		 //返回 31 (16+15)
```

`parseFloat()`

 > 用于把字符串转换为**整数**或者**浮点数**数值<br>
 > `parseFloat()`第一个小数点有效，直至遇到一个无效的浮点数字符为止
 > `parseFloat()`会忽略字符串前的空格<br>
 > `parseFloat()`会忽略前导的零<br>
 > `parseFloat()`转换空字符串返回NaN <br>

 ```js
 parseFloat("28px");       //28
 parseFloat("28.5px");     //28.5
 parseFloat("28.5.2px");     //28.5
 parseFloat("   28px");    //28
 parseFloat("a28px");      //NaN
 parseFloat("");           //NaN
```

## string

string类型用于表示由零个或者多个16位Unicode字符组成的字符序列，即**字符串**，字符串可以由双引号`""`或者单引号`''`表示

**字符串转换**

`toString()`

> `x.toString()` 无法转换`null`和`undefined`

```js
//数组
var array = ["CodePlayer", true, 12, -5];
array.toString(); // CodePlayer,true,12,-5
// 日期
var date = new Date();
date.toString();  // Wed Sep 13 2017 22:07:48 GMT+0800 (中国标准时间)
// 数字
var num =  15.26540;
num.toString();   // 15.2654
// 布尔
var bool = true;
bool.toString();  // true
// Object
var obj = {name: "张三", age: 18};
obj.toString();   // [object Object]
// HTML DOM 节点
var eles = document.getElementsByTagName("body");
eles.toString(); // [object NodeList]
eles[0].toString(); // [object HTMLBodyElement]
```

`String()`

> `String()`能够将任何类型的值转换为字符串，在不知道要转换的值是不是`null`和`undefined`的情况下，可以使用`String()`

```js
// 数字
var a=123;
String(a);    //123
//布尔值
var b=true;
String(b);    //true
//undefined
var c;
String(c);    //undefined
//null
var d=null;
String(d);    //null
```

## boolean

用于表示真假的类型，`true`表示真，`false`表示假

**布尔型转换**

`Boolean()`

- 除0之外的所有数字，转换为布尔型都为true
- 除`""`空字符串之外的所有字符串，转换为布尔型都为true
- `null`和`undefined`转换为布尔型都为false

```js
Boolean(0);            //false
Boolean(5);            //true
Boolean("");           //false
Boolean("abc");        //true
Boolean(null);         //false
Boolean(undefined);    //false
Boolean(NaN);          //false
```

***

# 表达式

> 将同类型的数据（如常量、变量、函数等），用运算符号按一定的规则连接起来的、有意义的式子称为`表达式`

## 操作符

**算数操作符**

- `+` 加
- `-` 减
- `*` 乘
- `/` 除
- `%` 取余
- `++a`与`a++` 递增
	>`++a`返回递增之后的a的值<br>
	>`a++`先返回a的原值，再返回递增之后的值<br>

- `--a`与`a--` 递减，与递增同理
	
	```js
	var x1 = 20,
	    x2 = 30,
	    x3 = --x1 + x2--;
	x1;    //19
	x2;    //29
	x3;    //19+30  49
	```

**赋值操作符**

- `=`
- `+=`
- `-=`
- `*=`
- `/=`
- `%=`

```js
var a = 10;
a += 5;   //15
a -= 1;   //14
a *= 2;   //24
a /= 4;   //6
a %= 5;   //1

var b = "hello ";
b += "world";   //hello world
```

**比较操作符**

- `>`
- `<`
- `>=`
- `<=`
- `==`
- `===`
- `!=`
- `!==`

> `==` 只比较值是否相等<br>
> `===` 比较值的同时比较数据类型是否相等<br>
> `！=` 只比较值是否不相等<br>
> `===` 比较值的同时比较数据类型是否不相等<br>
> **返回值**： boolean型<br>

```js
var a = 15,
    b = "15";
a == b;                 //true
a === b;                //false
a != b;                 //false
a !== b;                //true
null == undefined;      //true
null === undefined;     //false          
```

**三元操作符**

语法
> 条件 ? 执行代码1 : 执行代码2

说明
> 可以代替简单的if语句<br>
> 如果条件成立，执行代码1，否则执行代码2

**逻辑操作符**

- `&&` 与

	> 只要有一个条件不成立，就返回`false`

	> 在有一个操作数不是**布尔值**的情况下，逻辑与`&&`操作就不一定返回**布尔值**，此时它遵循以下规则：<br>
	>  1. 如果第一个操作数隐式类型转换后为`true`，则依次后推至`false`的操作数并返回
	>  2. 如果第一个操作数隐式类型转换后为`false`，则返回第一个操作数
	>  3. 如果操作数中有`null`，则返回`null`
	>  4. 如果操作数中有`NaN`，则返回`NaN`
	>  5. 如果操作数中有`undefined`，则返回`undefined`

	```js
	2 && 3 && "hello";                 //"hello"
	0 && 3 && "hello";                 //0
	0 && null && NaN && undefined;     //0
	3 && null && NaN && undefined;     //null
	3 && NaN && null && undefined;     //NaN
	3 && undefined && NaN && null;     //undefined
	```

- `||` 或

	> 只要有一个条件成立，就返回`true`

	> 在有一个操作数不是**布尔值**的情况下，逻辑或`||`操作就不一定返回**布尔值**，此时它遵循以下规则：<br>
	>  1. 如果第一个操作数隐式类型转换后为`true`，则返回第一个操作数
	>  2. 如果第一个操作数隐式类型转换后为`false`，则依次后推至`true`的操作数并返回
	>  3. 如果操作数都是`null`，则返回`null`
	>  4. 如果操作数都是`NaN`，则返回`NaN`
	>  5. 如果操作数都是`undefined`，则返回`undefined`

	```js
	2 || 3 || "hello";                 //2
	0 || 3 || "hello";                 //3
	0 || null || NaN || undefined;     //undefined
	0 || undefined || null || NaN;     //NaN
	0 || undefined || NaN || null;     //null
	```

- `!` 非

	> 无论操作数是什么类型，逻辑非`!`都会返回一个布尔值

	> `!!`同时使用两个逻辑非操作符时<br>
	> 1. 第一个逻辑非操作符会基于操作数返回一个布尔值
	> 2. 第二个逻辑非操作符则对该布尔值求反

	```js
	!0   //true
	!1   //false
	!""   //true
	!"hello"   //false
	!NaN   //true
	!null   //true
	!undefined   //true

	!!""   //false
	!!"hello"   //true
	```
