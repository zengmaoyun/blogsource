---
layout: _posts
title: 使用vue-cli脚手架开发一个项目
date: 2017-08-08 12:44:02
tags: [vue]
description: 使用vue-cli脚手架开发一个项目的过程，包括如何使用sass等
---

## 安装vue-cli

### 全局安装 vue-cli
```
$ npm install --global vue-cli
```
### 创建一个基于 webpack 模板的新项目
```
$ vue init webpack my-project
```
### 安装依赖
```
$ cd my-project
$ npm install      //yarn install
$ npm run dev
```

## 在项目里面使用sass/scss
### 安装sass-loader 和  node-sass依赖
```
npm install --save-dev sass-loader
//sass-loader运行依赖node-sass
npm install --save-dev node-sass
```

### 组件中使用
```
<style lang="scss">

</style>
```

### 引用scss文件
``` css
<style lang="scss">
	@import "../assets/scss/commom";
	
	.select-box {
		font-size: .75rem;
		.title {
			color: #333;
		}
		.select {
			color: #CCC;
		}
	}
</style>
```
### 引用scss文件遇到的问题
报如下错误：
![报错](/blog/img/article/vue-sass-error.png)

**原因：** @import .scss文件之后没有写分号(;)了
**TIP：** 使用@import引用scss文件必须写分号

## 使用ajax请求数据

### 使用promise请求数据（不跨域）
新建一个js：ajax.js，代码如下：
``` javascript
export default ({type="GET", url="", data={}, async=true} = {}) => {
	return new Promise((resolve,reject) => {
		type = type.toUpperCase();
		
		let requestObj = new XMLHttpRequest();
		
		/* 兼容IE5、6的代码
		 * let requestObj;
		if (window.XMLTttpRequest) {
			requestObj = new XMLHttpRequest();
		}else { 
			//IE5 和 IE6
			requestObj = new ActiveXObject;
		}
		*/
		
		if (type == "GET") {
			let dataStr = '';
			//拼接URL
			Object.keys(data).forEach(key => {
				dataStr += key + '=' + data[key] + '&';
			});
			dataStr = dataStr.substr(0,dataStr.lastIndexOf('&'));
			url = url + '?' + dataStr;
			requestObj.open(type, url, async);
			requestObj.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
			requestObj.send();
		}else if(type == 'POST') {
			requestObj.open(type, url, async);
			requestObj.setRequestHeader("content-type", "application/x-www-form-urlencoded");
			requestObj.send(JSON.stringify(data));
		}else {
			reject('type error');
		}
		//返回状态
		//onreadystatechange 是一个事件句柄。它的值是一个函数，当 XMLHttpRequest对象的状态发生改变时，会触发此函数
		//状态从 0 (uninitialized) 到 4 (complete) 进行变化。状态4表示解析完成，我们开始执行代码
		requestObj.onreadystatechange = () => {
			if(requestObj.readyState == 4) {  //4表示解析完毕
				if(requestObj.status == 200) {
					let obj = requestObj.response;
					if(typeof obj !== 'object') {
						obj = JSON.parse(obj);
					}
					resolve(obj);
				}else {
					reject(requestObj);
				}
			}
		}
	})
}
```

参数使用解构赋值，在vue文件里面使用这个方法获取数据。
ajax(resdata)方法返回的是一个promise对象，要加上.then(function(){})，获得data数据并处理

``` javascript
openCityBox: function(){
	let that = this;
	this.citybox = true;
	let resdata = {
		"url": "static/city.json"
	};
	//处理数据
	ajax(resdata).then((data) => {
		that.cityarr = data.city;
	});
}
```

## 组件之间通信

### 父子组件之间通信
父子组件：父组件传递值给子组件、子组件通过props来接收数据
在vue2.0开始不允许子组件直接修改父组件的值，否则会报错
**原理** 在我看来，这个原理等同于在js中给一个函数传递参数（obj），允许在函数里面修改这个参数内部变量的属性值（如obj.data）,但是不能直接修改参数（obj）的值，因为这**相当于是改变了参数的指针**，是不允许的。
**结论** 在vue里面我们不能直接修改props接收的变量的值，但是可以修改这个变量的内部变量的值
要想在子组件能够修改变量的值
**解决方法**
1、在子组件里面创建一个新的变量保存props里面的值
``` javascript
props: ['tflag'],
data() {
	return {
		hflag: this.tflag
	}
}
```

