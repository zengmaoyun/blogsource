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