---
title: vue2.0 组件复用 数据没有更新
date: 2020-06-02 16:23:24
tags:
---
如果开发过程中出现使用**v-if-else**来切换组件时发现数据没有更新，那么就是因为元素被复用具体可以[参考vue官网-组件]( https://cn.vuejs.org/v2/guide/conditional.html#%E7%94%A8-key-%E7%AE%A1%E7%90%86%E5%8F%AF%E5%A4%8D%E7%94%A8%E7%9A%84%E5%85%83%E7%B4%A0)
```javascript
<template v-if="loginType === 'username'">
  <input placeholder="Enter your username">
</template>
<template v-else>
  <input placeholder="Enter your email address">
</template>
```
<!-- more -->
查看input元素发现只有input的placeholder的变化了 元素本身就没有改变，而且输入的值也没有被清除；原因是vue为了尽快的渲染页面所以通常会复用已经渲染的元素，所以会导致绑定的数据并没被实时刷新；
解决办法就是：对复用的元素添加 **key**

```javascript
<template v-if="loginType === 'username'">
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <input placeholder="Enter your email address" key="email-input">
</template>
```
这样就避免了数据没有刷新的问题
