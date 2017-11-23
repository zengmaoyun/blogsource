---
layout: _posts
title: JS基础知识
date: 2017-11-13 20:34:36
tags: [JS]
description: js：变量，类型，类型转换，作用域，this、闭包，es6、原型链等基础知识
---

### 数据类型

#### JavaScript 类型转换
+ 转换为数字：Number()
+ 转换为字符串：String() 
+ 转化为布尔值： Boolean()

#### JavaScript 数据类型

5种数据类型: string 、  number 、  boolean  、 object 、 function

3中对象类型： Object 、  Date 、 Array

2个不包含任何值的数据类型：  null 、  undefined
	
基本数据类型：null、 undefined 、 string 、 boolean、  number

#### Tips

+ NaN 的数据类型是 number
+ 数组(Array)的数据类型是 object
+ 日期(Date)的数据类型为 object
+ null 的数据类型是 object
+ 未定义变量的数据类型为 undefined
+ typeof function () {}  是function

#### constructor属性
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

#### constructor使用
用constructor来确认数组和日期
```
myArray.constructor.toString().indexOf("Array") > -1;
myArray.constructor.toString().indexOf("Date") > -1;
```

#### Number对象方法
+ toExponential(x): 把对象的值转换为指数计数法。
+ toFixed(x): 把数字转换为字符串，结果的小数点后有指定位数的数字。
+ toPrecision(x) : 把数字格式化为指定的长度。
+ toString() : 把数字转换为字符串，使用指定的基数。
+ valueOf()	: 返回一个 Number 对象的基本数字值。

exp:
numberObject.toExponential(2)

Number("323asd") // NaN


#### 使用运算符类型转化

+ 加号"+"，减号"-" ，可以转化数字字符串为数字

exp:
``` javascript
var x = + "22"  //22
var x = + "2wsd" //NaN
```

var x = "5" - 0  //5
var x = 5 + null //5,  null转化为了0


#### 自动转化
当你尝试输出一个对象或一个变量时 JavaScript 会自动调用变量的 toString() 方法：

``` javascript
document.getElementById("demo").innerHTML = myVar;

// if myVar = {name:"Fjohn"}  // toString 转换为 "[object Object]"
// if myVar = [1,2,3,4]       // toString 转换为 "1,2,3,4"
// if myVar = new Date()      // toString 转换为 "Fri Jul 18 2014 09:08:55 GMT+0200"
```