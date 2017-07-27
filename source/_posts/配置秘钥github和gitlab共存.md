---
layout: _posts
title: github一台电脑配置多个秘钥
date: 2017-07-27 11:09:34
tags: [github]
description: 公司的电脑用的gitlab账号，那么如何在公司的电脑上也能用上自己的github账号呢？这篇博客记录了我在公司电脑上配置github秘钥的过程，以及过程中遇到的问题。
---

## 【github第二个秘钥】【配置历程】

参照网上的配置，新增一个秘钥（我是在C盘windows下面的user里面的.ssh文件里面操作的）：
### 1、新建秘钥：
右键打开git bash 
``` javascript
$ ssh-keygen -t rsa -C "wode@gmail.com(我的邮箱)"
```
+ 回车之后，输入名字，如果不输入生成默认的id_rsa，会把原来公司的秘钥覆盖掉，比如这里输入：id_rsa_github
+ 回车之后输入提交密码，直接回车表示不设置密码，在pull和push的时候就不用输入密码了，否则每次都得输入。

### 2、配置config
新建config文件（不带后缀的只能用命令行新建）
```
$ touch config
```

在config文件中配置（相当于路由）
``` javascript
#myself
#self-github
Host github        //*****Host随便写，用在clone项目的时候
    HostName github.com
    PreferredAuthentications publickey
    User git
    IdentityFile ~/.ssh/id_rsa_github      //***** 这个写自己创建的公钥的名字

#company
Host gitlab.com
    HostName gitlab.com
    PreferredAuthentications publickey
    User git
    IdentityFile ~/.ssh/id_rsa

```
### 3、将公钥id_rsa_github添加到github中
### 4、连接测试（可以不做）
$ ssh –T github (ssh -T 之后的内容是命名的Host，会提示成功)
### 5、clone项目（更改了方式）
** 原来： **
+ $ git clone **git@github.com**:zengmaoyun/zengmaoyun.github.io.git

** 现在：**
把前面git@github.com，改成Host的内容
+ $ git clone **github**:zengmaoyun/zengmaoyun.github.io.git

配置时参考的博文：[link](https://segmentfault.com/a/1190000009572470)


## 【遇到的问题】
### 下面就是一直导致我clone不成功的问题
+ 公司的账号是gitlab，之前公司配置的是全局的账号（用户名和邮箱）

** 全局配置 **
``` javascript 
$ git config --global user.name "username"
$ git config --global user.email "email"
```

** 任意目录查询 **
``` javascript
$ git config user.name  —— 结果是公司的用户名
$ git config --global user.email ——结果是公司的邮箱
```

** 出现的问题 **
配置秘钥之后，clone github上面的项目，一直提示：确定有操作权限？

** 原因 **
全局账号：相当于在公司账号下clone我自己的github上的项目，所以失败了

** 解决方法 **
 在要放置自己项目的文件夹里面单独配置账户：
 ``` javascript
git config --global user.name "（github账户名称）"  //配置你的账户名字
git config --global user.email "github的邮箱"     //配置你的创建github账户的邮箱
```
这时候查看用户信息：
 ``` javascript
git config user.name    //github名称
git config user.email   //github邮箱
```

此时clone github上的项目就成功了
