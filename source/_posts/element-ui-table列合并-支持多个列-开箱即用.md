---
title: element-ui table列合并--支持多个列 开箱即用
date: 2020-06-02 15:03:43
tags:
---
项目中使用table组件的时候，存在合并列或者合并行看element-ui table组件的文档
由于数据是动态获取，所以存在合并不方便的场景
所以换个思路来实现合并
具体代码：
<!-- more -->
```html
      <el-table
        :data="tableData"
        :span-method="arraySpanMethod"
        border
        style="width: 100%; margin-top: 20px">
        <el-table-column
          prop="id"
          label="ID"
          width="180">
        </el-table-column>
        <el-table-column
          prop="name"
          label="姓名">
        </el-table-column>
        <el-table-column
          prop="amount1"
          label="数值 1（元）">
        </el-table-column>
        <el-table-column
          prop="amount2"
          label="数值 2（元）">
        </el-table-column>
        <el-table-column
          prop="amount3"
          label="数值 3（元）">
        </el-table-column>
      </el-table>
```
```js
export default {
	data(){
		return {
		  // 模拟后台返回数据
		  tableData: [{
	          id: '12987122',
	          name: '王小虎',
	          amount1: '234',
	          amount2: '3.2',
	          amount3: 10
	        }, {
	          id: '12987123',
	          name: '王小虎',
	          amount1: '165',
	          amount2: '4.43',
	          amount3: 12
	        }, {
	          id: '12987124',
	          name: '王小虎',
	          amount1: '324',
	          amount2: '1.9',
	          amount3: 9
	        }, {
	          id: '12987125',
	          name: '王小虎',
	          amount1: '621',
	          amount2: '2.2',
	          amount3: 17
	        }, {
	          id: '12987126',
	          name: '王小虎',
	          amount1: '539',
	          amount2: '4.1',
	          amount3: 15
	        }, {
	          id: '12987126',
	          name: '王小虎3',
	          amount1: '539',
	          amount2: '4.1',
	          amount3: 15
	        }, {
	          id: '12987126',
	          name: '王小虎2',
	          amount1: '539',
	          amount2: '4.1',
	          amount3: 15
	        }, {
	          id: '12987126',
	          name: '王小虎2',
	          amount1: '539',
	          amount2: '4.1',
	          amount3: 15
	        }],
        needMergeArr: ['name', 'id'], // 有合并项的列
     	rowMergeArrs: {}, // 包含需要一个或多个合并项信息的对象
		};
	},
	methods:{
		    /**
		     * @description 实现合并行或列
		     * @param row:Object 需要合并的列name 如:'name' 'id'
		     * @param column:Object 当前行的行数，由合并函数传入
		     * @param rowIndex:Number 当前列的数据，由合并函数传入
		     * @param columnIndex:Number 当前列的数据，由合并函数传入
		     *
		     * @return 函数可以返回一个包含两个元素的数组，第一个元素代表rowspan，第二个元素代表colspan。 也可以返回一个键名为rowspan和colspan的对象
		     * 参考地址：https://element.eleme.cn/#/zh-CN/component/table#table-biao-ge
		    */
		    arraySpanMethod({ row, column, rowIndex, columnIndex }) {
		        // 没办法循环判断具体是那一列 所以就只好写了多个if
		        if (column.property === 'name') return this.mergeAction('name', rowIndex, column);
		        if (column.property === 'id') return this.mergeAction('id', rowIndex, column);
		    },
		    /**
		     * @description 根据数组来确定单元格是否需要合并
		     * @param val:String 需要合并的列name 如:'name' 'id'
		     * @param rowIndex:Number 当前行的行数，由合并函数传入
		     * @param colData:Object 当前列的数据，由合并函数传入
		     *
		     * @return 返回值为一个数组表示该单元格是否需要合并; 说明: [0,0]表示改行被合并了 [n+,1]n为1时表示该单元格不动,n大于1时表示合并了N-1个单元格
		    */
		    mergeAction(val, rowIndex, colData) {
		        let _row = this.rowMergeArrs[val].rowArr[rowIndex];
		        let _col = _row > 0 ? 1 : 0;
		        return [_row, _col];
		    },
		    /**
		     * @description 根据数组将指定对象转化为相应的数组
		     * @param arr:Array[String] 必填 必须是字符串形式的数组
		     * @param data:Array 必填 需要转化的对象
		     *
		     * @return 返回一个对象
		     * 例：rowMerge(['name','value'], [{value:'1', name:'小明'}, {value:'2', name:'小明'}, {value:'3', name:'小明'}, {value:'1', name:'小明'}, {value:'1', name:'小明'}])
		     * 返回值: {
		     *          name:{
		     *            rowArr: [5, 0, 0, 0, 0]
		     *            rowMergeNum: 0,
		     *          },
		     *          value: {
		     *            rowArr: [1, 1, 1, 2, 0],
		     *            rowMergeNum: 3
		     *          }
		     *        }
		    */
		    rowMergeHandle(arr, data) {
		      if (!Array.isArray(arr) && !arr.length) return false;
		      if (!Array.isArray(data) && !data.length) return false;
		      let needMerge = {};
		      arr.forEach(i => {
		        needMerge[i] = {
		          rowArr: [],
		          rowMergeNum: 0
		        };
		        data.forEach((item, index) => {
		          if (index === 0) {
		            needMerge[i].rowArr.push(1);
		            needMerge[i].rowMergeNum = 0;
		          } else {
		            if (item[i] === data[index - 1][i]) {
		              needMerge[i].rowArr[needMerge[i].rowMergeNum] += 1;
		              needMerge[i].rowArr.push(0);
		            } else {
		              needMerge[i].rowArr.push(1);
		              needMerge[i].rowMergeNum = index;
		            }
		          }
		        });
		      });
		      return needMerge;
		    }
		  }
	},
	mounted(){
		this.rowMergeArrs = this.rowMergeHandle(this.needMergeArr, this.tableData); // 处理数据
	}
}
```
效果如下：
[![ttGzE4.png](https://s1.ax1x.com/2020/06/02/ttGzE4.png)](https://imgchr.com/i/ttGzE4)

[参考地址 - elementUI表格合并单元格](https://www.cnblogs.com/yuwenjing0727/p/10110721.html)
