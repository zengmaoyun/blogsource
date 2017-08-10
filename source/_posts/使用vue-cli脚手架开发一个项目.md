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