---
layout: _posts
title: JS日常积累
date: 2017-07-11 22:46:31
tags: [积累,js]
description: 工作学习中遇到的js知识点的积累
---

## js内容

### 1、object作为参数传递

这个问题是来自同学，问到下面这段代码这个输出的结果：
``` javascript
function setName(obj){
	obj.name="name2";
	obj = new Object()
	obj.name = "name3";
}

var obj = new Object();
obj.name= "name1";
setName(obj);
console.log(obj.name);  //name2
```
最后输出的结果是"name2"
原理：`当对象obj作为一个参数传递的时候，其实传递的是它的引用`
我们在函数中对其属性的操作会反应到外部对象obj上，
但是上面的代码 new Object()//或者是其他任何的赋值，都是相当于新建了一个局部变量obj，而不会去改变参数的引用，
接下来改变name的操作对象已经变成了新建的局部变量

在网上搜到一篇文章对此做了很好的解释：[js中一个对象当做参数传递是按值传递还是按引用](http://www.cnblogs.com/xljzlw/p/4399414.html)

### 2、Object.defineProperty()
语法：
```
Object.defineProperty(obj, prop, descriptor)
```
参数说明：
+ obj：必需。要修改的对象
+ prop：必需。要创建或者修改的属性的名称
+ descriptor：必需。属性描述符对象，可以是属性特性（value、writable、enumerable、configurable）,或者是存储器特性（get、set、enumerable、configurable）
vue的双向绑定的实现原理使用了这个方法。

### 3、insertAdjacentHTML /insertAdjacentElement /insertAdjacentText
语法：
```
element.insertAdjacentHTML(position, element);

targetElement.insertAdjacentElement(position, element);

element.insertAdjacentText(position, text);
```
**position**: 
+ beforebegin : 在该元素前插入
+ afterbegin : 在该元素第一个子元素前插入
+ beforeEnd：在该元素最后一个子元素后面插入
+ afterEnd：在该元素后插入

```
<!-- beforebegin -->
<p>
  <!-- afterbegin -->
  foo
  <!-- beforeend -->
</p>
<!-- afterend -->
```

### 4、Function属性

+ 1)、func.caller) —— 返回调用指定函数的函数 **非标准**，用函数的arguments.callee.caller代替
函数在全局作用于下被调用的时候，arguments.callee.caller为null，否则指向调用这个函数的函数
检测一个函数被谁调用
```
function checkCaller() {
	if(arguments.callee.caller == null) {
		console.log("全局windows下调用");
	} else {
		console.log("在函数" + arguments.callee.caller + "中调用");
	}
}
```

+ 2)、Function.length —— length 是函数对象的一个属性值，指该函数有多少个必须要传入的参数
已定义了默认值的参数不算在内，比如function a(xx = 0)的a.length是0

+ 3)、arguments.length 返回的是函数被调用时实际传参的个数

例子
```
Function.length   // 1
(function() {}).length  // 0
(function(a) {}).length  // 1
(function(a, b) {}).length  //2
(function(...args) {}).length  // 0 , 剩余参数(rest parameter) 不计入length
(function(a, b=2, c) {}).length  // 1, length只计算默认值之前的参数个数

```

+ 4)、function.name —— 返回函数实例的名称

```
function doSomething(){}
doSomething.name  // "doSomething"
```
 
+ 5)、Function.prototype —— 存储Function的原型对象，不能被修改

+ 6)、Object.prototype.constructor —— 返回一个指向创建了该对象原型的函数引用

+ 7)、Function.prototype.apply()
```
fun.apply(thisArg, [argsArray])
```
**thisArg**
在 fun 函数运行时指定的 this 值。
需要注意的是，指定的 this 值并不一定是该函数执行时真正的 this 值，如果这个函数处于非严格模式下，则指定为 null 或 undefined 时会自动指向全局对象（浏览器中就是window对象），同时值为原始值（数字，字符串，布尔值）的 this 会指向该原始值的自动包装对象。
**argsArray**
一个数组或者类数组对象，其中的数组元素将作为单独的参数传给 fun 函数。
如果该参数的值为null 或 undefined，则表示不需要传入任何参数。

### DOMContentLoaded事件
当初始HTML文档被完全加载和解析完成之后，DOMContentLoaded 事件被触发，而无需等待样式表、图像和子框架完成加载。
**load对比**：load事件用于检测一个完全加载的页面
使用：
```
document.addEventListener("DOMContentLoaded", function(event) {
  console.log("DOM fully loaded and parsed");
});
```


### 5、各种距离计算（scroll中）
```
//页面滚动距离：
window.pageYOffset|| document.documentElement.scrollTop || document.body.scrollTop;
//元素距离页面顶部距离
ele.offsetTop
//元素高度
ele.offsetHeight
//元素宽度
ele.offsetWidth
```

### DOM节点
获取孩子节点
```
//children获取不包括空格节点的元素节点
ele.children
//childNodes获取包括空文本节点的节点数组
ele.childNodes
```