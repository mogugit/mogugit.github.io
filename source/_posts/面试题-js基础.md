---
title: 面试题-js基础
date: 2020-06-02 13:18:07
tags:
---
下面的代码输出结果是：
```js
  let obj = {
    2:3,
    3:4,
    length: 2,
    push: Array.prototype.push
  }

  obj.push(1);
  obj.push(2);
  console.log(obj)
```
<!-- more -->
分析：
obj是一个对象里面有四个属性，包括push 方法，正常情况下对象是没有push() 方法的，那么push()方法做什么？或者怎么实现一个 push() 方法？
如下：
```js
	// 实现简易版 push() 方法, 原理：在数组的末尾添加传入的值，改变数组长度并返回数组;
	Array.prototype.push = function(num){
		// this 指向当前Array
		// 默认会使 length 属性加1, 如果没有 length 属性那么 push() 方法会默认 length为0；
		this.length = this.length || 0;
		return this[this.length] = num;
	}
	let arr = [1,2];
	arr.push(3); // => arr = [1,2,3];
```
通过上面的 push() 解析，可以得知：
```js
	obj.push(1) // => 执行this[this.length] length为2 this[2] = 1;
	obj.push(2) // => 执行this[this.length] length为3 this[3] = 2;
	console.log(obj) // => obj = {2:1, 3:2, length: 4, push: Array.prototype.push}
```



