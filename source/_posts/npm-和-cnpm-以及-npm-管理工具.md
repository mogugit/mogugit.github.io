---
title: npm 和 cnpm 以及 npm 管理工具
date: 2020-06-02 14:06:28
tags:
---

### 关于 npm 源设置
#### 1、临时使用某个安装源地址：
如果只想某一次使用某个安装源命令如下：
```bash
	// 使用源镜像
	npm --registry https://registry.npm.org install
	// 使用淘宝镜像
	npm --registry https://registry.npm.taobao.org install
```
<!-- more -->
上面两个命令 **install** 后面跟安装包名则安装指定包；不指定则按照**package.json**安装

#### 2、持久使用源地址：
如果想一直使用某一个源地址命令如下：
```bash
	npm config set registry https://registry.npm.taobao.org
```
配置后可通过下面方式来验证是否成功
```bash
	// 如果成功则返回的是 上一步设置好的源地址；
	// npm config get registry 返回的是当前源地址;
	npm config get registry
	// npm info <name> 返回的是当前安装包的信息;
	npm info <安装包name>
```
然后就可以像往常正常使用**npm命令**

#### 3、通过使用阿里的镜像cnpm使用
使用阿里的镜像 cnpm 的执行命令：
```bash
	npm install -g cnpm --registry=https://registry.npm.taobao.org
```
只需要执行上面的命令就可以像使用 npm 一样使用 cnmp了,**唯一不同就是所有npm命令都换成cnpm**；
例：
```bash
	cnpm install
```
#### 4、通过使用 nrm 来管理 npm 源地址：
4-1、安装
安装 nrm 命令：
```bash
	npm install -g nrm
```
安装成功之后使用 **nvm -V** 查看是否安装成功，
如遇报错**1[请查看解决办法](https://mogugit.github.io/2020/06/02/nrm-%E6%97%A0%E6%B3%95%E5%8A%A0%E8%BD%BD%E6%96%87%E4%BB%B6-C-Program-Files-nodejs-nrm-ps1%EF%BC%8C%E5%9B%A0%E4%B8%BA%E5%9C%A8%E6%AD%A4%E7%B3%BB%E7%BB%9F%E4%B8%8A%E7%A6%81%E6%AD%A2%E8%BF%90%E8%A1%8C%E8%84%9A%E6%9C%AC%E3%80%82/#more)**

4-2、查看当前源列表
执行命令
```bash
	nrm ls
```
展示如下 可以看到列表中左侧为名称，右侧为地址。带*的为当前配置：
[![ttewgH.png](https://s1.ax1x.com/2020/06/02/ttewgH.png)](https://imgchr.com/i/ttewgH)
更换命令 nrm use < registry> registry:代表源的名；
例：
```bash
	nrm use npm/yarn/cnpm/taobao
```
使用npm的 npm config list 命令查看查看当前源配置。
```bash
	npm config list
```
[![tteN4O.png](https://s1.ax1x.com/2020/06/02/tteN4O.png)](https://imgchr.com/i/tteN4O)
nrm还提供了测速功能，命令为 **nrm test [registry]** ，不知道选哪个源时，可以先测一波，哪个快用哪个。不加registry时，可测所有的
```bash
	nrm test
```
结果如下
[![tted8e.png](https://s1.ax1x.com/2020/06/02/tted8e.png)](https://imgchr.com/i/tted8e)
#### 命令提示：
1. **nrm -V** ：查看当前nvm版本。
2. **nrm -h** ：显示所有命令。
3. **nrm current** ：显示当前源名称。
4. **nrm use < registry>** ：切换源。
5.  **nrm add < registry> < url> [home]** ：添加一个源。比如公司自己的私有源等。
6.  **nrm set-auth < registry> < value> [always]** ：设置自定义源的授权信息。
7.  **nrm set-email < registry> < value>** ：给自定义源设置路径。
8.  **nrm set-hosted-repo < registry> < value>** ：设置发布到自定义源的npm托管仓储。
9.   **nrm del < registry>** ：删除自定义源。
10. **nrm home < registry> [browser]** ：浏览器中打开源首页。
11.  **nrm publish [options] [< tarball> | < folder>]** ：发布包到自定义源，如果没有使用自定义源，则直接发布到npm。
12.  **nrm test [registry]** ：测试源的访问速度。不加registry时，测试所有的。


