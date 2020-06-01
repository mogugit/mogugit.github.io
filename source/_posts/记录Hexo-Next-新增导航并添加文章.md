---
title: 记录Hexo + Next 新增导航并添加文章
date: 2020-06-01 17:43:00
caregories:
  - skill
tags:
  - skill
  - add article
---
## 1、新建导航/菜单
执行以下命令:
```hexo
  hexo new page '导航/菜单名称'
```
执行之后 开启预览会有新建的菜单 `导航/菜单名称`

## 2、修改主题菜单/导航配置
  1. 打开 `hexo/_config.yml` 文件，找到 `language:`，这行代码。例如我的语言是 `language: zh-CN` 。
  2. 打开文件 `hexo/themes/next/language`（`next`主题，打开你当前主题里的 `language` 文件夹）。那么我就打开 `language/zh-CN.yml`，与你的语言对应的文件。
  3. 在 Menu: 下方添加一行 `导航/菜单 英文 : 导航/菜单 中文`，就修改成 `导航/菜单 中文`了。
  4. 打开新建菜单文件下的 `index.md` ，修改成 `title: 写成你想要的名字`  即可（可省略）。

## 3、实现新建导航下多篇文章（这里用到的办法借用了 categories 的分类功能，其实就是将某个分类移到了左侧菜单上）
  1. 我们在 `_post` 目录下有几篇文章想放到该菜单下，我们给这些文章分类为 `categories: - 新建导航/菜单 的英文` 。
  2. 打开 `hexo/themes/next/_config.yml` 文件，找到 `menu:`，添加一行 `新建导航/菜单 的英文: //categories/ '新建导航/菜单 的英文' / || icon的名称(可以参照 home 项更改)` 即可。
  3. 这时我们点击，该菜单，就会发现里面可以显示这几篇文章。
