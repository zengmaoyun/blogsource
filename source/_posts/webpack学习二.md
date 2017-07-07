---
layout: _posts
title: webpack学习(二)
date: 2017-07-06 10:59:46
tags: [前端构建工具,webpack]  
description: webpack生成文件的调试，webpack打包css模块，热更新，loader使用等~~
---

## webpack调试
>* 打包后的文件有时候你是不容易找到出错了的地方对应的源代码的位置的
>* Source Maps就是来帮我们解决这个问题的。

通过简单的配置，Webpack在打包时可以为我们生成的source maps，为我们提供了一种对应编译文件和源文件的方法，使得编译后的代码可读性更高，也更容易调试。
### 介绍devtool
在webpack的配置文件：`webpack.config.js`中配置source maps，需要配置`devtool`
`devtool`参数：
>* source-map : 在一个单独的文件中产生一个完整且功能完全的文件。这个文件具有最好的source map，但是它`会减慢打包文件的构建速度`；
>* cheap-module-source-map : 在一个单独的文件中生成一个不带列映射的map，不带列映射提高项目构建速度，但是也使得浏览器开发者工具只能对应到具体的行，不能对应到具体的列（符号），`会对调试造成不便`；
>* eval-source-map : 使用eval打包源文件模块，在同一个文件中生成干净的完整的source map。这个选项可以在不影响构建速度的前提下生成完整的sourcemap，但是对打包后输出的JS文件的执行具有性能和安全的隐患。不过在`开发阶段这是一个非常好的选项`，但是在`生产阶段一定不要用这个选项`；
>* cheap-module-eval-source-map : 这是在打包文件时最快的生成source map的方法，生成的Source Map 会和打包后的JavaScript文件同行显示，没有列映射;

上述选项由上到下`打包速度越来越快`，不过同时也具有越来越多的负面作用，较快的构建速度的后果就是对打包后的文件的的`执行有一定影响`。
在学习阶段以及在小到中性的项目上，`eval-source-map`是一个很好的选项，不过记得`只在开发阶段使用它`。

>* cheap-module-eval-source-map 方法构建速度更快，但是不利于调试，推荐在大型项目考虑da时间成本是使用。

### 配置devtool
``` javascript
module.exports = {
  devtool: 'eval-source-map',//配置生成Source Maps，选择合适的选项
  entry:  __dirname + "/app/main.js",
  output: {
    path: __dirname + "/public",
    filename: "bundle.js"
  }
}
```

## webpack构建本地服务
目的：览器监测你的代码的修改，并自动刷新修改后的结果。
实现：一个单独的组件（webpack-dev-server）—— Webpack提供一个可选的本地开发服务器，这个本地服务器基于node.js构建。
### 安装：
``` javascript
npm install --save-dev webpack-dev-server
```
### 参数介绍
devserver作为webpack配置选项中，有下面的配置：

>* contentBase : 默认webpack-dev-server会为根文件夹提供本地服务器，如果想为另外一个目录下的文件提供本地服务器，应该在这里设置其所在目录（本例设置到“public"目录）
>* port : 端口，默认8080
>* inline : true —— 源文件改变时自动刷新页面
>* colors : true —— 终端输出的文件为彩色的
>* historyApiFallback : true —— 所有的跳转将指向index.html，在开发单页应用时非常有用，它依赖于HTML5 history API


### 配置devtool
``` javascript
module.exports = {
  devtool: 'eval-source-map',

  entry:  __dirname + "/app/main.js",
  output: {
    path: __dirname + "/public",
    filename: "bundle.js"
  },

  devServer: {
    contentBase: "./public",//本地服务器所加载的页面所在的目录
    colors: true,//终端中输出结果为彩色
    historyApiFallback: true,//不跳转
    inline: true//实时刷新
  } 
}
```

## Loaders