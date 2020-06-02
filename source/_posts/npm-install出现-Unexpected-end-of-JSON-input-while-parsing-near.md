---
title: 'npm install出现: Unexpected end of JSON input while parsing near'
date: 2020-06-02 17:14:54
tags:
---
安装node版本管理工具的时候 把node卸载了 然后重新安装了node版本 然后就出现了Unexpected end of JSON input while parsing near  网上各种查，，，，终于解决了
第一种情况是 最新版的Nodejs与npm版本不合适的问题（因为没更新Node之前是不会的）。
解决办法就是 把npm的版本降到4版。
在windows下使用cmd执行语句：**npm -g i npm@4**  或者安装6.x 版本；
<!-- more -->
Mac系统的就按照相同的思路修改就行啦，btw
[具体原因看这里](https://blog.csdn.net/weixin_41902031/article/details/80041000)

第二种情况是 因为npm存在缓存导致 无法安装
在cmd下（windows）执行：**npm cache clean --force**

还有一种情况是我个人遇到的
	在nrm下使用taobao的npm链接也会报上面的错误
	**解决办法就是:使用npm 而不使用淘宝的镜像**


