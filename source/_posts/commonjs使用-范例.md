---
title: commonjs使用 范例
date: 2020-06-02 18:04:03
tags:
---
commonJS 规范  千言万语不如一行代码
例
<!-- more -->
```
//example.js
var n = 1;
function sayHello( name ){
	var name = name || "Tom";
	return "Hello~"+name
}
function addFn(val){
    var val = val.x+val.y;
    return val
}
module.exports ={
	n:n,
	sayHello:sayHello,
	addFn:addFn
}

```
使用requier()引入使用

```
//main.js
var example = require('./example.js');
var addNum = {
	"x":10,
	"y":5
}
console.log( example )//查看example输出的对外模块接口；
console.log( example.n )//1;
console.log( example.sayHello("Jack") )// "Hello~ Jack";
console.log( example.addFn(addNum) ) //15;

```

 参考地址：[CommonJS规范](http://javascript.ruanyifeng.com/nodejs/module.html)






