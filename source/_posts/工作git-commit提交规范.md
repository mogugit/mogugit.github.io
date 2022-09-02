---
title: 工作git commit提交规范
date: 2022-09-02 14:29:52
tags: 
    - git commit
    - 提交规范
---
#### git 提交规范
**规范的优势**
 1. 版本回退可以快速定位到指定版本
 2. 清晰明了知道每次提交的内容
 3. 统一规范
 
**commit 提交格式**
  commit 提交包括三个部分 `Header`, `Body`和 `Footer`;
  其中 `Header` 是必须的, `Body`和`Footer`是可忽略。 
范例如下：

<!-- more -->
```ts
    <type>（scope）: <subject>
    // 空行必须
    <body>
    // 空行必须
    <footer> 
```
**Header**
Header部分只有一行，包括`type`（必需）,`scope`(可忽略)和 `subject`(必需)；
 `type` 包括一下9个类型
```text
    feat: 新功能（feature）；
    fix: 修补BUG；
    docs: 文档（documentation）；
    style: 样式修改（不影响代码运行的样式修改）；
    refactor: 已有功能重构（既不是新增，也不是修改BUG）；
    chore: 构建过程，辅助工具变更；
    revert: 撤销，版本回退；
    perf: 性能优化；
    test: 测试相关；
    improvement: 改进；
    build: 打包相关，比如buil工具变更；
    cli: 持续集成；
```
`scope` 影响范围，比如：数据层，控制层，试视图层：
```text
    all: 表示影响面大，比如修改了网络请求框架，会影响整个程序；
    module: 表示影响某个模块，跟某些模块相关，比如登录，用户管理等；
    location: 表示影响较小，只有某个小功能影响；
```
`subject` 简述本次提交的改动。

**Body**
`Body`是对本次提交的详细描述，可以分为很多行。例如：
```text
    xx/xxx.xx 修改内容
    xx/xxx.xx 修改内容
```

**Footer**
Footer 适用于 不兼容变更，关闭需求和Bug；
- 关闭需求
    ```text
        close bugID, 需求ID,...
    ```
- 不兼容变动 如果当前代码与上一个版本不兼容，则 Footer 部分以BREAKING CHANGE开头，后面是对变动的描述、以及变动理由和迁移方法。
    ```text
        BREAKING CHANGE: 项目webpack升级到4.0版本
        xx插件替换成xx插件  
    ```

使用范例：
```text
    revert: feat(xx模块): 回退当前版本667ec到 sssee2

    因为某次提交失误，造成xxx问题
    xxx.vue
    xxx.js

    closes xxx
```
范例2:
```text
    chore(项目组件, 项目构建): 增加公用组件库xxx和xxx公用一套组件

    为了解决xx问题， 引入公用组件库，使用方式见xx.md文档
```