---
title: HTML5 拖拽
date: 2020-06-02 17:45:24
tags:
---
1、设置元素可拖拽属性；
```
	<img draggable="true">
```
<!-- more -->
2、拖动什么 - ondragstart  设置被拖拽元素 需要传递的数据  需要用onondragstart 来监听事件；
```
// html
...
<img draggable="true" onondragstart ="drap(event)">
...
// javascript
<script>
...
	// dataTransfer.setData(format,data)
	// 方法设置被拖数据的数据类型和值
	// 参数说明： format 要传递的数据类型；data 传递的数据

	function drap(event){
		// 具体使用可以看场景而定 也可以自己设置定义的数据 通过全局声明来使用
		event.dataTransfer.setData('text',ev.target.id)
	}
<script>
```
3、放到何处 - ondragover
ondragover 事件规定在何处放置被拖动的数据。
默认地，无法将数据/元素放置到其他元素中。如果需要设置允许放置，我们必须阻止对元素的默认处理方式。
这要通过调用 ondragover 事件的 event.preventDefault() 方法：
```
<div ondragover="allowDrop(event)" ></div>
// javascript
<script>
	...
	function allowDrop(event){
		// 阻止默认事件
		event.preventDefault()
	}
<script>
```
4、进行放置 - ondrop
当放置被拖数据时，会发生 drop 事件。ondrop 属性调用了一个函数，drop(event)：
```
<div ondragover="allowDrop(event)" ondrop="drop(event)"></div>
// javascript
<script>
	...
	function drop(event){
		// 阻止默认事件
		event.preventDefault()
	}
	function allowDrop(event){
		// 阻止默认事件
		event.preventDefault()
	}
<script>
```

完成的demo
```
<!DOCTYPE HTML>
<html>
<head>
<meta charset="utf-8">
<title>Html5-拖动</title>
<style type="text/css">
#div1 {width:350px;height:70px;padding:10px;border:1px solid #aaaaaa;}
</style>
<script>
	function allowDrop(ev)
	{
	    ev.preventDefault();
	}

	function drag(ev)
	{
	    ev.dataTransfer.setData("Text",ev.target.id);
	}

	function drop(ev)
	{
	    ev.preventDefault();
	    var data=ev.dataTransfer.getData("Text");
	    ev.target.appendChild(document.getElementById(data));
	}
</script>
</head>
<body>

	<p>拖动图片到矩形框中:</p>

	<div id="div1" ondrop="drop(event)" ondragover="allowDrop(event)"></div>

	<img id="drag1" src="/images/logo.png" draggable="true" ondragstart="drag(event)" width="336" height="69">

</body>
</html>


```
