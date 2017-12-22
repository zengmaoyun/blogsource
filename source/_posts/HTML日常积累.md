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

### 4、移动端左右滚动
不要忘记了最简单最流畅的左右滚动就是源生的scroll-x: sroll;
去掉滚动条的属性：
```
::-webkit-scrollbar {
    display: none; //这条
    width: 0;
    background: 0 0;
}
```


### 5、设置页面不缓存
``` html
<meta http-equiv="Expires" content="0">
<meta http-equiv="Pragma" content="no-cache">
<meta http-equiv="Cache-control" content="no-cache">
<meta http-equiv="Cache" content="no-cache">
```

### 6、标签
别人问我的一个问题，下面代码click为什么不生效
``` html
<!DOCTYPE html>
<html>
	<head></head>
	<body>
	
		<td onclick="alert(1)">复制文本</td>
	
	</body>
</html>
```

**原因：** td只能在table下面有意义，不在table下时，直接解析成了文字，也就没有事件了，将td改成span，onclick生效，或者
``` html
<!DOCTYPE html>
<html>
	<head></head>
	<body>
		<table>
			<td onclick="alert(1)">复制文本</td>
		<table>
	</body>
</html>
```