---
title: JS 设计模式-发布订阅与观察者模式
date: 2020-10-15 17:52:15
tags:
  - 设计模式
  - 发布订阅
  - js应用技巧
---
JS 设计模式之发布订阅模式
<!-- more -->
```js
	// 发布/订阅模式
	function EventEmit(){
	  this._arr = [];
	}
	// 订阅
	EventEmit.prototype.on = function(callBack){
	  this._arr.push(callBack)
	}
	// 取消订阅
	EventEmit.prototype.unon = function(callBack){
	  this._arr.filter(fn=>{
	    if(fn !== callBack){
	      return fn;
	    }
	  })
	}
	// 发布
	EventEmit.prototype.emit = function(){
	  this._arr.forEach(fn=>fn.apply(this, arguments))
	}

	let fn = new EventEmit();
	let obj = {};

	fn.on(function(data,key){
	  obj[key] = data;
	  if(Object.keys(obj).length==2){
	    console.log(obj)
	  }
	})

	fn.emit('张三', 'name' )
	fn.emit(18,'age')
```

JS 设计模式之观察者模式

```js
	// 被观察者/服务
	class Serve {
	  constructor(){
	    this._arr = []
	  }
	  attch(callBack){
	    this._arr.push(callBack)
	  }
	  setState(val){
	    this._arr.forEach(fn=>{
	      fn.update(val)
	    })
	  }
	}

	// 观察者
	class Observe {
	  constructor(props){
	    this.state = props;
	  }
	  update(val){
	    console.log('name-',this.state, 'datra-',val)
	  }
	}

	// 新建观察者
	let o1 = new Observe('name1')
	let o2 = new Observe('name2')

	// 新建服务
	let server = new Serve();

	// 为服务添加观察者；
	server.attch(o1) // name- name1 datra- asdfasd
	server.attch(o2) // name- name2 datra- asdfasd

	// 服务运行观察者记录到
	server.setState('asdfasd')
```
发布/订阅模式和观察者模式的区别在于：

**发布订阅模式是发布和订阅相互独立的，耦合度较松；
观察者模式是将观察者耦合到服务内部，消息触发之后会通知/执行观察者的操作；**
