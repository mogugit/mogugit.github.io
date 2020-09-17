---
title: >-
  Python出现Could not find a version that satisfies the requirement openpyxl (from
  versions: )
date: 2020-09-14 16:21:08
tags:
  - python
  - python包安装源
---
##### 一.环境
使用python3.7时，用pip安装openpyxl出现如下错误：
<!-- more -->
[![wD5qh9.png](https://s1.ax1x.com/2020/09/14/wD5qh9.png)](https://imgchr.com/i/wD5qh9)
#### 二. 解决方案
可能是python国内网络的问题，这时我们用国内的镜像源来加速。
```python
pip install 包名 -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com
# 这个是豆瓣源
# --trusted-host pypi.douban.com 这是为了获得ssl证书的认证，要不然会报错
```
[![wD5OpR.png](https://s1.ax1x.com/2020/09/14/wD5OpR.png)](https://imgchr.com/i/wD5OpR)
#### 三. pip和pip3的区别
其实这两个命令效果是一样的，没有区别：

比如安装库openpyxl，pip3 install openpyxl或者pip install openpyxl：只是当一台电脑同时有多个版本的Python的时候，用pip3就可以自动区分用Python3来安装库。是为了避免和Python2发生冲突的。
（2）如果你的电脑只安装了Python3，那么不管用pip还是pip3都一样的。
安装了python3之后，会有pip3
（1）使用pip install XXX ：
新安装的库会放在这个目录下面：python2.7/site-packages
（2）使用pip3 install XXX ：
新安装的库会放在这个目录下面：python3.7/site-packages
（3）如果使用python3执行程序，那么就不能importpython2.7/site-packages中的库。
