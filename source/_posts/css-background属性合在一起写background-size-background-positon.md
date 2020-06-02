---
title: css background属性合在一起写background-size background-positon
date: 2020-06-02 11:45:23
tags:
  - css
  - background
---
```css
background: no-repeat scroll 56px 78px / 69px 69px rgba(0, 0, 0, 0) url('.....');

background: no-repeat scroll `56px 78px（background-position）/（分割线） *69px 69px *（background-size ）` rgba(0, 0, 0, 0);
```
