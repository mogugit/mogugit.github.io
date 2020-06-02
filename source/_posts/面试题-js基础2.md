---
title: 面试题-js基础2
date: 2020-06-02 13:18:41
tags:
---
面试题：当 a 等于什么的时候下面的代码成立？
```js
	// 问： 当 a 等于什么的时候下面的代码成立
	var a = ?;
	if (a == 1 && a == 2 && a == 3){
		console.log('OK')
	}
```

<!-- more -->
方案一：利用比较的时候调用 toString() 方法，来重写 toString() 方法；
```js
	// 写法一：
	var a = {
		i: 0,
		toString(){
			return ++this.i;
		}
	}

	if (a == 1 && a == 2 && a == 3) {
		console.log('OK')
	} // OK

	// 写法二：
	var a = [1,2,3];
	a.toString = a.shift;

	if (a == 1 && a == 2 && a == 3) {
		console.log('OK')
	} // OK
```
方案二：利用[Objext.defineProperty()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty) 来拦截，并改变值；
```js
	let i = 0;
	Object.defineProperty(window, 'a', {
		get(){
			return ++i;
		},
		set(){
		}
	})
	if (a == 1 && a == 2 && a == 3) {
		console.log('OK')
	} // OK
```
###### [关于比较的小知识](https://www.cnblogs.com/litsword/archive/2010/07/22/1782933.html)
==， 两边值类型不同的时候，要先进行类型转换，再比较。
===，不做类型转换，类型不同的一定不等。
NaN 与任何值都不相等(包括 NaN)，不管是双等还是恒等;
```js
 NaN == NaN // false
 NaN === NaN // false
```

具体如下：
**关于 === ：**
1) 如果类型不同，就不相等
2) 如果两个都是数值，并且是同一个值，那么相等(NaN 除外)；
3) 如果两个都是字符串，每个位置的字符都一样，那么相等；否则不相等。
4) 如果两个值都是true，或者都是false，那么相等。
5) 如果两个值都引用同一个对象或函数，那么相等；否则[相等。
6) 如果两个值都是null，或者都是undefined，那么相等。

**关于  == :**
1) 如果两个值类型相同，进行 === 比较。
2) 如果两个值类型不同，他们可能相等。根据下面规则进行类型转换再比较：
a) 如果一个是null、一个是undefined，那么相等。
b) 如果一个是字符串，一个是数值，把字符串转换成数值再进行比较。
c) 如果任一值是 true，把它转换成 1 再比较；如果任一值是 false，把它转换成 0 再比较。
d) 如果一个是对象，另一个是数值或字符串，把对象转换成基础类型的值再比较。对象转换成基础类型，利用它的toString或者valueOf方法。 js核心内置类，会尝试valueOf先于toString；
e) 任何其他组合，都不相等。
```js
	false == 1 // 会先把 false 转化为数字 0 然后比较
	1 ==  true // 会先把 true 转化为数字 1 然后比较
```

**总结就是如下：**
**1) NaN 和谁都不相等;
2) null和undefind 两个等号相等 三个等号不相等;
3) 对象 == 字符串, 会把对象转化为字符串 (对象.toString());
4) 其余情况都是转化为数字进行比较 对象转化为数字 Number(对象.toString());**
