---
title: 使用canvas制作背景水印
date: 2020-06-02 16:43:02
tags:
---
记录使用canvas 制作文字背景水印
<!-- more -->
```javascript
// 首先创建canvas标签 并设置画布大小
      var watchCanvas = document.createElement('canvas'); // 创建canvas标签
      watchCanvas.id = 'myCanvas'; // 设置canvas id名
      watchCanvas.width = '300'; // 设置画布大小
      watchCanvas.height = '120';
      watchCanvas.style.display = 'none'; // 隐藏画布
      document.documentElement.appendChild(watchCanvas); // 将画布插入到document中

      // 创建 画布内容
      var c = document.getElementById('myCanvas'); // 获取画布

      // 获取画布上下文
      var ctx = c.getContext('2d');

      // 设置字体文字大小及字体类型
      ctx.font = '20px Arial';

      // 设置旋转角度 格式 (-45 * Math.PI) / 180
      ctx.rotate((-45 * Math.PI) / 180);

      // 设置水印实心文字及偏移量 fillText(text, x, y) strokeText(text,x,y)  说明 text | 在画布上出现的值, x 在x方向上的值(相对于画布), y 在y方向上的值(相对于画布);
      ctx.fillText('Hello Word', -40, 80);
      ctx.fillText('canvas', 30, 160);

      // 生成base64格式的图片路径
      var curl = c.toDataURL('image/png');

      // 将图片作为背景样式插入
      document.querySelector('.container').style.background =
        '#bf0000 url(' + curl + ')';

      // 设置水印文字旋转偏移量时 注意是先旋转在偏移 这个时候的偏移量是以偏转之后的坐标轴为基础的
      // toDataURL(type, encoderOptions) 方法 type | 可选 默认值为'image/png', encoderOptions | 可选 在指定图片格式为 image/jpeg 或 image/webp的情况下，可以从 0 到 1 的区间内选择图片的质量。如果超出取值范围，将会使用默认值 0.92。其他参数会被忽略。
```
