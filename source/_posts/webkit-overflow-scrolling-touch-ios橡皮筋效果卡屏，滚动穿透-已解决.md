---
title: 'webkit-overflow-scrolling:touch; ios橡皮筋效果卡屏，滚动穿透 --已解决'
date: 2020-06-02 15:52:54
tags:
---
#### -webkit-overflow-scrolling 属性
[MDN中概述](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty) 入下
>-webkit-overflow-scrolling 属性控制元素在移动设备上是否使用滚动回弹效果.

##### 值选项
> 1、auto
>   使用普通滚动, 当手指从触摸屏上移开，滚动会立即停止
>   2、touch
>   使用具有回弹效果的滚动, 当手指从触摸屏上移开，内容会继续保持一段时间的滚动效果。继续滚动的速度和持续的时间和滚动手势的强烈程度成正比。同时也会创建一个新的堆栈上下文。
<!-- more -->
#### 问题 BUG
##### 1、使用了-webkit-overflow-scrolling:touch之后，页面偶尔会卡住不动。
   **复现场景**：发现页面在滚动期间是不会出现卡住的问题，但是在滚动条到顶部或者底部的时候会出现这个问题
   **分析问题**：如果在到达顶部/底部的时候不让它到达顶部/底部,是不是可以规避这个问题
   **解决办法**: 在滑动的时候为滑动区域添加 ```scroll事件```监听滚动条是否到了底部/顶部 如果到了顶部/底部那么让```scrollTop```[^1]相应的减少 1；
   代码如下：
```javascript
// js 文件

// 获取滚动区域
let el = document.getElementById('scrollSectionArea')
// 获取滚动内容的高度 如果是在是vue 使用动态的获取高度请将获取元素替换为 ref 来获取,不然是获取不到高度的 在监听函数内部注意this指向
let scrollHeight =  document.getElementById('scrollSection').scrollHeight;

el.addEventListener('scroll', function() {
// 如果滚动条到顶部/底部则scrollTop值相应的减 1
	if (el.scrollTop <= 1) {
	  el.scrollTop = 1;
	}
	if ((el.scrollTop + el.offsetHeight) >= (scrollHeight - 1)) {
	  el.scrollTop = scrollHeight - el.offsetHeight - 1;
	}
});
```
2、使用了-webkit-overflow-scrolling:touch之后，弹窗会引发底部滑动（滚动穿透）。
  **复现场景**：弹窗带有滚动内容 滑动到底部或者顶部会触发底层的滚动；
   **分析问题**：滚动事件穿透，
   **解决办法**: 弹窗显示的时候固定底部滚动并记录底层scrollTop值，并将底部内容设置为position: fixed;
  height: 100%; 来达到固定底层的目的，然后就可以规避滚动穿透；
  代码如下：
 ```css
 // css
 /* 解决点击穿透滑动失效 */
.mask_show {
  position: fixed;
  height: 100%;
}
  ```
```javascript
// js部分
data:{
	return {
	...
		eventScrollTop: 0
	...
	}
}

methods:{
	// 弹窗显示的时候调用 记录底部滚动条位置
	setScrollTopValue() {
		this.eventScrollTop = document.scrollingElement.scrollTop ||
		                document.documentElement.scrollTop ||
		                document.body.scrollTop;
		document.body.classList.add('mask_show ');
		document.body.style.top = -this.eventScrollTop + 'px';
	},
	// 弹窗关闭的时候调用 回归到原来滚动条位置
	useScrollTopValue() {
	    document.body.classList.remove('mask_show');
	    document.scrollingElement.scrollTop = document.documentElement.scrollTop = document.body.scrollTop = this.eventScrollTop;
	}
}
```

[^1]: 底部 scrollTop = scrollHeight- offsetHeight
**offsetHeight**：包括元素的边框、内边距和元素的水平滚动条（如果存在且渲染的话），不包含:before或:after等伪类元素的高度。
**scrollHeight**：值与元素视图填充所有内容所需要的最小值clientHeight相同。包括元素的padding，但不包括元素的border和margin。scrollHeight也包括 ::before 和 ::after这样的伪元素。

参考文档：[滚动穿透问题的解决方案](https://mp.weixin.qq.com/s?__biz=MzUyNDYxNDAyMg==&mid=2247484280&idx=1&sn=63ae69e55f3590c42f52c208db3bc3be&chksm=fa2be391cd5c6a877a6ab37d17067a2a1998ffdc9af42aa4a1cda7f93a6b7f457529eb64896b&mpshare=1&scene=1&srcid=&key=8df06d6b6fc234cf15109e8931ffa1e60dc6de4f8a5b317840422a6bdb0ca8558f888c8bd545b30566e4e1f1b7b339e17875e54d16411a596fb5ef1b02a50359b8550a248aa25aab345754dec0a0f290&ascene=1&uin=MTMyMzkwNzQ0MQ%3D%3D&devicetype=Windows+10&version=62060739&lang=zh_CN&pass_ticket=sGx9Uuwxeper%2B1H5fZA88N2x1ErrdSdkX4lVKdIZrxZeWVcwNUXvzfdZ50Qz0ia6)
