---
layout: _posts
title: css3动态提示效果
date: 2017-06-09 14:18:33
tags: CSS3
---

------
### 1、transition
```
transition：all 1s linear 1s;
```
<!-- more -->
#### 动画过渡
transition-property：规定设置过渡效果的 CSS 属性的名称
transition-property: none|all|property;
默认：all
值:
> * none：所有都不平滑
> * all：表示所有属性变换都产生平滑的效果
> * css属性名称，如width，height，相应的会产生平滑变化的效果，用逗号隔开
        
transition-duration：规定动画完成所用时间
默认：0，意味着没有效果
要有动画效果，必须设置时间值
    值：时间，单位s

transition-timing-function：规定动画的速度曲线
默认：ease
值：
> * ease：慢速-快-慢速
> * linear：匀速
> * ease-in：慢速开始
> * ease-out：慢速结束
> * ease-in-out：慢速开始和结束
> * cubic-bezier(n,n,n,n)：n在0-1
       
transition-delay：规定动画何时开始，延迟多久
默认：0

兼容性：
    safari支持-webkit-transition
    IE9以及之前的IE不支持tansition

//兼容性写法
```
-moz-transition: width 2s; /* Firefox 4 */
-webkit-transition: width 2s; /* Safari 和 Chrome */
-o-transition: width 2s; /* Opera */
```
### 2、transform
#### 1）兼容性
```
transform:rotate(7deg);
-ms-transform:rotate(7deg); 	/* IE 9 */
-moz-transform:rotate(7deg); 	/* Firefox */
-webkit-transform:rotate(7deg); /* Safari 和 Chrome */
-o-transform:rotate(7deg); 	/* Opera */
```
    opera和IE9只支持2D

#### 2）translate
位移，x轴，y轴，z轴
```
translate(x,y)，translateX(x)，translateY(y)，translateZ(z)，translate3d(x,y,z)
transform : translate();
translateZ() 和 translate3d()，是3d效果
```
#### 3）rotate()
绕轴旋转
```
transform: rotate(angle)
```
angle：旋转角度
> * rotate(angle)：2d旋转和rotateZ(angle)相同效果，绕Z轴旋转
> * rotateX(angle)：绕x轴旋转，3d旋转效果
> * rotateY(angle)：绕y轴旋转，3d旋转效果
> * rotateZ(angle)：绕z轴旋转，2d旋转效果

#### 4）skew()
倾斜转换
> * skew(x-angle,y-angle)：沿着x轴和y轴倾斜转换
> * skewX(angle)：沿着x轴倾斜
> * skewY(angle)：沿着y轴倾斜

倾斜效果的时候注意倾斜的对象，比如div倾斜，但是div较大，div里面的文字的倾斜效果就看着不明显
或者可以直接把倾斜效果写到文字上

5）scale()
缩放
> * scale(x,y)：定义2d缩放，x和y轴缩放的比例
> * scaleX(x)：定义x轴缩放比例
> * scaleY(y)：定义y轴缩放比例
> * scale3d(x,y,z)：定义3d缩放
> * scaleZ(z)：定义z轴3d缩放


