---
title: vue开发 cross-env跨平台设置环境变量
date: 2020-06-02 15:56:47
tags:
---
# vue开发 cross-env跨平台设置环境变量
vue开发过程成中使用环境变量 以便在不同的环境里面查看不同的数据。
cross-env可以很方便帮我们解决
关于介绍 [cross-env](https://www.npmjs.com/package/cross-env)

首先安装 cross-env
```
npm install cross-env --save-dev
```
<!-- more -->
使用
在package.json 里面配置环境变量名称
```javascript
// config/proxyDev.js 代理地址一
const proxyDev = {
  '/api': {
    target: 'http://xxxxxxxxxxxx', // 代理的接口地址
    secure: false // 是否验证SSL
  }
};
module.exports = proxyDev;
```
```javascript
// config/proxyTestCenter.js 代理地址二
const proxyTestCenter= {
  '/api': {
    target: 'http://xxxxxxxxxxxxx', // 代理的接口地址
    secure: false // 是否验证SSL
  }
};
module.exports = proxyTestCenter;
```
```javascript
// config/index.js 新增下面代码
const proxyDev= require('./proxyDev'); // 新建文件
const proxyTestCenter= require('./proxyTestCenter'); // 新建文件

let proxyPath;
switch (process.env.proxyPath) { // 根据命令来切换环境变量
  case 'dev': proxyPath = proxyDev; break;
  case 'tc': proxyPath = proxyTestCenter; break;
}
```
修改**config/index.js** 文件
```javascript
// 修改config/index.js 文件

module.exports = {
	dev: {
		...
		proxyTable: proxyPath, // 将 proxyTable得知替换为上面定义的proxyPath 以便使用命令更换环境变量
		...
	}

	...
}
```
在package.json中配置命令 在package.json中修改如下代码：
```javascript
// package.json 中修改scripts 的dev和rc; 配置方式模板 cross-env key=value 注：这里的key是proxyTable value是dev和tc;
   ...
   "scripts": {
    	"dev": "cross-env proxyTable=dev webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
    	"tc": "cross-env proxyTable=tc webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
		...
  	},
  	...
```
最后在使用不同的编译命令就可以来改变环境变量
```
npm run dev  //执行的是 dev环境变量

npm run tc  //执行的是 tc环境变量
```
参考博文/文档：[^1]
 [http-proxy-middleware 代理]: https://www.npmjs.com/package/http-proxy-middleware
 [cross-env 跨平台设置环境变量]: https://www.npmjs.com/package/cross-env
