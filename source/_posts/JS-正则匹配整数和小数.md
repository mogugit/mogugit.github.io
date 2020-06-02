---
title: JS 正则匹配整数和小数
date: 2020-06-02 15:01:03
tags:
---
正则匹配正整数和小数
```javascript
let _check = /^([1-9][\d]{0,6}|0)(\.[\d]{1,2})?$/; //限制小数点前后位数
let _check1 = /^([1-9][\d]*|0)(\.[\d]+)?$/; //不限制小数点前后位数
_check.test('0.10') // true
_check.test('000.10') // false
_check.test('0') // true
_check.test('9') // true
_check.test('9.9') // true
_check.test('9.90') // true
_check.test('9.900') // false
_check.test('90')  // true
_check.test('090')  // false
_check.test('9..90')  // false
_check.test('9.9.0') // false
_check.test('009') // false
_check.test('0009.90') //false
_check.test('9000000')  // false
```
<!-- more -->