2、方法触发改变hflag的值
``` javascript
methods: {
	changeTab: function(type) {
		this.hflag = type;
	}
}
```

3、监听hflag的变化，如果变化，触发父组件事件，传变化值为参数
``` javascript
watch: {
	hflag: function(value) {
		this.$emit("change-tflag",value);
	}
}
```

4、在父组件里面创建事件钩子，监听变化的事件
``` javascript
created() {
	let that = this;
	//切换标题
	that.$on("change-tflag",(val) => {
		that.changeTflag(val);
	});
}
```

5、所有代码：
**父组件：App.vue**

``` html
<template>
  <div id="app">
  	<Lhead :tflag="tflag"></Lhead>
  </div>
</template>

<script>
	import Lhead from './components/Lhead';
	
	export default {
	  	name: 'app',
	  	data() {
			return {
				tflag: 'city'
			}
			},
	  	components: {
  			Lhead
	  	},
	  	created() {
	  		let that = this;
	  		//切换标题
	  		that.$on("change-tflag",(val) => {
	  			that.changeTflag(val);
	  		});
	  	},
	  	methods: {
  			changeTflag: function(val) {
  					this.tflag = val;
  			}
	  	}
	}
</script>

<style lang="scss"></style>

```

**子组件Lhead.vue**
``` html
<template>
	<section class="header">
		<div class="title-box clearfix">
			<span :class="[hflag == 'city' ? 'active' : '']" @click="changeTab('city')">城市查询</span>
			<span :class="[hflag == 'standard' ? 'active' : '']" @click="changeTab('standard')">标准查询</span>
		</div>
	</section>
</template>

<script>
	export default {
		props: ['tflag'],
		data() {
			return {
				hflag: this.tflag
			}
		},
		methods: {
			changeTab: function(type) {
				this.hflag = type;
			}
		},
		watch: {
			hflag: function(value) {
				this.$parent.$emit("change-tflag",value);  //要用$parent
			}
		}
	}
	
</script>

<style lang="scss"></style>
```
### 非父子组件之间通信
非父子组件之间通信，通过一个新的Vue示例作为事件中心来实现
#### 方法一
1、新建一个文件：newvue.js
``` javascript
let eventBus = new Vue(); //创建事件中心
/*
 export default new Vue();
 */
```
2、在要通信的组件里面引用
``` javascript
import eventBus from '/newvue'
```
组件1触发：
``` javascript
methods: {
	changeCity() {
		eventBus.$emit("change","北京");
	}
}
```
组件2接收：
``` javascript
created() {
	let that = this;
	eventBus.$on("change", (city) => {
		that.city = city;
	});
}
```

上面的是通用方法，但是要新建一个文件创建新的vue实例，每次都需要引用很不方便。方法二实现一个更加通用的事件中心
#### 方法二
在方法一上优化事件中心（new Vue()实例）
在main.js里面直接把新的实例定义成这个实例的变量。
``` javascript
new Vue({
  el: '#app',
  data() {
  	return {
  		eventBus: new Vue()      //直接定义     
  	}
  },
  template: '<App/>',
  components: { App }
})
```

在 组件里面不需要import新的vue实例的文件，直接用this.$root.eventBus触发事件。
子组件：
```
this.$root.eventBus.$emit("change","北京");
```

## 创建全局方法
在项目开发中，组件之间会有重复的方法，这时候就需要把方法提取出来，作为公共方法。
### 实现
1、创建一个js文件util.js
在util里面像下面这样创建方法，把方法添加到Vue的原型中
``` javascript
/*公用方法库*/
export default {
	install(Vue, options) {
		Vue.prototype.change = (value) => {
			 // do something here
		},
		...
	}
}
```

2、在main.js里面引用

``` javascript
import Vue from 'vue'
import App from './App'
import util from './config/util'   //***

//全局方法
Vue.use(util);     //** 必须使用这一步 

```

3、现在，在我们的vue组件里面就可以直接使用change这个方法了，和使用methods里面的方法一样，直接使用