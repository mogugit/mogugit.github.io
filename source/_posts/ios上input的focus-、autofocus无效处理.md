---
title: ios上input的focus()、autofocus无效处理
date: 2020-06-02 15:56:03
tags:
---
## 出现focus无效原因：
> **ios的UIWebView 默认的KeyboardDisplayRequiresUserAction为false，设置为true就行，WKWebView 不支持这个属性，如果要从原生入手解决，请参考https://stackoverflow.com/questions/32407185/wkwebview-cant-open-keyboard-for-input-field**
## 解决思路：
> **从无效原因可以看出，是键盘需要用户触发才能弹出，这导致了autofocus或者element.focus()无效，所以，在键盘弹出的情况下再去focus，或者跳转到带有autofocus的页面也就可以正常focus了**
<!-- more -->
## 解决方法：
> **通常的场景是，我们点击页面某个元素 => 逻辑交互 => 希望focus元素、或者跳转到有aotufocus的页面。再这里有个大前提，就是要有点击页面行为。**

## 划重点
>**只要是点击事件的回调就具备focus到input的能力，所以无论是点击生成input再focus到这个input、还是跳转到autofocus的页面，先利用点击focus到一个占位input调起键盘，在键盘存在的情况下调用element.focus()或者跳转到有autofocus的页面就都可以正常focus了。**
```javascript
.clip{
  position: absolute;
  clip: rect(0 0 0 0);
}

<input ref="tempFocus" class="clip">

<div @click="gotoCommentClick">快来留言吧！</div>

gotoCommentClick() {
  this.afterLogin().then(_ => {
    this.$refs.tempFocus.focus();
    this.$router.push(this.$route.path + '/comment');
  })
},
mounted() {
    this.$refs.editor.focus();
  }
```

### 注意事项 就是调起的键盘如果收回还是会出现focus(),autofocus无效；
参考文章  [ios上input的focus()、autofocus无效处理方法](https://www.jianshu.com/p/ea0b447c781e)
