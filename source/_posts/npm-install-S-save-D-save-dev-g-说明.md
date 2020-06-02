---
title: npm install -S | --save | -D | --save-dev | -g 说明
date: 2020-06-02 15:53:48
tags:
---
npm 安装包命令
#### 1、局部安装
```
npm install <package_name>
```
说明 安装到当前项目
>npm 5x 以后 这个命令等同于npm install --save <package_name> 同时也是会同样写入到依赖 dependencies

#### 2、 全局安装 -g
<!-- more -->
```
npm install -g <package_name>
```
安装到全局并不会体现到package.json 里面

#### 3、安装到生产依赖 --save
```
npm install  <package_name>  --save | -S
```
安装到当前项目，并将包信息写入到dependencies

#### 4、安装到开发依赖 --save-dev
```
npm install  <package_name>  --save-dev | -D
```
安装到当前项目 并写入到devDependencies

##### devDependencies与dependencies 的区别:

>devDependencies 是本地开发时用的依赖项
>dependencies 是生产环境的依赖项

##### 安装依赖包
```
npm install
```
会将package.json 里面的devDependencies和dependencies下的所有包都会下载到项目的node_modules文件夹下(没有的改文件夹会新建一个)
##### 只安装生产依赖
```
npm install --production
```

