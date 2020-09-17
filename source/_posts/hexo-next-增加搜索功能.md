---
title: hexo+next 增加搜索功能
date: 2020-09-17 10:42:32
tags:
  - blog 增加搜索功能
  - hexo搜索
---
#### 1、安装本地搜索插件 hexo-generator-search
在博客根目录安装搜索插件
```git
  # 安装插件，用于生成博客索引数据（在博客根目录下执行下列命令）：
  npm install hexo-generator-search --save
```
<!-- more -->
安装之后，会在站点目录的 `public` 文件夹下创建一个 `search.xml` 文件。（如果没有 `search.xml` 文件，请继续往下看）

#### 2、修改站点配置文件(如果上一步没有找到 `search.xml`文件 则可以跳过 )
在主题配置文件中的 `_config.yml` 中添加如下内容：
```git
  # Search
  search:
    path: ./public/search.xml
    field: post
    format: html
    limit: 10000
```
- path：索引文件的路径，相对于站点根目录
- field：搜索范围，默认是 post，还可以选择 page、all，设置成 all 表示搜索所有页面
- limit：限制搜索的条目数

#### 3、主题配置文件

在主题配置文件 `_config.yml` 中找到如下内容：
```git
  local_search:
    enable: true
    trigger: auto
    top_n_per_article: 1
```

确保 `enable` 设成 `true`。

`top_n_per_article` 字段表示在每篇文章中显示的搜索结果数量，设成 `-1` 会显示每篇文章的所有搜索结果数量。

然后，重新部署网站即可愉快的使用本地搜索功能了。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200917110556774.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZseV93dWd1aQ==,size_16,color_FFFFFF,t_70#pic_center)
以上就是在博客添加本地搜索~
