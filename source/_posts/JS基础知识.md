---
layout: _posts
title: JS基础知识
date: 2017-11-13 20:34:36
tags: [JS]
description: js：变量，类型，类型转换，作用域，this、闭包，es6、原型链等基础知识
---

[变量](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Grammar_and_types)
## 变量（variable）
作用域区分：全局变量，局部变量（let申明的语句块作用域）

### base
变量的名字又叫做标识符，其需要遵守一定的规则。
变量组成：字母、下划线（_）或者美元符号（$） 和 数字 ，大部分 ISO 8859-1 或 Unicode 编码的字符 或  Unicode 转义字符
变量开头：字母、下划线（_）或者美元符号（$）

### undefined
+ 布尔环境中：变成false
+ 数字环境中：变成NaN
``` javascript
var a;
a + 1; //NaN
```

### null
+ 布尔环境中：变成false
+ 数字环境中：变成0

``` javascript
var a = null;
a + 1; //1
```

### 变量提升（Hoisting）
Hoisting 真正发生的是在编译阶段将变量和函数声明放入内存中，但仍然保留在编码中键入的位置。
ES6 : let不存在 Hoisting
[变量提升](https://developer.mozilla.org/zh-CN/docs/Glossary/Hoisting)

### 函数提升
只有函数声明会被提升到顶部，函数表达式不会
``` javascritp
function foo(){} //会提升

boo // undefined;
var boo = function() {}   //函数表达式，只会提升boo
```


### 申明变量
声明变量是它所在上下文环境的不可配置属性，非声明变量是可配置的（如非声明变量可以被删除）
``` javascript
var a = 1;
b = 2;
delete this.a   //严格模式报错，其他没反应
delete this.b   //删除成功
```
 
## 常量（Constants）
用const定义
同一个作用域下，`不能`使用与变量名或函数名相同的名字来命名常量。

**TIPS**
+ 必须初始化一个值
+ 常量不可通过赋值修改
+ 常量如果是一个对象，对象的属性可以改变（不能修改对象的引用）

 
## 数据类型

### JavaScript 数据类型

基本数据类型：null、 undefined 、 string 、 boolean、  number

可识别7中数据类型：String、 Number 、 Boolean 、 null 、 undefined 、 Symbol 、 Object

引用数据类型：Object

3中对象类型： Object 、  Date 、 Array

2个不包含任何值的数据类型：  null 、  undefined

js的常见内置对象：Date, Array, Math, Number, Boolean, String, Array, RegExp, Function

js两个基本要素：object、 function
+ object: 存放值的命名容器
+ function: 应用程序执行的过程

### Tips

+ NaN 的数据类型是 number
+ 数组(Array)的数据类型是 object
+ 日期(Date)的数据类型为 object
+ null 的数据类型是 object
+ 未定义变量的数据类型为 undefined
+ typeof function () {}  是function

### constructor属性
返回所有javascript变量的构造函数
``` javascript
"John".constructor                 // 返回函数 String()  { [native code] }
(3.14).constructor                 // 返回函数 Number()  { [native code] }
false.constructor                  // 返回函数 Boolean() { [native code] }
[1,2,3,4].constructor              // 返回函数 Array()   { [native code] }
{name:'John', age:34}.constructor  // 返回函数 Object()  { [native code] }
new Date().constructor             // 返回函数 Date()    { [native code] }
function () {}.constructor         // 返回函数 Function(){ [native code] }
```

### constructor使用
用constructor来确认数组和日期
```
myArray.constructor.toString().indexOf("Array") > -1;
myArray.constructor.toString().indexOf("Date") > -1;
```

### Number对象方法
+ toExponential(x): 把对象的值转换为指数计数法。
+ toFixed(x): 把数字转换为字符串，结果的小数点后有指定位数的数字。
+ toPrecision(x) : 把数字格式化为指定的长度。
+ toString() : 把数字转换为字符串，使用指定的基数。
+ valueOf()	: 返回一个 Number 对象的基本数字值。

exp:
numberObject.toExponential(2)

Number("323asd") // NaN


## 类型转换

+ 转换为数字：Number()
+ 转换为字符串：String() 
+ 转化为布尔值： Boolean()

### 使用运算符类型转换

+ 加号"+"，减号"-" ，可以转化数字字符串为数字
**单目加法运算符**
exp:
``` javascript
var x = ( + "22" )  //22
var x = ( + "2wsd" ) //NaN
```

var x = "5" - 0  //5
var x = 5 + null //5,  null转化为了0


### 自动转换
当你尝试输出一个对象或一个变量时 JavaScript 会自动调用变量的 toString() 方法：

``` javascript
document.getElementById("demo").innerHTML = myVar;

// if myVar = {name:"Fjohn"}  // toString 转换为 "[object Object]"
// if myVar = [1,2,3,4]       // toString 转换为 "1,2,3,4"
// if myVar = new Date()      // toString 转换为 "Fri Jul 18 2014 09:08:55 GMT+0200"
```


## this（执行环境上下文）

[MDN-this-LINK](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/this)

严格模式下，this为执行上下文
``` javascript
function f2(){
  "use strict"; // 这里是严格模式
  return this;
}

f2() // undefined
window.f2()  //window
```

### this传递
使用call或者apply
``` javascript
func.call(obj, arg1, arg2...)
func.apply(obj, [arg1,arg2...])
```
ES6引入了bind方法
``` javascript
func.bind(obj/this)
```
箭头函数的上下文已经确定（外层执行上下文），不能通过call,apply和bind改变
**TIPS**
箭头函数的this是在`函数调用`过程中设置的；
