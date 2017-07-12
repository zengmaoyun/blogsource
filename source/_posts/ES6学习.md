---
layout: _posts
title: ES6学习
date: 2017-07-05 13:26:18
tags: [ES6,工具]
description: ES6学习简单记录
---

## 简单记录

1、let命令
声明变量，只在代码块中有效
2、const
声明一个只读的常量，一旦声明，常量的值就不能改变。
`注`：并不是变量的值不得改动，而是变量指向的那个内存地址不得改动。
所以变量的属性可以改变，比如：
``` javascript
const obj = {"name","jike"};
//错误`obj = 1`;
obj.address = "北京";  //正确
```
3、一个将元素彻底冻结的函数
``` javascript
var constantize = (obj) => {
  Object.freeze(obj);
  Object.keys(obj).forEach( (key, i) => {
    if ( typeof obj[key] === 'object' ) {
      constantize( obj[key] );
    }
  });
};
```
1) Object.freeze(obj)
阻止修改现有属性的特性和值，并阻止添加新属性。
2) Object.keys(obj)
返回对象的可枚举属性和方法的名称。

4、数组解构赋值-按照位置解构
``` javascript
let a = 1;
let b = 2;
let c = 3;

ES6 允许写成下面这样。

let [a, b, c] = [1, 2, 3]; 
```

`在这里没看懂的`
事实上，只要某种数据结构具有 Iterator 接口，都可以采用数组形式的解构赋值。

``` javascript
function* fibs() {
  let a = 0;
  let b = 1;
  while (true) {
    `yield a``;
    [a, b] = [b, a + b];
  }
}
let [first, second, third, fourth, fifth, sixth] = fibs();
sixth // 5
上面代码中，fibs是一个 Generator 函数（参见《Generator 函数》一章）
原生具有 Iterator 接口。解构赋值会依次从这个接口获取值。 
```

5、对象的解构赋值-按照属性名解构
或者-不同名的时候
``` javascript
let obj = { first: 'hello', last: 'world' };
let { first: `f`, last: `l` } = obj;
f // 'hello'
l // 'world'
如：
let { foo: baz } = { foo: "aaa", bar: "bbb" };
baz // "aaa"
foo // error: foo is not defined
```
foo是匹配的模式，baz才是变量。真正被赋值的是变量baz，而不是模式foo。

注意，采用这种写法时，变量的声明和赋值是一体的。对于let和const来说，变量不能重新声明，所以一旦赋值的变量以前声明过，就会报错。

6、字符串的扩展
不太懂的地方：
``` javascript
var a = 5;
var b = 10;

tag`Hello ${ a + b } world ${ a * b }`;
// 等同于
tag(['Hello ', ' world ', ''], 15, 50);
```

7、正则的扩展
1）
new RegExp(/abc/ig, 'i').flags —— i，覆盖前面的
2）
flags 属性
ES6 为正则表达式新增了flags属性，会返回正则表达式的修饰符。
``` javascript 
// ES5 的 source 属性
// 返回正则表达式的正文
/abc/ig.source
// "abc"

// ES6 的 flags 属性
// 返回正则表达式的修饰符
/abc/ig.flags
// 'gi'
```