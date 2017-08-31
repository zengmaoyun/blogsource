---
layout: _posts
title: HTML日常积累
date: 2017-08-23 17:04:04
tags: [积累,html]
description: 工作学习中遇到的html知识点的积累
---

## HTML知识点

### 1、网页上禁止左右键
**禁止左键**
```html
<body onselectstart="return false">
```
**禁止右键**
```html
<body oncontextmenu=self.event.returnValue=false>
```
**禁止左右键**
```html
<body oncontextmenu=self.event.returnValue=false onselectstart="return false">
```

### 2、两列自适应布局

**1)**
`左模块` — float:left
`右模块` — margin-left: 

### 3、字母导航（点击定位，滚动切换字母）

**1、点击定位**
点击字母-页面滚动到相应位置的实现方法

+ 1、锚点
兼容IE8-加id
缺点：页面刷新的时候带#xxx会对当前页面有影响，如果涉及相关hash的操作，可能出现一些不可预料的问题
``` html
<a href="#A">A</a>
...文字省略
<div name="A" id="A" ></div>
```
+ 2、scrollTop定位
``` javascript
let anchor = $el.querySelector(selector);   //要定位到的div 
document.body.scrollTop = anchor.offsetTop;
//切换当前字母颜色
to do...
```

**2、滚动切换字母**
### 1、滚动列表每个li元素高度固定的情况
获得屏幕滚过的高度+li高度+第一个li的offsetTop，计算出来当前是第几个li在屏幕顶端，显示这个li的字母导航
### 2、滚动列表每个li元素高度不固定的情况
使用el.getBoundingClientRect().top方法，每次事件遍历所有元素，计算距离屏幕上边的高度，如果在0到负el.offsetHeight之间
那么奇幻字母导航为这个el的字母