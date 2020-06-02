---
title: js String padStart()自动补全操作
date: 2020-06-02 14:48:07
tags:
---
###### 字符串自动补全函数 注意一定是字符串！
[padStart(targetLength [, padString]) ](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/padStart)方法用另一个字符串填充当前字符串(重复，如果需要的话)，以便产生的字符串达到给定的长度。填充从当前字符串的开始(左侧)应用的。
**targetLengt** 目标长度
**padString** 补充的字符串
<!-- more -->
```javascript
var str1 = "1";
// 补充两位 场景：日期时间
var _str1 = sr1.padStart(2, '0') // '01'

// 补充多位 场景：单据号
var receiptNO = '1';
var _receiptNO = receiptNO.padStart(16, '0'); // '000000000000001';

```
###### 另外补充浏览器不支持的情况下需要添加Polyfill
如果原生环境不支持该方法，在其他代码之前先运行下面的代码，将创建 String.prototype.padStart() 方法。
```javascript
// https://github.com/uxitten/polyfill/blob/master/string.polyfill.js
// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/padStart
if (!String.prototype.padStart) {
    String.prototype.padStart = function padStart(targetLength,padString) {
        targetLength = targetLength>>0; //floor if number or convert non-number to 0;
        padString = String((typeof padString !== 'undefined' ? padString : ' '));
        if (this.length > targetLength) {
            return String(this);
        }
        else {
            targetLength = targetLength-this.length;
            if (targetLength > padString.length) {
                padString += padString.repeat(targetLength/padString.length); //append to original to ensure we are longer than needed
            }
            return padString.slice(0,targetLength) + String(this);
        }
    };
}
```
