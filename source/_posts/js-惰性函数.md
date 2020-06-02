---
title: js-惰性函数
date: 2020-06-02 14:01:09
tags:
---
利用闭包思想
原来定义一个函数来根据情况执行
<!-- more -->
```js
// DOM2 事件绑定 DOM事件参考链接：DOM级别事件
	// 元素.addEventListener();
	// 元素.attachEvent();
function emit(element,type,func) {
	if(element.addEventListener) {
		element.addEventListener(type,func,false)
	}else if (element.attachEvent){
		element.attachEvent('on'+type,func)
	} else {
		element['on'+type] = func
	}
}

emit(box,'click',func);
emit(box,'click',func);
```
使用闭包
```js
function emit(element,type, func){
	if (element.addEventListener) {
		emit = function(element,type,func){
			element.addEventListener(type,func,false)
		}
	} else if(element.attachEvent) {
		emit = function(element,type,func){
			element.attachEvent('on'+type, func);
		}
	} else {
		emit = function(element,type,func){
			element['on'+type] = func
		}
	}
	emit(element,type,func)
}
emit(box,'click',func);
emit(box,'click',func);
```
两者的区别
前者在添加事件时每次都会去判断当前环境兼容性；
而使用了闭包思想的惰性函数之后,在多次调用后，可以大大节省后面的判断步骤；
