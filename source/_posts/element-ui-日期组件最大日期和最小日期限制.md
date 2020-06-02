---
title: element-ui 日期组件最大日期和最小日期限制
date: 2020-06-02 14:49:09
tags:
---
element-ui 日期组件最大日期和最小日期
```html
<el-date-picker
	v-model="transactionDate"
	type="daterange"
	range-separator="至"
	start-placeholder="开始日期"
	end-placeholder="结束日期"
	placeholder="选择日期"
	value-format="yyyy-MM-dd HH:mm:ss"
	:default-time="['00:00:00','23:59:59']"
	:picker-options="isDisabled"
>
</el-date-picker>
```
<!-- more -->
设置最大选择日期和最小选择日期
```javascript
isDisabled: {
  disabledDate(time) {
  	// 大于某个日期不能选择
  	let myDate = new Date();
    let _beforeDay = myDate.setDate(new Date().getDate() - 1);
  	return time.getTime() >= _beforeDay;
    // 设置日期限制 小于某个日期不能选择
    return time.getTime() < new Date('2019/07/25 00:00:00').getTime() - 8.64e7;
  }
},
```
