---
title: "nrm : 无法加载文件 C:\Program Files\nodejs\nrm.ps1，因为在此系统上禁止运行脚本。"
date: 2020-06-02 14:17:51
tags:
---
Win10系统 安装 nrm 出现报错：
nrm : 无法加载文件 C:\Program Files\nodejs\nrm.ps1，因为在此系统上禁止运行脚本。有关详细信息，请参阅 https:/go.microsoft.com/fwlink/?LinkID=135170 中的 about_Execution_Policies。
所在位置 行:1 字符: 1
<!-- more -->
[![ttmYss.png](https://s1.ax1x.com/2020/06/02/ttmYss.png)](https://imgchr.com/i/ttmYss)
**解决办法：**
1、win键 + s 搜索 powershell 并一管理员身份运行：
[![ttmtLn.png](https://s1.ax1x.com/2020/06/02/ttmtLn.png)](https://imgchr.com/i/ttmtLn)
2、执行以下命令：**set-ExecutionPolicy RemoteSigned** 回车；
[![ttmJMj.png](https://s1.ax1x.com/2020/06/02/ttmJMj.png)](https://imgchr.com/i/ttmJMj)
并按 **Y** 执行；
3、重新安装即可；
