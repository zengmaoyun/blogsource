---
layout: _posts
title: git常用命令
date: 2017-10-16 19:25:54
tags: [git]
description: GIT常用命令
---

## 丢掉本地修改，拉去远程代码
//全部丢掉
```
git checkout .
```

//丢掉单个文件
```
git checkout head.js
```


## 冲突了出现(分支|MERGING)
运行下面就可以了
```
git reset --hard head 
```
