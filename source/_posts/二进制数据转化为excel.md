---
title: 二进制数据转化为excel
date: 2020-06-02 14:59:35
tags:
---
原理就是通过a标签的href属性将二进制表格数据转化为表格，再通过download属性将文件下载到本地；
需要注意的是接口请求的数据类型要设置为 **blob** 类型 [具体参考 axios https://www.npmjs.com/package/axios](https://www.npmjs.com/package/axios)
```javascript
  responseType: 'blob'
```
<!-- more -->
具体的代码
```javascript
   let url = window.URL.createObjectURL(res.data);
   let link = document.createElement('a');
   link.style.display = 'none';
   link.href = url;
   link.setAttribute('download', filename); // filename 自定义下载的表格名称及后缀名；
   document.documentElement.appendChild(link);
   link.click();
   document.documentElement.removeChild(link);
```
完整的例子：

```javascript
	 // 首先要引入axios
	  import axios from 'axios';
	// 组件内部新增方法
      downloadExl(filename) {
        axios({
            method: 'get',
            url: '/api/export',
            params: Object, // Object 导出接口所需要的参数对象
            responseType: 'blob'
          }).then(res => {
            let url = window.URL.createObjectURL(res.data);
            let link = document.createElement('a');
            link.style.display = 'none';
            link.href = url;
            link.setAttribute('download', filename);
            document.documentElement.appendChild(link);
            link.click();
            document.documentElement.removeChild(link);
          }).catch(e => {
          	console.log('导出失败')
          }).finally(() => {
          	console.log('不管导出成功/失败')
          });
      },
```
