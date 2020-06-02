---
title: iOS 下键盘唤出后，fixed 元素失效
date: 2020-06-02 17:21:28
tags:
---
遮罩一般要充满全屏，做好的办法就是设置容器position: fixed;
但是如果遮罩上面的弹框需要输入弹出键盘的话，这时fixed就失效了，比如这种情况
<!-- more -->
![ttbRzQ.png](https://s1.ax1x.com/2020/06/02/ttbRzQ.png)

**解决思路**
iOS 下由于软键盘唤出后，页面 fixed 元素会失效，导致跟随页面一起滚动，那么假如页面不会过长出现滚动，那么即便 fixed 元素失效，也无法跟随页面滚动，也就不会出现上面的问题了。

那么按照这个思路，如果使 fixed 元素的父级不出现滚动，而将原 body 滚动的区域域移到 main 内部，而 header 和 footer 的样式不变，代码如下：
```
<body class="layout-scroll-fixed">
    <!-- fixed定位的头部 -->
    <div class="header">

    </div>

    <!-- 可以滚动的区域 -->
    <div class="main">
        <div class="content">
        <!-- 内容在这里... -->
        </div>
    </div>

    <!-- fixed定位的底部 -->
    <div class="footer">
        <input type="text" placeholder="Footer..."/>
        <button class="submit">提交</button>
    </div>
</body>
<style>
.header {
    position: fixed;//或者absolute
    height: 40px;
    left: 0;
    right: 0;
    top: 0;
}
.footer {
    position: fixed;//或者写成absolute
    height: 30px;
    left: 0;
    right: 0;
    bottom: 0;
}
.main {
/* main绝对定位，进行内部滚动 */
position: absolute;
top: 40px;
bottom: 30px;
/* 使之可以滚动 */
 overflow-y: scroll;
  /* 增加该属性，可以增加弹性，是滑动更加顺畅 */
  -webkit-overflow-scrolling: touch;
}

.main .content {
    height: 2000px;
}
</style>
```
