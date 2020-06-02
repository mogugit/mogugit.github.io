---
title: 谈谈 Object.prototype.toString
date: 2020-06-02 13:17:35
tags:
---
#### ECMAScript 5
在ECMAScript 5中，Object.prototype.toString()被调用时，会进行如下步骤：
- 如果 `this` 是 `undefined` ，返回 `object Undefined` ；
- 如果 `this `是 `null` ， 返回 `object Null` ；
- 令 `O` 为以 `this` 作为参数调用 `ToObject`的结果；
- 令 `class` 为 `O` 的内部属性 `[[Class]]` 的值；
- 返回三个字符串 "[`object"`, `class`, 以及`"]"` 拼接而成的字符串。
#### [[Class]]
`[[Class]]` 是一个内部属性，值为一个类型字符串，可以用来判断值的类型。

<!-- more -->
有这么一段详细的解释：
>本规范的每种内置对象都定义了 [[Class]] 内部属性的值。宿主对象的 [[Class]] 内部属性的值可以是除了 “Arguments”, “Array”, “Boolean”, “Date”, “Error”, “Function”, “JSON”, “Math”, “Number”, “Object”, “RegExp”, “String” 的任何字符串。[[Class]] 内部属性的值用于内部区分对象的种类。注，本规范中除了通过 Object.prototype.toString ( 见 15.2.4.2) 没有提供任何手段使程序访问此值。

在JavaScript代码里，唯一可以访问该属性的方法就是通过 `Object.prototype.toString` ，通常方法如下：
```js
	Object.prototype.toString.call(value)
```
例：
```js
	Object.prototype.toString.call(null)
	// => '[object Null]'

	Object.prototype.toString.call(undefined)
	// => '[object Undefined]'

	Object.prototype.toString.call(Math)
	// => '[object Math]'

	Object.prototype.toString.call({})
	// => '[object Object]'

	Object.prototype.toString.call([])
	// => '[object Array]'

	Object.prototype.toString.call(0)
	// => '[object Number]'

	Object.prototype.toString.call(NaN)
	// => '[object Number]'

	Object.prototype.toString.call('')
	// => '[object String]'
```
因此，可以用下列函数，来获取任意变量的`[[Class]]`属性：
```js
	function getType (val) {
	  const _str = Object.prototype.toString.call(val)
	  return /^\[object (.*)\]$/.exec(_str)[1]
	}
```
运行：
```js
	getClass(null)
	// => 'Null'

	getClass(undefined)
	// => 'Undefined'

	getClass(Math)
	// => 'Math'

	getClass({})
	// => 'Object'

	getClass([])
	// => 'Array'
```
#### ECMAScript 6
在ES6，调用 `Object.prototype.toString` 时，会进行如下步骤：
- 如果 `this` 是 `undefined` ，返回 `'[object Undefined]' `;
- 如果 `this` 是 `null` , 返回 `'[object Null]'` ；
- 令 `O` 为以 `this` 作为参数调用 `ToObject` 的结果；
- 令 `isArray` 为 `IsArray(O)` ；
- `ReturnIfAbrupt(isArray)` （如果 `isArray` 不是一个正常值，比如抛出一个错误，中断执行）；
- 如果 `isArray` 为 `true` ， 令 `builtinTag` 为 `'Array'` ;
- `else` ，如果 `O is an exotic String object` ， 令 `builtinTag` 为 `'String'` ；
- `else` ，如果 `O` 含有 `[[ParameterMap]] internal slo`, ， 令 `builtinTag 为 'Arguments'` ；
- `else` ，如果 `O` 含有 `[[Call]] internal method` ， 令 `builtinTag` 为 `Function` ；
- `else` ，如果 `O` 含有 `[[ErrorData]] internal slot` ， 令 `builtinTag 为 `Error` ；
- `else` ，如果 `O` 含有 `[[BooleanData]] internal slot` ， 令 `builtinTag` 为 `Boolean` ；
- `else` ，如果 `O` 含有 `[[NumberData]] internal slot` ， 令 `builtinTag` 为 `Number` ；
- `else` ，如果 `O` 含有 `[[DateValue]] internal slot` ， 令 `builtinTag 为 Date` ；
- `else` ，如果 `O` 含有 `[[RegExpMatcher]] internal slot` ， 令 `builtinTag` 为 `RegExp` ；
- `else` ， 令 `builtinTag` 为 `Object` ；
- 令 `tag` 为 `Get(O, @@toStringTag)` 的返回值（ `Get(O, @@toStringTag`) 方法，既是在 `O` 是一个对象，并且具有 `@@toStringTag` 属性时，返回 `O[Symbol.toStringTag]` ）；
- `ReturnIfAbrupt(tag)` ，如果 `tag` 是正常值，继续执行下一步；
- 如果 `Type(tag) `不是一个字符串，`let tag be builtinTag` ；
- 返回由三个字符串 `"[object", tag, and "]"` 拼接而成的一个字符串。

在ES6里，之前的` [[Class]]` 不再使用，取而代之的是一系列的 `internal slot` ，有一个比较完整的解释：
>Internal slots correspond to internal state that is associated with objects and used by various ECMAScript specification algorithms. Internal slots are not object properties and they are not inherited. Depending upon the specific internal slot specification, such state may consist of values of any ECMAScript language type or of specific ECMAScript specification type values

大概的意思是：Internal slots 对应于与对象相关联并由各种ECMAScript规范算法使用的内部状态，它们没有对象属性，也不能被继承，根据具体的 Internal slot 规范，这种状态可以由任何ECMAScript语言类型或特定ECMAScript规范类型值的值组成。

此外，通过对 Object.prototype.toString 在ES6的实现步骤分析，我们其实可以很容易改变 Object.prototype.toString.call 的结果，像下面一样：
```js
	let obj = {}

	Object.defineProperty(obj, Symbol.toStringTag, {
	    get: function() {
	        return "newClass"
	    }
	})

	console.log(Object.prototype.toString.call(obj)) // "[object newClass]"
```
#### 参考：
1) [http://www.ecma-international.org/ecma-262/5.1](http://www.ecma-international.org/ecma-262/5.1)
2) [http://www.adobe.com/devnet/archive/html5/articles/categorizing-values-in-javascript.html](http://www.adobe.com/devnet/archive/html5/articles/categorizing-values-in-javascript.html)
3) [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/toString](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/toString)
4) [http://www.ecma-international.org/ecma-262/6.0/](http://www.ecma-international.org/ecma-262/6.0/)
5) [http://es6.ruanyifeng.com/#docs/symbol](http://es6.ruanyifeng.com/#docs/symbol)
6) [https://tc39.github.io/ecma262/#sec-object.prototype.tostring](https://tc39.github.io/ecma262/#sec-object.prototype.tostring)
