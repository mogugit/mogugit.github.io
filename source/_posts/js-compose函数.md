---
title: js-compose函数
date: 2020-06-02 13:19:43
tags:
---
#### compose函数
将函数扁平化处理
```js
function fn1(x){
	return x + 1;
}
function fn2(x){
	return x + 10;
}
function fn3(x){
	return x*10;
}
function fn4(x){
	return x/10;
}
// fn1执行然后结果作为fn2参数 一直执行到fn4;
fn4(fn3(fn2(fn1(5)))) // 16
```

<!-- more -->
但是这么写太麻烦了 所以可以简化为这样 使用[reduce 函数](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Objets_globaux/Array/reduce)
```js
function compose(...funs){
	return function proxy(...args){
		let len = funs.length;
		if(len===0){
			return args;
		}
		if(len === 1){
			return funs[0](...args);
		}
		return funs.reduce((x,y)=>{
			return typeof x === 'function' ? y(x(...args)) : y(x);
		})
	}
}
compose()(5); // [5]
compose(fn1,fn2,fn3,fn4)(5) // 16
```
