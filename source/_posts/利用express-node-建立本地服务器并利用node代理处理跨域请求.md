---
title: 利用express+node 建立本地服务器并利用node代理处理跨域请求
date: 2020-06-02 17:50:44
tags:
---
第一步 下载安装[node.js](https://nodejs.org/en/)
第二步 初始化文件夹  命令: npm init
			然后一路回车
<!-- more -->
```npm
  npm init
```
[![ttvBLQ.png](https://s1.ax1x.com/2020/06/02/ttvBLQ.png)](https://imgchr.com/i/ttvBLQ)
第三步 安装express，ejs, request，
```npm
	npm install express ejs request --save-dev
```
[![ttv0sg.png](https://s1.ax1x.com/2020/06/02/ttv0sg.png)](https://imgchr.com/i/ttv0sg)

在初始化文件下新建nodeServer.js(名称自己定义)；
```
var express = require('express');
var app = express();
var path = require('path');
var request = require("request");//request 封装了 HTTP 请求的各种方法，让发起请求变得非常容易；https://www.npmjs.com/package/request

//指定静态资源访问目录
// app.use(express.static(require('path').join(__dirname, 'views'))); 如果有文件夹存放资源，出现报错的话，那就多use几次就可以了
// 设定views变量，意为视图存放的目录
app.use(express.static(require('path').join(__dirname, './items/dist/')));

// 做代理
app.use('/',function(req,res){
	//拿到请求的路径来代理请求，并把响应的结果传给request客户端然后将目标的服务器响应的数据传回浏览器；
    var url = "http://localhost:3001"+req.url;//http://localhost:3001 这里是我后台服务器的端口；
    req.pipe(request(url)).pipe(res);
});

// app.set('views', __dirname);
// 修改模板文件的后缀名为html
app.set('views', (__dirname + "./items/dist/"));
app.set( 'view engine', 'html' );

// 运行ejs模块
app.engine( '.html', require( 'ejs' ).__express );

app.get("/", function(req, res) {
  res.render('index');
});

var server = app.listen(8889, "10.1.1.114",function(){
  var host = server.address().address;
  var port = server.address().port;
  console.log("Server running at http://%s:%s", host, port)
});

```
最后 运行：node  nodeServer.js
