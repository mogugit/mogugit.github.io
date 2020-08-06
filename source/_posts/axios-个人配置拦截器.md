---
title: axios 个人配置拦截器
date: 2020-08-06 10:08:43
tags:
  - axios
  - axios 拦截器
---

个人配置 axios 拦截器
<!-- more -->
```js
import Vue from 'vue';
import axios from 'axios';
import { Message } from 'element-ui';
Vue.component(Message.name, Message);
let sessionLoginId = sessionStorage.getItem('LoginId') || '';
axios.interceptors.request.use((res) => {
  res.headers['TIME_TOKEN'] = new Date().getTime();
  res.headers['Cache-Control'] = 'no-cache'; // 以秒为单位 no-cache
  res.headers['Token'] = sessionLoginId;
  try {
    if (!res) return;
    res.headers['content-type'] = 'application/json; charset=UTF-8';
    if (res.method === 'get') {
      res.data = {unused: 0};
    }
  } catch (e) {
    console.error(e);
  }
  return res;
}, (err) => {
  return Promise.reject(err);
});
// 添加响应拦截器-错误处理
axios.interceptors.response.use(res => {
    return res;
  },
  err => {
    // 无权限-40013  sessionId过期-40005 均跳转到登录
    if (err && err.response && err.response.data.code === 40005) {
      location.href = err.response.data.extraInfo;
    } else if (err && err.response && err.response.data.code === 40013) {
      Vue.prototype.$message({
        message: err.response.data.msg || '系统异常，请联系管理员！',
        type: 'error'
      });
      setTimeout(function() {
        axios.get('/admin/logout');
        sessionStorage.clear();
      }, 3000);
    } else {
      let errRes = err.response.data.extraInfo || err.response.data.msg || '系统异常，请联系管理员！';
      if (errRes.length >= 40) errRes = errRes.slice(0, 40) + '...';

      Vue.prototype.$message({
        message: errRes,
        type: 'error'
      });
    }
    return Promise.reject(err);
  }
);

export default class BaseApi {
  constructor() {
    this.axios = axios;
  }
}

```
记录配置拦截器;
