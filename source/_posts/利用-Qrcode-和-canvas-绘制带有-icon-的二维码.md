---
title: 利用 Qrcode 和 canvas 绘制带有 icon 的二维码
date: 2020-06-17 16:44:58
tags:
  - canvas
  - qrcode
---
#### canvas生成二维码附带icon并实现下载
参考地址 https://blog.csdn.net/chy555chy/article/details/85785819

利用 [qrcode](https://github.com/soldair/node-qrcode) 生成二维码是不带 `icon` 的，去网上[查找解决方案](https://blog.csdn.net/chy555chy/article/details/85785819)需要修改源码。
所以只好自己开搞了
效果如下：
![NEz2n0.png](https://s1.ax1x.com/2020/06/17/NEz2n0.png)
代码如下
<!-- more -->
```js
<template>
  <div>
    <!-- 二维码和icon -->
    <img :src="rediusIconSrc" alt="ICON">
    <img :src="qrCodeSrc" alt="qrcode">
    <img :src="iconCodeSrc" alt="IconAndQrcode">
    <button @click="getQrcode">下载</button>
  </div>
</template>
<script>
import QRCode from 'qrcode'; // 需安装 qrcode 插件;
export default {
  data() {
    return {
      qrCodeSrc: '',
      rediusIconSrc: '',
      iconCodeSrc: '',
      qrCodeW: '200',
      qrCodeH: '200',
      codeContext: 'text123',
      iconScale: 0.2 // icon 占二维码的比例;
    };
  },
  mounted() {
    this.getCanvas(this.codeContext);
  },
  methods: {
    getQrcode() {
      let _a = document.createElement('a');
      _a.href = this.iconCodeSrc;
      _a.download = '带有 icon 的二维码';
      _a.click();
    },
    getCanvas(codeContext, option = {errorCorrectionLevel: 'H', margin: 1}) {
        // 配置参数 参考https://github.com/soldair/node-qrcode#options
        // var opts = {
        //   errorCorrectionLevel: 'H', // 容错级别 low, medium, quartile, high or L, M, Q, H.
        //   type: 'image/jpeg', // 图片类型
        //   quality: 0.9, // 透明度 貌似没啥效果
        //   maskPattern: 1, // 遮罩图案
        //   margin: 1, // 二维码距离边框的距离 单位为4px 默认值1个单位
        //   color: { // 二维码 码块的颜色
        //     dark: '#010599FF', // 默认值 #000000
        //     light: '#FFBF60FF' // 默认值 #ffffff
        //   }
        // };

        QRCode.toDataURL(codeContext, option, (err, url) => {
          if (err) throw err;
          // 生成二维码图片地址
          this.qrCodeSrc = url;

          // 二维码 转化为 canvas 并将 icon 画入到 canvas 中，然后导出;
          this.addicon();
        });
    },
    // 生成带边框及圆角的 icon 图片;
    addicon(X = 0, Y = 0, W, H, radius = 5, birderW = 2, borderColor = '#fff', isbgColor = true, bgColor = 'rgba(255,255,255, 1)') {
      let c = document.createElement('canvas');
        c.width = this.qrCodeW * this.iconScale;
        c.height = this.qrCodeH * this.iconScale;
        let _w = W || c.width, _h = H || c.height;
        let ctx = c.getContext('2d');

        // 是否设置背景颜色
        if (isbgColor) {
          ctx.fillStyle = bgColor;
          ctx.fillRect(0, 0, c.width, c.height);
        }
        this.drawRoundRect(ctx, X, Y, _w, _h, radius, birderW, borderColor);
        let img = document.createElement('img');

        // 网络图片直接给地址;
        img.src = 'https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=2069790559,642682254&fm=26&gp=0.jpg';

        // 如果是本地图片请使用 require() 引入;
        // img.src = require(src); // src 不能使变量 会查找不到;

        // 解决 访问网络图片属于跨域操作 Failed to execute 'toDataURL' on 'HTMLCanvasElement': Tainted canvases may not be exported.
        img.setAttribute('crossOrigin', 'Anonymous');
        img.onload = () => {
          ctx.clip();
          ctx.drawImage(img, 0, 0, 40, 40);
          ctx.restore();
          this.rediusIconSrc = c.toDataURL();
          this.mergeImg();
        };
    },
    // 圆角 icon 和 二维码 合并到一起;
    mergeImg() {
      let c = document.createElement('canvas');
        c.width = this.qrCodeW;
        c.height = this.qrCodeH;
        let ctx = c.getContext('2d');
        let bg = document.createElement('img');
        let icon = document.createElement('img');
        bg.src = this.qrCodeSrc;
        icon.src = this.rediusIconSrc;

        let _bgLoad = new Promise((resolve, reject) => {
          try {
            bg.onload = () => resolve();
          } catch (error) {
            reject(error);
          }
        });
        let _iconLoad = new Promise((resolve, reject) => {
          try {
            icon.onload = () => resolve();
          } catch (error) {
            reject(error);
          }
        });

        Promise.all([_bgLoad, _iconLoad]).then(res => {
          let w = this.qrCodeW;
          let h = this.qrCodeH;
          let iconW = w * this.iconScale;
          let iconH = h * this.iconScale;
          let iconX = (w - iconW) / 2;
          let iconY = (h - iconH) / 2;
          // 画二维码
          ctx.drawImage(bg, 0, 0, w, h);
          // 画icon
          ctx.drawImage(icon, iconX, iconY, iconW, iconH);
          // 转化为图片 src;
          this.iconCodeSrc = c.toDataURL();
        }).catch(e => {
          console.log('error-', e);
        });
    },
    // 画圆角 ctx 上下文 x,y 起始位置 width,height icon的宽高 radius 圆角的角度 lineWidth 边框的宽度 lineColor 边框的颜色
    drawRoundRect(ctx, x, y, width, height, radius, lineWidth, lineColor) {
      ctx.lineWidth = lineWidth;
      ctx.strokeStyle = lineColor;
      ctx.beginPath();
      ctx.arc(x + radius, y + radius, radius, Math.PI, Math.PI * 3 / 2);
      ctx.lineTo(width - radius + x, y);
      ctx.arc(width - radius + x, radius + y, radius, Math.PI * 3 / 2, Math.PI * 2);
      ctx.lineTo(width + x, height + y - radius);
      ctx.arc(width - radius + x, height - radius + y, radius, 0, Math.PI * 1 / 2);
      ctx.lineTo(radius + x, height + y);
      ctx.arc(radius + x, height - radius + y, radius, Math.PI * 1 / 2, Math.PI);
      ctx.closePath();
      ctx.stroke();
    }
  }
};
</script>

```

