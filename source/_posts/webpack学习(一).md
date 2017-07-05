---
layout: _posts
title: webpack学习(一)
date: 2017-06-14 13:42:50
tags: [前端构建工具,webpack]  
description: 什么是webpack，webpack有什么作用，怎么使用webpack
---

## 什么是webpack
Webpack是一个前端资源加载/打包工具。
学习参考了简书上的一个教程：[webpack教程](http://www.jianshu.com/p/42e11515c10f)
### 作用
它将根据模块的依赖关系进行静态分析，然后将这些模块按照指定的规则生成对应的静态资源。
可以将多种静态资源 js、css、less 转换成一个静态文件，减少了页面的请求。
### 工作方式
Webpack的工作方式是：
把你的项目当做一个整体，通过一个给定的主文件（如：index.js），Webpack将从这个文件开始找到你的项目的所有依赖文件，使用loaders处理它们，最后打包为一个浏览器可识别的JavaScript文件。

## 开始使用webpack
### 安装
新建一个文件夹：webpackexam，打开文件夹，新建package.json文件：默认主文件是index.js
>* 新建package.json文件：`npm init`
>* 局部安装webpack: `npm install webpack --save-dev`


`【遇到的问题】`：最开始使用了webpack作为文件夹名，在初始化package.json时，默认`"name"`:`"webpack"`,在局部安装webpack时，
`报错`：不能在名为webpack的文件夹中安装webpack，（文件夹名不能与包名相同）因此把文件夹名字改成了webpackexam（`注`：全局安装没问题）。


`【全局安装】`：我是看着网上的教程来学习的，教程上安装webpack包的时候是先安装全局，再安装局部，不明白为什么要安装全局的，所以只安装了局部的，继续学习之后才知道了为什么要安装全局的（下面解释）。
`原因`：在打包依赖的时候使用命令`webpack source-path dist-path`，如果不安装全局的，必须制定webpack的具体路径（在node_modules/.bin/webpack文件夹里面）
安装全局：`npm install webpack -g`


### 使用
#### 目录结构
![目录结构](/blog/img/article/basedir.png)
>* 新建app文件夹-下面包含文件greeter.js(模块文件)和main.js(主文件)
>* 新建public文件夹(最后打包之后的文件)—下面包含index.html文件，包含一个id=main的div

#### greeter.js里面的代码
``` javascript
module.exports = function(){
	var greet = document.createElement("div");
	greet.innerHTML = "this is a greet info";
	return greet;
}
```

#### main.js里面的代码
``` javascript
//加载依赖greeter.js
var greeter = require("./greeter.js");
document.getElementById("main").appendChild(greeter());
```

#### index.html里面
``` html
<body>
	<div id="main"></div>
</body>
//引用js文件
<script src="./js/bundle.js"></script>
```

### 打包
文件都建好之后就是打包了，按照网上教程学习了三种打包方式，一一来说
1.直接命令行：
```
webpack app/main.js public/js/bundle.js （webpack 源文件path 目标文件path）
```
这里会遇到上面说的是否全局webpack安装的问题：如果不是全局安装，那么webpack要指定到具体目录下（node_modules/.bin/webpack）
——打包完成之后就在public下面新创建了一个文件夹js，下面有bundle.js：js/bundle.js

2.使用配置文件直接命令行webpack:
在项目根目录下面新建文件：webpack.config.js
把输入输出目录文件配置好之后，使用命令行`webpack`，即可打包
__dirname(两个下划线)是node.js中的一个全局变量，它指向当前执行脚本所在的目录。
内容：
``` javascript
module.exports = {
	entry: __dirname + "/app/main.js",   //源目录
	output: {
		path: __dirname + "/public/js",  //输出目录
		filename: "bundle.js"            //输出文件名
	}
}
```

3.使用npm start / npm run {script name} 打包
第三种方法是不使用webpack命令打包，使用npm替代
在package.json文件里面，在scripts里面配置上如下，在命令行中使用npm start就可以执行打包了。
``` javascript
  "scripts": {
    "start": "webpack"  
  },
```
npm的start是一个特殊的脚本名称，它的特殊性表现在：
在命令行中使用npm start就可以执行相关命令，如果对应的此脚本名称不是start，想要在命令行中运行时，需要这样用npm run {script name}如npm run build

如下：
``` javascript
  "scripts": {
    "build": "webpack"  
  },
```
打包时要使用：`npm run build`

【优点】
改方法除了快捷之外还有一个优点，因为package.json本来就和node_module相关，所以使用这个命令打包的时候，webpack可以不用全局安装。

