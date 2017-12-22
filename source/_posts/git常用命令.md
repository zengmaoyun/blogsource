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

## 分支
### 查看分支
```
git branch -a
```

### 创建分支
```
git branch maoamo
```

### 切换分支
```
git checkout maomao
```

创建本地分支maomao 并切换到maomao分支
```
git checkout -b maomao
```

### 推送本地分支到远程分支
```
git push origin local_branch:remote_branch
一般写：
git push origin local_branch:local_branch   //远程创建分支local_branch
```

### 删除本地分支
```
git branch -d <branch名>
```