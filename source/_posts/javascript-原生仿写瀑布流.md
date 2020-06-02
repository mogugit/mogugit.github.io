---
title: javascript 原生仿写瀑布流
date: 2020-06-02 17:47:35
tags:
---
瀑布流的效果 原理是初始列数top值为0，然后将第一行的高度添加到一个新的数组里，从第二行开始根据储存高度这个数组来，确定最低高度列，然后向最低高度列添加展示数据；
效果图：
<!-- more -->
[![ttjBU1.png](https://s1.ax1x.com/2020/06/02/ttjBU1.png)](https://imgchr.com/i/ttjBU1)
[![ttjD4x.png](https://s1.ax1x.com/2020/06/02/ttjD4x.png)](https://imgchr.com/i/ttjD4x)
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <title>Document</title>
<style>
    * {
        margin: 0;
        padding: 0;
        position: relative;
    }

    img {
        width: 220px;
        display: block;
    }

    .item {
        box-shadow: 2px 2px 2px #999;
        position: absolute;
    }
    #box {
        width: 816px;
        position: relative;
    }
    #box div {
        position:absolute;
        width: 198px;
        border: 1px solid #ddd;
        margin-left: 4px;
    }

</style>

</head>
<body>
    <div id="box">
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
    </div>
</body>
<script type="text/javascript">
    /**
     * [ 用来生成瀑布流，不足之处 没有做dom优化在某种极端的情况下，会增加浏览器负担; 待优化 由于是使用定位所以父级是没有高度的]
     * @return {[type]} [description]
     *
     */
    window.onload= function(){
        var box = document.getElementById('box');
        var items = box.getElementsByTagName('div');
        var url = ''; // 请求地址;
        var itemCol = 4; // 默认列的间距
        var itemColGap = 2; // 默认div 上下间的距离；
        var colNum = 5; // 默认列数;
        var itemWidth = items[0].offsetWidth; // 获取单个列的宽度；

        // 此处数据只是测试填充；
        var randomData = []; // 填充数据数组 测试;
        // 产生随机数
        function randomHeight(min,max){
            var min = min;
            var max = max;
            var randomVal = Math.ceil(Math.random()*max+min);
            return randomVal;
        }

        // 随机数添加到数组并去重；
        function forData(){
            if( randomData.length<20 ){
                var randVal = randomHeight(20,100);
                if(randomData.indexOf(randVal) < 0 ){
                    randomData.push(randVal);
                }
                forData();
            }else {
                return;
            }
        }
        forData(); // 获取随机好的去重数组
        // 此处数据只是测试填充；--END

        // 初始化
        pubuFn();

        function pubuFn(){
            var HeightData = []; // 各个列的初始高度;

            for( var i = 0; i < items.length; i++ ){
                items[i].style.height = randomData[i]+'px';
                if( i < colNum){
                    var getHeightVal = items[i].offsetHeight; // 获取起始高度；
                    items[i].style.left = i*(itemWidth+itemCol)+'px';
                    items[i].style.top = 0;
                    HeightData.push(getHeightVal);
                } else {
                    // 获取最低高度值;
                    var minVal = HeightData[0];
                    var z = 0;
                    for( let y=0;y<HeightData.length;y++ ){
                        if( minVal>HeightData[y] ){
                            minVal=HeightData[y];
                            z = y;
                        }
                    }

                    items[i].style.top = (HeightData[z]+itemColGap)+'px';
                    items[i].style.left = (items[z].offsetLeft-itemCol)+'px';
                    HeightData[z] = items[i].offsetHeight+HeightData[z]+itemColGap; // 每添加一个div 则重新计算初始列高度最低高度;
                }
            }
        }

        // 后续加载数据;
        window.onscroll = function(event){
            var crollTop =  document.documentElement.scrollTop || window.pageYOffset;
            var crollHeihgt = window.innerHeight || document.documentElement.clientHeight || document.body.clientHeight;
            var topSum = crollTop + crollHeihgt;
            var offsetTopVal = items[items.length-1].offsetTop;

            if( topSum > offsetTopVal){ // 如果滚动条的Top值与视口高度之和 大于 最后一张图片的offsetTop值 那么说明已经到最后一张图片了；
                // 此处可动态获取数据
                // todo。。。
                // 循环添加数据
                for( var c = 0; c<30;c++ ){
                    var cHei = randomHeight(0,20);
                    if( !randomData[cHei] ) continue;
                    var div = document.createElement('div');
                    div.style.height= randomData[cHei]+'px';
                    // 此处可添加需要在div 内添加的内容
                    // div.innerHTML = '<img src="' + datas[i] + '" alt="">';
                    div.innerHTML = '<p>'+c+'</p><span>'+c+'</span>';
                    box.appendChild(div);
                }
            }
            pubuFn() // 每次超过阈值调用;
        }

    }
</script>
</html>
```
