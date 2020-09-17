---
title: 记录搭建github二级域名博客遇到的问题
categories:
  - skill
tags:
  - probelm
  - blog
---


## 1.推送失败本地预览没有问题 `Deployer not found: git`
**提示`Deployer not found: git`**

原因1、没有安装插件 该插件可以将本地代码 `build` 之后提交到你指定的 `github` 仓库分支

<!-- more -->
```
  npm install hexo-deployer-git --save
```
原因2、本地的hexo主题的 `_config.yml` 文件配置有问题
检查 `deploy` 字段有没有 问题
## 2、`_config.yml` 文件配置  `deploy` 说明

  字段 `type` 提交工具 一般为 `git`
  字段 `repo` 静态文件仓库地址 `git@github.com:仓库名/博客名字.github.io.git`
  字段 `branch` 注意该字段 **该字段是指执行 `hexo d` 命令后文件最后推送到远程库的分支**

  完整例子（`youranme` 为要替换自己的数据）：
  ```
  deploy:
    type: git
    repo: git@github.com:youranme/youranme.github.io.git
    branch: master
  ```


## 3、如何解决在多台电脑上提交博客
**解决办法：利用 `github` 的分支**
github 新建分支 `newBranch` 并设置为默认分支
  (设置默认分支 当前代码库 -> setting -> Branchs -> Default branch 选择分支 -> 点击 Update)
1、首先将本地远程库克隆到本地 (此时的本地仓库默认指向的是 `newBranch`)
```git
  git clone 仓库地址
```

2、然后此时仓库有两个分支 `newBranch` 和 `master` ;
在当前克隆下来的文件夹里面执行 (需要安装 `hexo`)
```hexo
  hexo init
```
3、在新电脑上 生成 `ssh` ;
```git
    ssh-keygen -t rsa -C 'github账号'
```
`ssh` 值在 `/用户/当前账户名/.ssh/id_rsa.pub` 的文件内

4、将值新建到 `guhub` 的 `ssh keys`:
github账户 -> setting -> SSH and GPG keys -> SSH keys -> New SSH key、

5、设置本地账户信息
```git
    $ git config --global user.name "yourname"
    $ git config --global user.email youeremail@example.com
```

6、将 hexo 当前目录下的文件 全部提交到远程库 `newBranch`

7、本地预览
新建博文
```git
  hexo new 'newBlogTitle'
```

生成静态文件
```git
    hexo g  // 生成文件
```
预览静态文件
```git
    hexo s  // 预览静态文件 访问localhost:4000
```

8、将修改的文件提交的远程仓库
```git
  git add .
  git commit -m '提交备注'
  git push
```
9、发布到远程仓库
执行以下
```git
   hexo clean && hexo g && hexo d
```
此时会将博客的静态文件推送到 `master` 分支，
同时访问 https://mogugit.github.io/ 也就能看到更新的内容

10、更换电脑之后，只需要将远程仓库的分支克隆下来 在 `github` 中添加 `ssh` 就可以了
每次更新文件只需要执行 步骤 8,9 就好了

如果代码提交错误 那就回滚代码；
首先通过 `git log` 获取相应的版本号
```
  git log
```
回滚
```
  git reset --hard 版本号
```
从新推送到远程仓库
```
   git push -f -u origin 分支名称
```
重新更新 代码
```
  git pull
```
