---
title: bootstrap-table一个td使用多个数据
date: 2020-06-02 18:01:50
tags:
---
在bootstrap-table 里面在定义title 的时候

```
var bkDetTit = [
        {
        field:'Number',
        title:'编号',
        formatter: function( value, row, index ){
                return index + 1;
            }
        },
        {
            field:'股票代码',
            title:'股票代码及行业',
            formatter:function( value,row,index ){
	            //如何使用拿到的多个数据 直接返回拼接好的html;
                var html = "<span>"+row["股票代码"]+"</span><span>"+row["股票名称"]+"</span><span>"+row["所属行业"]+"</span>"
                return html;
            }
        },
        {
            field:'起始价格',
            title:'起始价格',
            sortable:true
        },
        {
            field:'截止价格',
            title:'截止价格',
            sortable:true
        }
        ]
```
里面有**formatter(value,row,index)**方法可以用来返回多个需要的数据，其中里面有三个参数（value,row,index）;
**value: 返回该field对应的value**
**row: 是返回表格的所有数据**
**index:返回该行数据的下标**
