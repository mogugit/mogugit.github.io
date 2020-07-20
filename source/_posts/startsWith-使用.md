---
title: startsWith() 使用
date: 2020-07-20 13:58:27
tags:
  - startsWith()
  - js 实用方法
---

`startsWith()` 方法用于检测字符串是否以指定的子字符串开始。

如果是以指定的子字符串开头返回 `true`，否则 `false`。

`startsWith()` 方法对大小写敏感。
```js
	var str = "Hello world";
	var n = str.startsWith("Hello");
	console.log(n) // true
```
<!-- more -->
##### 语法
```js
	string.startsWith(searchvalue, start)
```
##### 参数值
| 参数 | 描述 |
|:---- |:-----|
|  searchvalue  |  必需，要查找的字符串。|
|  start  |  可选，查找的开始位置，默认为 0。|

##### 返回值
|返回值|
|:--|
| 如果字符串是以指定的子字符串开头返回 true，否则 false。 |

```js
	var str = "Hello world";
	var n = str.startsWith("o", 4);
	console.log(n) // true
```


