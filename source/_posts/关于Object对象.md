---
layout: _posts
title: 关于Object对象
date: 2017-11-23 11:24:35
tags: [js,object]
description: 关于object相关内容的记录积累
---

#### 1、Object.getPrototypeOf(obj)
返回obj对象的所有prototype

``` javascript
Object.getPrototypeOf(obj)
```

#### 2、Object.getOwnPropertyNames(obj)

返回一个数组，由指定对象的所有`自身属性`的属性名（包括枚举和不可枚举属性但不包括Symbol值作为名称的属性）组成。
数组中枚举属性的顺序与通过 for...in 循环（或 Object.keys）迭代该对象属性时一致。数组中不可枚举属性的顺序未定义。

#### 3、Object.defineProperty() 
直接在一个对象上定义一个新属性，或者修改一个对象的现有属性， 并返回这个对象。
``` javascript
//obj: 要改变的对象
//prop: 要定义或修改的属性的名称。
//descriptor: 将被定义或修改的属性描述符。
Object.defineProperty(obj, prop, descriptor)

描述符：
{
  enumerable: false,
  configurable: false,
  writable: false,
  value: "static"
}
```