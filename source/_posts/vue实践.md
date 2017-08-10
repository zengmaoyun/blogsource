---
layout: _posts
title: vue项目学习
date: 2017-07-18 10:00
tags: [vue,笔记]
description: 在快速看了一遍vue的语法之后，从git克隆一些项目来学习vue的使用，对期间的学习做下笔记。
---

## vue2 + webpack最新配置项目
github路径：[link](https://github.com/zengmaoyun/vue2-config)

## vue学习

### vue-cli脚手架安装
```
# 全局安装 vue-cli
$ npm install --global vue-cli
# 创建一个基于 webpack 模板的新项目
$ vue init webpack my-project
# 安装依赖，走你
$ cd my-project
$ npm install      //yarn install
$ npm run dev
```

### vue语法
+ 基础语法，v-bind、v-on等
+ [*学习文档](https://cn.vuejs.org/v2/guide/)
### vue-router路由
+ 路由
+ [*学习文档](https://router.vuejs.org/zh-cn/)
### vuex
+ 状态管理模式

## 知识点

### vue组件的三种编写方式
#### 1、使用script标签
```html
<!DOCTYPE html>
<html>
    <body>
        <div id="app">
            <my-component></my-component>
        </div>
 <-- 注意：使用<script>标签时，type指定为text/x-template，意在告诉浏览器这不是一段js脚本，
 浏览器在解析HTML文档时会忽略<script>标签内定义的内容。-->
 /*注意 type 和id*/
        <script type="text/x-template" id="myComponent">
            <div>This is a component!</div>
        </script>
    </body>
    <script src="js/vue.js"></script>
    <script>
        //全局注册组件
        Vue.component('my-component',{
            template: '#myComponent'
        })
        new Vue({
            el: '#app'
        })
    </script>
</html>
```

#### 2、使用template标签
``` html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
    </head>
    <body>
        <div id="app">
            <my-component></my-component>
        </div>
/*template下面的div最后实现到了#app下面*/
        <template id="myComponent">
            <div>This is a component!</div>
        </template>
    </body>
    <script src="js/vue.js"></script>
    <script>
        Vue.component('my-component',{
            template: '#myComponent'
        })
        new Vue({
            el: '#app'
        })
    </script>
</html>
```

#### 3、单文件组件*
创建.vue后缀的文件,组件Hello.vue，放到components文件夹中。
``` html
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  </div>
</template>

<script>
export default {
  name: 'hello',
  data () {
    return {
      msg: '你好！'
    }
  }
}
</script>
```
app.vue
```html
<!-- 展示模板 -->
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <hello></hello>
  </div>
</template>

<script>
// 导入组件
import Hello from './components/Hello'

export default {
  name: 'app',
  components: {
    Hello
  }
}
</script>
<!-- 样式代码 -->
<style>
#app {
  /*省略*/
}
</style>
```

### vue单文件组件方式参数

`export default`：是ES6的语法，为模块指定默认输出，在其它模块import的时候可以任意命名。

### webpack 配置
**插件**：html-webpack-plugin
**作用** 这个插件可以帮助生成 HTML 文件，在 body 元素中，使用 script 来包含所有你的 webpack bundles
**参数配置**
+ title: 用来生成页面的 title 元素
+ filename: 输出的 HTML 文件名，默认是 index.html, 也可以直接配置带有子目录。
+ template: 模板文件路径，支持加载器，比如 html!./index.html
+ inject: true | 'head' | 'body' | false  ,注入所有的资源到特定的 template 或者 templateContent 中，如果设置为 true 或者 body，所有的 javascript 资源将被放置到 body 元素的底部，'head' 将放置到 head 元素中。
+ favicon: 添加特定的 favicon 路径到输出的 HTML 文件中。
+ minify: {} | false , 传递 html-minifier 选项给 minify 输出
+ hash: true | false, 如果为 true, 将添加一个唯一的 webpack 编译 hash 到所有包含的脚本和 CSS 文件，对于解除 cache 很有用。
+ cache: true | false，如果为 true, 这是默认值，仅仅在文件修改之后才会发布文件。
+ showErrors: true | false, 如果为 true, 这是默认值，错误信息会写入到 HTML 页面中
+ chunks: 允许只添加某些块 (比如，仅仅 unit test 块)
+ chunksSortMode: 允许控制块在添加到页面之前的排序方式，支持的值：'none' | 'default' | {function}-default:'auto'
+ excludeChunks: 允许跳过某些块，(比如，跳过单元测试的块)
  
**示例**
``` javascript
new HtmlWebpackPlugin({
	filename: 'index.html', /*文件名*/
    template: './src/category/service/index.html',  /*模板*/
    inject: true
}),
```

**html**
通过在配置文件中添加多次这个插件，来生成多个 HTML 文件。