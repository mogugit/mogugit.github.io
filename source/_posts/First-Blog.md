---
title: First Blog
date: 2020-05-27 17:44:06
tags: blog,learn
---

利用 `github` 的二级域名搭建个人博客（默认已经拥有了`node`, `git`, `github`）等基础要求；

### 1、github 构建新仓库

构建新仓库注意 构建的这个博客仓库具有唯一性 所以名称格式为 username.github.io
<!-- more -->
### 2、生成本地 ssh

```git
    ssh-keygen -t rsa -C 'github账号'
```
生成的 ssh 信息 默认为 用户/当前账户/.ssh/id_rsa.pub 文件内 可以用记事本打开

MAC 查看 SSH 信息命令如下：
```bash
    cat ~/.ssh/id_rsa.pub
```
返回以 `ssh-rsa` 开头的 ssh 秘钥

### 3、生成 ssh 保存到 github

右上角个人信息 -> settings -> SSH and GPG keys -> new SSH key 填入上一步生成的 ssh 信息

测试 ssh  和本地是否连通
```git
$ ssh -T git@github.com
```
回车 输入`yes`
果看到Hi后面是你的用户名，就说明成功了


### 4、本地安装 hexo 主题

```npm
    npm install hexo -g
```
修改配置信息 打开 `_congfig.yml` 文件 修改 `deploy`
```
    deploy:
        type: git
        repo: git@github.com:username/username.github.io.git
        branch: master
```

### 5、设置本地账户信息

```git
    $ git config --global user.name "yourname"
    $ git config --global user.email youeremail@example.com
```

### 6、执行命令
生成静态文件
```git
    hexo g  // 生成文件
```
预览静态文件
```git
    hexo s  // 预览静态文件 访问localhost:4000
```
发布到远程仓库
```git
    hexo d
```
这一步默认会报错 提示没有git 错误： `Deployer not found: git`
**解决办法：**
```npm
    npm install hexo-deployer-git --save
```
然后再次执行
```
    hexo d
```
即可发布到远程仓库
最后可以通过 `yourname.github.io` 访问博客


