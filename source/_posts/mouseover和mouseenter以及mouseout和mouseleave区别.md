---
title: mouseover和mouseenter以及mouseout和mouseleave区别
date: 2020-06-02 13:20:10
tags:
---
#### 1、mouseover与mouseenter

**共同点**：
当二者都没有子元素时,二者的行为是一致的,但是二者内部都包含子元素时,行为就不同了.

<!-- more -->
**不同点：**
mouseover事件：不论鼠标指针进入被选元素或其子元素，都会触发其父级的 	mouseover 事件。mouseenter事件：只有在鼠标指针进入被选元素时，才会触发 mouseenter 事件。
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
    #box1 {
      width:200px;
      height: 200px;
      background-color: aquamarine;
    }
    #box2 {
      width:200px;
      height: 200px;
      background-color: brown;
    }
    #ch1 {
      width:150px;
      height: 100px;
      background-color:cornsilk;
      margin: 0 auto;
    }
    #ch2 {
      width:150px;
      height: 100px;
      background-color:darkgrey;
      margin: 0 auto;
    }
  </style>
</head>
<body>
    <div id="box1">
      <div id="ch1"></div>
    </div>
    <div id="box2">
      <div id="ch2"></div>
    </div>
    <script>
      var box1 = document.getElementById('box1');
      var box2 = document.getElementById('box2');
      var ch1 = document.getElementById('ch1');
      var ch2 = document.getElementById('ch2');
      var a = 1;
      var b = 1;
      var c = 1;
      var d = 1;

      function fnb1(){
        ch1.innerText = '父节点mouseover次数' + ++a + "\n" +'子节点mouseover次数'+ c;
      }

      function fnb2(){
        ch2.innerText = '父节点mouseenter次数' + ++b + "\n" +'子节点mouseenter次数'+ d;
      }

      function fnc1(){
        ch1.innerText = '父节点mouseover次数' + a + "\n" +'子节点mouseover次数'+ ++c;
      }

      function fnc2(){
        ch2.innerText = '父节点mouseenter次数' + b + "\n" +'子节点mouseenter次数'+ ++d;
      }

      box1.addEventListener('mouseover', fnb1,false)
      box2.addEventListener('mouseenter', fnb2,false)

      ch1.addEventListener('mouseover', fnc1,false)
      ch2.addEventListener('mouseenter', fnc2,false)
    </script>
</body>
</html>
```
页面展示如下：
[![ttSO8P.png](https://s1.ax1x.com/2020/06/02/ttSO8P.png)](https://imgchr.com/i/ttSO8P)
#### 2、mouseout和mouseleave
**共同点：**
当二者都没有子元素时,二者的行为是一致的,但是二者内部都包含子元素时,行为就不同了.
**不同点：**
mouseout事件：不论鼠标指针离开被选元素还是任何子元素，都会触发 mouseout 事件。
mouseleave事件：只有在鼠标指针离开被选元素时，才会触发 mouseleave 事件。

效果和上面一样这里就不演示代码了 。
