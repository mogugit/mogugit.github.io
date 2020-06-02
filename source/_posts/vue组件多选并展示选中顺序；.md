---
title: vue组件多选并展示选中顺序
date: 2020-06-02 17:18:39
tags:
---
在公司某些开发场景中需要多选并显示优先级 现记录一下；
基本思路： 根据Vue.js 的数据绑定，当事件发生时会触发数据的变化来更新数据；
<!-- more -->
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <title>arrSelect</title>
</head>
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<body>
<div id="app">
      <div>
          <ul>
              <li v-for="(item,index) in tableData" :key="item.index"><input type="checkbox" @click="fn(item,item.checkval)" :checked="item.checkval" /><span>{{ item.date }}-{{ item.name }}-{{ item.preference }}</span></li>
          </ul>
      </div>
</div>
</body>


<script type="text/javascript">
    var vue =new Vue({
        el: "#app",
        data() {
          return {
          tableData: [{
                  date: '2016-05-04',
                  name: '智立方',
                  checkval: false,
                  preference: '',
                  address: '上海市番禺路868'
                }, {
                  date: '2016-05-04',
                  name: '智立方',
                  checkval: false,
                  preference: '',
                  address: '上海市番禺路868'
                }, {
                  date: '2016-05-04',
                  name: '智立方',
                  checkval: false,
                  preference: '',
                  address: '上海市番禺路868'
                }, {
                  date: '2016-05-04',
                  name: '智立方',
                  checkval: false,
                  preference: '',
                  address: '上海市番禺路868'
                }, {
                  date: '2016-05-04',
                  name: '智立方',
                  checkval: false,
                  preference: '',
                  address: '上海市番禺路868'
                }],
                handleArr:[], // 选中的数组
                handleNum: 0 // 选中的顺序
              }
        },
        methods: {
          fn(val,status){
            val.checkval = !status // 再次点击反选字段;
            if( val.checkval ){ // 根据是否选中来处理相应的选中顺序;
                this.handleArr.push(val); // 选中则添加到选中数组；
                val.preference = (this.handleNum<=this.handleArr.length ?  this.handleNum+=1 : this.handleNum = this.handleArr.length); // 根据选中数组的长度来增加选中顺序值；
            } else {
                this.handleArr.splice(this.handleArr.indexOf(val),1); // 反选之后删除选中的数组
                for( let item in this.tableData ){ // 循环绑定的数据来判断顺序值是否需要减少；
                    if( this.tableData[item].preference>0 && this.tableData[item].preference > val.preference  ){
                        this.tableData[item].preference --;
                        this.handleNum--;
                    }
                }
                this.handleNum = this.handleArr.length; // 将同步数组的长度附顺序字段；目的是获取顺序值目前的最大值；
                val.preference = ''; // 如果是反选则清空顺序值
            }
          }
      }
 })
</script>
</html>
```

个人理解 仅供参考 如有不当 欢迎指正
