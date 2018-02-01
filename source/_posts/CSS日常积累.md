---
layout: _posts
title: CSS日常积累
date: 2017-08-23 17:01:16
tags: [积累,css]
description: 工作学习中遇到的css知识点的积累
---

## CSS知识点

### 1、html5页面屏蔽复制
``` css
-webkit-user-select: none; //设置了这个属性之后，网页不可复制
```

### 2、解决android里面不支持0.5px的border
**描述：** 在app改版需求中的一个H5页面（服务页），UI要求边框设为0.5px，开始是在IOS里面看效果，一切正常边框细了，在安卓里面看的时候，边框呢~_~？
**问题：** android里面0.5px的border无效
**解决：** 从网上获取到的方法，用after模拟边框，设置为1px，再用scale缩小0.5倍，定位到原来的边框位置。
宽高设置为200%，left和top是-50%，scale缩放0.5。

```css
.call-info .telbtn-in:after{
	content: "";
  	display: block;
 	position: absolute;
  	left: -50%;     //定位
  	top: -50%;
  	margin-top: -0.08rem;
  	margin-left: -0.1rem;
  	width: 200%;      ///宽高设置为200%
	height: 200%;
 	border: 1px solid #BBB;    //border为1px
 	-moz-border-radius: 30px;
	-webkit-border-radius: 30px;
	border-radius: 30px;
    transform: scale(0.5);      //缩小0.5
	-webkit-transform: scale(0.5); /* Chrome, Safari, Opera */
	-ms-transform: scale(0.5); /* IE 9 */
	-moz-transform: scale(0.5); 	/* Firefox */
	-o-transform: scale(0.5); 	/* Opera */
}
```
+ `具体实现`（如果页面内容还没变的话）：58同城APP > 二手车 > 底部导航点服务 > 拓展服务的边框，以及拨打记录的电话边框

### 3、不要随便给图片设置heigh:100%~~要注意

### 4、input等标签获得焦点的时候去掉默认边框（蓝色或者黄色的）
``` css
input {
	outline: none;
}
```


### 5、选择非第一个的p元素
``` css
 p:not(:first-child) {
    margin-top: pxToRem(20px);
}
```

### 6、隐藏滚动条
``` css
.scroll {
	height: 500px;
	overflow: auto;
}

::-webkit-scrollbar {/*隐藏滚轮*/
	display: none;
}
```

### 7、移动端流畅滚动
开发移动端页面设计滚动的时候一定加上
-webkit-overflow-scrolling: touch;
否则IOS的滚动会很卡

### 8、p元素里面一串标点符号不换行的情况
**描述：** 在一个车源详情页的时候，发现p元素里面的文字描述，结尾是一串感叹号的时候，没换行，直接出来边框
**解决：** 给这个p元素加上样式：
``` css
word-break: break-word;
```

### 9、加蒙层之后，底部body不滚动
底部设置
```
document.body.style.overflow = "hidden";
document.body.style.position = "fixed";
    	
document.body.style.overflow = "auto";
document.body.style.position = "static";
```