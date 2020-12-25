---
title: 使用＜details＞标签在网页里面添加脚注
date: 2020-12-25 10:43:00
tags:
  - html
  - 网页注脚
---

使用 `details` 标签可以为文章添加相应注解
代码：
<!-- more -->
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    details, summary {
      display: inline;
      vertical-align: super;
      font-size: 0.75em;
    }
    summary {
      cursor: pointer;
    }
    details[open] {
      display: contents;
    }
    details[open]::before {
      content: " [";
    }
    details[open]::after {
      content: "]";
    }
  </style>
</head>
<body>
  The most cited work in history, for example, is a 1951 paper
   <details>
      <summary>1</summary>
      Lowry, O. H., Rosebrough, N. J., Farr, A. L. & Randall, R. J. J. Biol. Chem. 193, 265–275 (1951).
   </details>
describing an assay to determine the amount of protein in a solution.
</body>
</html>
```
效果如下：
[![rRBzkt.gif](https://s3.ax1x.com/2020/12/25/rRBzkt.gif)](https://imgchr.com/i/rRBzkt)
