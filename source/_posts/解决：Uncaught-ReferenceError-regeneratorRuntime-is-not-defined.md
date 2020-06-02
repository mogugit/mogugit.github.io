---
title: '解决：Uncaught ReferenceError: regeneratorRuntime is not defined'
date: 2020-06-02 15:02:44
tags:
---
在使用 vuex actions 里的 异步函数 （async）时，出现**regeneratorRuntime is not defined** 错误

问题：
	**使用 ES7 的 async/await 时报错。**

**原因：regeneratorRuntime在浏览器上无法识别，需要安装一个
babel-plugin-transform-runtime插件**
<!-- more -->
安装插件
```javascript
npm i --save-dev babel-plugin-transform-runtime
```

在 **.babelrc** 文件中添加
```javasript
"plugins": [
	[
	  "transform-runtime",
	  {
	    "helpers": false,
	    "polyfill": false,
	    "regenerator": true,
	    "moduleName": "babel-runtime"
	  }
	]
]
```
