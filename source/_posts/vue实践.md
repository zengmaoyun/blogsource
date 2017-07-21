---
layout: _posts
title: vue项目学习
date: 2017-07-18 10:00
tags: [vue,笔记]
description: 在快速看了一遍vue的语法之后，从git克隆一些项目来学习vue的使用，对期间的学习做下笔记。
---

## vue学习

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

