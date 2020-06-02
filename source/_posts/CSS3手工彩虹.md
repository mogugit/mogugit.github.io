---
title: CSS3手工彩虹
date: 2020-06-02 15:46:33
tags:
---
[![ttrrEq.png](https://s1.ax1x.com/2020/06/02/ttrrEq.png)](https://imgchr.com/i/ttrrEq)
### html
```html
    <div class="caihong"></div>
```
<!-- more -->
### css
```css
  .caihong {
    width: 200px;
    height: 200px;
    border-radius: 50%;
    box-shadow:
      0 0 0 5px inset red,
      0 0 0 10px inset orange,
      0 0 0 15px inset yellow,
      0 0 0 20px inset lime,
      0 0 0 25px inset aqua,
      0 0 0 30px inset blue,
      0 0 0 35px inset magenta;
      clip-path: polygon(0 0, 100% 0, 100% 50%, 0 50%)

  }
```
参考地址：[clip-path属性详情https://developer.mozilla.org/zh-CN/docs/Web/CSS/clip-path#fill-rule](https://developer.mozilla.org/zh-CN/docs/Web/CSS/clip-path#fill-rule)
