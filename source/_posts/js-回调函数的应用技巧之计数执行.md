---
title: js - 回调函数的应用技巧之计数执行
date: 2020-10-15 11:15:46
tags:
  - js应用技巧
  - 函数回调应用
---
在一些业务场景会有两个独立函数共同修改一个对象，而我们又不知道对象什么何时完成的时候，就要用回调函数来进行解决。

如：有两个单独文本文件，我们需要获取里面的内容来填充一个对象，我们在填充完成之后来获取对象。

获取文本文件并填充
<!-- more -->
```js
	let fs = require('fs');

	function fn(acount, callBack){
	  let obj = {};
	  return function(key, data){
	    obj[key] = data;

	    // 计数自减
	    if(--acount == 0){
	      callBack(obj);
	    } else {
	      console.log('未执行完毕');
	    }
	  }
	}

	function getData(data){
	  console.log('获取完毕', data);
	}

	// 设置目标次数以及回调函数；
	let newFn = new fn(2, getData);

	// 获取数据1: 张三
	fs.readFile('./1.text', 'utf8', function(err, data){
	  if(err){
	    console.log(err);
	    return;
	  }
	  newFn('name', data);
	});

	// 获取数据2: 18
	fs.readFile('./2.text', 'utf8', function(err, data){
	  if(err){
	    console.log(err);
	    return;
	  }
	  newFn('age', data);
	})

```
应用场景为 需要多次调用、异步执行，但是不知道结果是否符合的情况下，可以指定次数来解决问题；
