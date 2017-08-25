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

