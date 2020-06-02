---
title: Clipboard.js 实现点击复制
date: 2020-06-02 16:18:07
tags:
---
在开发过程中难免会遇到点击分享的需求，这里有两种实现方式：
**第一种：** 通过原生js 的方法用（**存在兼容性**）：

**document.execCommand(** aCommandName, aShowDefaultUI, aValueArgument**)**
参数说明：
		**aCommandName**
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;命令的名称：常用的为"copy","cut"等；

注："copy"  拷贝当前选中内容到剪贴板
<!-- more -->
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "cut"&nbsp;&nbsp;  剪贴当前选中的文字并复制到剪贴板
		**aShowDefaultUI**
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;是否展示用户界面，一般为 false；

&nbsp;**aValueArgument**
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认为null;

**返回值:Boolean** 如果还是false 则表示还不能支持；

html ：
```html
<input type="text" id="copyVal" readonly value="被复制内容" />
<button class="copyBtn" >点击复制</button>
```
javascript：

```javascript
var copyBtn = document.getElementsByClassName('copyBtn')[0];

	copyBtn.onclick = function(){
	    var copyVal = document.getElementById("copyVal");
	        copyVal.select();

	    try{
	        if(document.execCommand('copy', false, null)){
	            //success info
	            console.log("doSomething...");
	        } else{
	            //fail info
	            console.log("doSomething...");
	        }
	    } catch(err){
	        //fail info
	        console.log("doSomething...");
	    }
	}
```
具体兼容如下-pc 端浏览器：
![tt2EEd.png](https://s1.ax1x.com/2020/06/02/tt2EEd.png)
![tt2kHH.png](https://s1.ax1x.com/2020/06/02/tt2kHH.png)
移动端浏览器-ios:
	目前是不支持所有浏览器，包括微信浏览器；

**第二种** 使用clipboard.js 实现(**个人推荐**)：
 它是一个不需要Flash,就能实现文本复制或者剪切到剪切板的轻量级插件；
 其中需要两个参数是
 > data-clipboard-action 是操作类型值为复制（copy ），剪切（cut） 默认为copy 可以选择不加该属性
 > data-clipboard-target 是要复制或者剪切的对象的id值  必选选项

 具体实例：
 可以使用[cdn](https://github.com/zenorocha/clipboard.js/wiki/CDN-Providers) 或者[直接下载](https://github.com/zenorocha/clipboard.js/archive/master.zip) 设置好引用路径
```
<script type="text/javascript" src="./dist/clipboard.min.js"></script>
```
html

```html
<input type="text" id="copyVal" readonly value="被复制内容" />

<button class="copyBtn"   data-clipboard-target="#copyVal">点击复制</button>
```
javascript

```javascript
		//实例化 ClipboardJS对象;
        var copyBtn = new ClipboardJS('.copyBtn');

        copyBtn.on("success",function(e){
            // 复制成功
            alert(e.text);
            e.clearSelection();
        });
        copyBtn.on("error",function(e){
            //复制失败；
            console.log( e.action )
        });
```
#### 这里的ClipboardJS在实例化时， 如果报错：`clipboard is not defined`

#### 解决办法就是如下：
```javascript
new ClipboardJS(obj)
```
#### 原因就是 Clipboard.JS版本是2.0及以上版本



