---
title: js - 回调函数的应用技巧之函数预处理
date: 2020-10-15 15:05:30
tags:
  - js应用技巧
  - 函数回调应用
  - 前置函数
  - 函数预执行
---

#### 回调函数的应用技巧之函数预处理
函数前置
<!-- more -->
```js
	// 前置函数 在当前函数执行前面增加一个函数实现再不更改原函数的情况下 对目标进行预处理;
	Function.prototype.before = function(callBack){
	  let that = this; // 将 this 指向原函数；
	  return function(){
	    callBack() // 执行 前置函数
	    that.apply(that, arguments) // 执行原函数
	  }
	}

	// 原函数
	function fn(data){
	  console.log('原函数', data)
	}

	// 回调操作
	function cb(){
	  console.log('callBack')
	}

	// 为原函数增加前置函数 before
	let newFn = fn.before(cb)

	// 执行函数
	newFn(123) // '原函数 123'

```
**应用场景：在不更改原函数的情况下对目标进行预处理**
