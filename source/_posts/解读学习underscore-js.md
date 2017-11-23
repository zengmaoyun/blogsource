---
layout: _posts
title: 解读学习underscore.js
date: 2017-09-21 14:56:45
tags: [underscore,js]
description: Underscore一个JavaScript实用库，提供了一整套函数式编程的实用功能，但是没有扩展任何JavaScript内置对象.看源码学习，记录知识点
---

## 学习underscore源码
+ 1、用void 0 代替undefined
**原因**
1）undefined 并不是保留词（reserved word），它只是全局对象的一个属性，在低版本 IE 中能被重写。undefined 在 ES5 中已经是全局对象的一个只读（read-only）属性了，不能被重写。但是在局部作用域中，还是可以被重写的。

2）void 运算符对任何内容求值返回都是undefined，void 0相比undefined字节更小

+ 2、Object.keys(obj)(源生) —— 参数是对象或者数组，返回一个数组，包含obj里面的所有key值，如果是数组返回的是从0开始的值。
``` javascript
Object.keys([1,2,3])  //[0,1,2]
Object.keys({a:1,b:2,c:3})   //[a,b,c]
```

+ 3、for in 遍历对象 —— 遍历出对象的可枚举属性
`可枚举属性包括：`
直接的赋值和属性初始化的属性
覆盖原型上面的方法属性，如toString
`不可枚举的属性：`
原型上面的方法和属性
用defineProperty()方法设置的属性
`判断一个属性是否可枚举`
源生方法，Object.propertyIsEnumerable
``` javascript
let obj = {toString: null};
obj.propertyIsEnumerable("toString")  //true ， 为true是可枚举出来，为false不能枚举出来，有bug
```
**Tips** IE9以下枚举bug：for in 不能枚举出重写的原型上的方法(比如toString)，上面的方法在用来检测是否能枚举出重写的原型方法，也就是IE6/7/8浏览器
**问题** 我在IE11里面模拟的IE6/7/8，上面的函数都能返回true，也能枚举出来toString，这个很奇怪？？？

+ 4、每个函数都是一个Function对象
判断一个变量是否是对象：
``` javascript
//判断是否是对象	 —— 要排除null的情况
function isObject(obj) {
	var type = typeof obj;
	return type === "function" || type === "object" && !!obj;
}
```

+ 5、Object.hasOwnProperty  ——  判断属性是否是对象的自有属性
``` javascript
var hasOwnProperty = Object.prototype.hasOwnProperty;
function hasKey(obj, key) {
	return obj != null && hasOwnProperty.call(obj, key);
}
```

+ 6、Int8Array，在ES2017中定义 
8 位整数值的类型化数组
``` javascript
typeof Int8Array // function
```

+ 7、typeof 正则
``` javascript
typeof /s/ === 'function'; // Chrome 1-12 , 不符合 ECMAScript 5.1
typeof /s/ === 'object'; // Firefox 5+ , 符合 ECMAScript 5.1
```

+ 8、length是数组或者类数组才有的属性，object.length 返回的是undefined
是否是数组，这里和jquery里面的方法有不同的地方，需要对比看看
``` javascript
const MAX_ARRAY_INDEX = Math.pow(2, 53) - 1;
function isArrayLike(collection) {
	let length = !!collection && "length" in collection && collection.length;
	return typeof length === "number" && length > 0 && length < MAX_ARRAY_INDEX
	//或者
	return Object.prototype.toString.call(collection) == "[object Array]";
}
```

+ 9、call和apply
call 比 apply 快很多
.apply 在运行前要对作为参数的数组进行一系列检验和深拷贝，.call 则没有这些步骤