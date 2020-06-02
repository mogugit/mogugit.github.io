---
title: 使用mockjs 模拟前后台交互
date: 2020-06-02 16:55:42
tags:
---
使用背景： vue项目 axios
使用详情：
1、首先安装
```javascript
	# 在项目中安装
    npm install mockjs
```
2、在项目中使用
在项目中src文件夹下 新建mock文件夹 新建mock.js 和index.js文件 这里面用来生成基础的接口
项目结构截图：
![tt54qf.png](https://s1.ax1x.com/2020/06/02/tt54qf.png)
数据
<!-- more -->
mock.js 文件
```javascript
//-----------------mock.js-------------------
// 引入mockjs
import Mock from 'mockjs'

// 创建模拟数据 具体的数据生成方法 请查看文档http://mockjs.com/examples.html
function creatPostMock () {
  const list = []
  const mockdata = {
    id: '@increment', // 数据定义 @increment
    'object|1': {
      '310000': '上海市',
      '320000': '江苏省',
      '330000': '浙江省',
      '340000': '安徽省'
    },
    name: '@pick(["a", "e", "i", "o", "u"])',
    m1: '@integer(60, 100)',
    m2: '@integer(60, 100)',
    m3: '@integer(60, 100)',
    m4: '@integer(60, 100)',
    m5: '@integer(60, 100)',
    m6: '@integer(60, 100)',
    m7: '@integer(60, 100)',
    m8: '@integer(60, 100)',
    m9: '@integer(60, 100)'
  }
  for (var i = 0; i < 10; i++) {
    const a = Mock.mock(mockdata)
    list.push(a)
  }
  const data = {
    data: {},
    size: 1,
    pagesize: 10
  }
  data.data = list
  return data
}

// 创建模拟数据
function creatGetMock () {
  const getMock = Mock.mock({
    'list|1-10': [{
      'id|+1': 1
    }]
  })
  return getMock
}

// 将模拟好的数据输出出去；
export {creatPostMock, creatGetMock}
```
index.js 文件
```javascript
// ------------index.js---------------
// 引入mockjs
import Mock from 'mockjs'
// 引入生成的模拟数据
import {creatPostMock, creatGetMock} from './mock'

// 设置请求延时时间
Mock.setup({
  // timeout: 2000 方式一 直接设置值
  timeout: '2000 - 5000' // 方式二 设置区间 注意这个是一个字符串形式
})

// 设置拦截的接口 格式请看文档 https://github.com/nuysoft/Mock/wiki/Mock.mock()
// 注意: 这里拦截的地址 最好使用正则匹配 如果直接使用字符串接口 就有可能拦截不到带参数的请求 报错404
Mock.mock(/\/api\/mock(|\?\S*)$/, 'post', creatPostMock)
// Mock.mock('/api/mock', 'get', creatGetMock) // 方式一
Mock.mock(/\/api\/mock(|\?\S*)$/, 'get', creatGetMock) // 方式二
```
然后在main.js 文件里面引入我们写好的mock/index.js文件 用于拦截请求
```javascript
//------------------main.js-------------------
// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue'
import App from './App'
import router from './router'
import axios from 'axios' // 引入axios
import('./mock/index') // 引入设置好基础的mock, 用于拦截请求

// 设置为 false 以阻止 vue 在启动时生成生产提示。
Vue.config.productionTip = false

// 在vue项目中 axios中无法直接使用vue.use() 所以将axios直接添加到Vue的原型上
Vue.prototype.axios = axios

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})
```
接下来我们就可以定义api了 在api文件下 新建一个自定义接口文件 如questMock.js 里面是我们需要请求数据的模拟接口；
```javascript
//-------------questMock.js----------------
// 引入axios
import axios from 'axios'
// 使用
this.axios = axios
// 封装的post
function postMockList (data) {
  return this.axios.post('/api/mock', {
    data
  })
}
// 封装的get
function getMockList (data) {
  return this.axios.get('/api/mock', {
    data
  })
}
// 输出
export { postMockList, getMockList }
```
最后在组件中使用
```javascript
// ----------------------HelloWorld---------------------
<template>
  <div class="hello">
    <div class="mask" :class="{boxNone:isMask }"></div>
    <div>
      <p>这是获取mock 数据</p>
      <button @click="getMockData">get模拟数据</button>
      <button @click="postMockData">post模拟数据</button>
    </div>
  </div>
</template>

<script>
import { postMockList, getMockList } from '../api/questMock.js'
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: '模拟前后台交互',
      getMock: getMockList,
      postMock: postMockList,
      isMask: true
    }
  },
  methods: {
    getMockData () {
      this.isMask = false
      this.getMock({
        params: {
          name: '隔壁老王'
        }
      }).then(res => {
        this.isMask = true
        console.log('GET模拟数据', res)
      }).catch(e => {
        console.log('错误', e)
      })
    },
    postMockData () {
      this.isMask = false
      this.postMock({
        name: 'xiaoming',
        age: '5'
      }).then(res => {
        this.isMask = true
        console.log('POST模拟数据', res)
      }).catch(e => {
        console.log('错误', e)
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.mask {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1000;
  background-color: green;
  opacity: 0.5;
}
.mask.boxNone {
  display: none;
}
</style>
```
遇到的问题
1、在设置模拟接口时 使用get请求 发现报错404 后来查资料发现是因为直接使用字符串接口会导致mockjs 拦截不到地址  **解决办法就是使用 正则去匹配请求接口**
2、如何设置请求延时 由于mockjs 是在本地模拟数据所以并未发起真正的请求，无法看到请求的加载效果，**解决办法就是使用Mock.setup({timeout: 加载时间}) 来设置每次的模拟请求时间**
也可参考以下：
 [vue-cli 中使用 Mockjs ](https://segmentfault.com/a/1190000014844604).
  [服务器端数据模拟，支持请求转发、返回JSON静态数据、返回JS可变数据 ](https://github.com/Yakima-Teng/mock-server).
