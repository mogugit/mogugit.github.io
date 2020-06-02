---
title: Mac下安装nvm及常见问题
date: 2020-06-02 17:11:02
tags:
---
转 [Mac下安装nvm及常见问题](https://www.jianshu.com/p/04d31f6c22bd)
前言：(这一步是可选，如果是windows 用户最好是删除已安装的node和npm)
1.卸载已安装到全局的 node/npm
    如果之前是在官网下载的 node 安装包，运行后会自动安装在全局目录，其中
```
node 命令在 /usr/local/bin/node ，npm 命令在全局 node_modules 目录中，具体路径为 /usr/local/lib/node_modules/npm
```

安装 nvm 之后最好先删除下已安装的 node 和全局 node 模块：

<!-- more -->

**一、安装**
	1、curl 安装
	```
		curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
	```
	或者 wget
	```
		wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
	```
	具体版本查看地址
		https://github.com/creationix/nvm/blob/master/README.md
安装完成后请重新打开终端环境
2、查看安装
	安装完 nvm 后，输入nvm，当看到有输出时，则 nvm 安装成功。
	```
		nvm: command not found
	```
	编辑.bash_profile文件，没有的话就新建一个，
>1. 启动终端Terminal
>2. 进入当前用户的home目录
>输入cd ~
>3. 创建.bash_profile
>输入touch .bash_profile
>4. 编辑.bash_profile文件
>输入open .bash_profile
>第一种方式
><1>、为在弹出的.bash_profile文件内进行编辑
><2>、编辑完成后直接保存文件
><3>、关闭.bash_profile文件
><4>、更新配置过的环境变量    输入source .bash_profile
><5>、 启动终端Terminal
>		第二种方式来编辑.bash_profile文件
>在Terminal终端通过指令来对.bash_profile文件进行编辑
><1>、输入 vim .bash_profile
><2>、输入 i 进行编辑模式
><3>、然后把需要编辑的内容键入，编辑完之后直接按esc退出编辑模式，
><4>、输入:w进行文件的保存，:wq为保存并退出指令


**二、 使用**

```
nvm install stable # 安装最新稳定版 node，现在是 5.0.0
nvm install 4.2.2 # 安装 4.2.2 版本
nvm install 0.12.7 # 安装 0.12.7 版本

# 特别说明：以下模块安装仅供演示说明，并非必须安装模块
nvm use 4 # 切换至 4.2.2 版本
npm install -g mz-fis # 安装 mz-fis 模块至全局目录，安装完成的路径是 /Users/<你的用户名>/.nvm/versions/node/v0.12.7/lib/mz-fis
nvm use 0 # 切换至 0.12.7 版本
npm install -g react-native-cli #安装 react-native-cli 模块至全局目录，安装完成的路径是 /Users/<你的用户名>/.nvm/versions/node/v4.2.2/lib/react-native-cli

nvm alias default 0.12.7 #设置默认 node 版本为 0.12.7

```
查看nvm版本：打开新的终端，用nvm current查看当前版本显示

删除指定版本 node：nvm uninstall v6.6.0

nvm 提供了 nvm use 命令。这个命令的使用方法和 install 命令类似。
```
例如，切换到 4.2.2：
nvm use 4.2.2
切换到最新的 `4.2.x``：
nvm use 4.2
切换到最新版：
nvm use node
```
我们还可以用 nvm 给不同的版本号设置别名：

```
我们给 4.2.2 这个版本号起了一个名字叫做 awesome-version
nvm alias awesome-version 4.2.2

然后我们可以运行：
nvm use awesome-version

下面这个命令可以取消别名：
nvm unalias awesome-version

另外，你还可以设置 default 这个特殊别名：
nvm alias default node
```
列出已安装实例
```
nvm ls
```
