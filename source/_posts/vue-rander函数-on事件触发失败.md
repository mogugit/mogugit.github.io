---
title: vue rander函数 on事件触发失败
date: 2020-06-02 16:00:57
tags:
---
在编写组件时 使用rander函数编写组件 发现事件没有被触发后来发现写的方式不对
错误的使用
````javascript
// js
render(h){
	return h(
	'div',
	{
	  'class': {
        foo: true,
        bar: false
      },
      style: {
        // color: 'red',
        fontSize: '14px',
        width: '100px',
        // height: '20px',
        backgroundColor: '#bf0000'
      },
      attrs: {
        id: 'foo'
      },
      // 需要手动匹配 keyCode。
      on: {
        click: (data)=>{
       		this.$emit("click",data)
       	 },
        visiableChange:value => {
        	 //这里有一个事件名称不是单单词 但是我们on监听时候发现就触发不了emit
            this.$emit('visible-change', value);
        }
      },
    },
    [
      '一些内容',
      createElement('h1', '一条文字')
    ]
  )
}

````
<!-- more -->
正确使用
```javascript
render(h){
	return h(
	'div',
	{
	  'class': {
        foo: true,
        bar: false
      },
      style: {
        // color: 'red',
        fontSize: '14px',
        width: '100px',
        // height: '20px',
        backgroundColor: '#bf0000'
      },
      attrs: {
        id: 'foo'
      },
      // 需要手动匹配 keyCode。
      on: {
        click: (data)=>{
       		this.$emit("click",data)
       	 },
        "visiable-change":value => {
        	 //这里的解决办法就是使用引号包裹一下，就解决非单单词事件名称
            this.$emit('visible-change', value);
        }
      },
    },
    [
      '一些内容',
      createElement('h1', '一条文字')
    ]
  )
}
```
**问题：**
	**render编写组件 非单单词无法触发emit事件**

**解决办法：**

	将非单单词修改为原生事件名称层 然后用引号包裹一下
