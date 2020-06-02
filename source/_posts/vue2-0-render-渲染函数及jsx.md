---
title: vue2.0 render()渲染函数及jsx
date: 2020-06-02 16:37:53
tags:
---
render()函数通过虚拟DOM 来渲染页面
>因为它所包含的信息会告诉 Vue 页面上需要渲染什么样的节点，及其子节点。我们把这样的节点描述为“虚拟节点 (Virtual Node)”，也常简写它为“VNode”。“虚拟 DOM”是我们对由 Vue 组件树建立起来的整个 VNode 树的称呼。
														--来源：https://cn.vuejs.org/v2/guide/render-function.html#%E8%99%9A%E6%8B%9F-DOM
<!-- more -->
```javascript
createElement()函数用来生成模板：
参数说明：
createElement(
// @returns {VNode}
createElement(
  // {String | Object | Function}
  // 一个 HTML 标签字符串，组件选项对象，或者
  // 解析上述任何一种的一个 async 异步函数。必需参数。
  'div',

  // {Object}
  // 一个包含模板相关属性的数据对象
  // 你可以在 template 中使用这些特性。可选参数。
  {
    // 和`v-bind:class`一样的 API
  // 接收一个字符串、对象或字符串和对象组成的数组
	  'class': {
	    foo: true,
	    bar: false
	  },
	  // 和`v-bind:style`一样的 API
	  // 接收一个字符串、对象或对象组成的数组
	  style: {
	    color: 'red',
	    fontSize: '14px'
	  },
	  // 普通的 HTML 特性
	  attrs: {
	    id: 'foo'
	  },
	  // 组件 props
	  props: {
	    myProp: 'bar'
	  },
	  // DOM 属性
	  domProps: {
	    innerHTML: 'baz'
	  },
	  // 事件监听器基于 `on`
	  // 所以不再支持如 `v-on:keyup.enter` 修饰器
	  // 需要手动匹配 keyCode。
	  on: {
	    click: this.clickHandler
	  },
	  // 仅用于组件，用于监听原生事件，而不是组件内部使用
	  // `vm.$emit` 触发的事件。
	  nativeOn: {
	    click: this.nativeClickHandler
	  },
	  // 自定义指令。注意，你无法对 `binding` 中的 `oldValue`
	  // 赋值，因为 Vue 已经自动为你进行了同步。
	  directives: [
	    {
	      name: 'my-custom-directive',
	      value: '2',
	      expression: '1 + 1',
	      arg: 'foo',
	      modifiers: {
	        bar: true
	      }
	    }
	  ],
	  // 作用域插槽格式
	  // { name: props => VNode | Array<VNode> }
	  scopedSlots: {
	    default: props => createElement('span', props.text)
	  },
	  // 如果组件是其他组件的子组件，需为插槽指定名称
	  slot: 'name-of-slot',
	  // 其他特殊顶层属性
	  key: 'myKey',
	  ref: 'myRef',
	  // 如果你在渲染函数中向多个元素都应用了相同的 ref 名，
	  // 那么 `$refs.myRef` 会变成一个数组。
	  refInFor: true
  },

  // {String | Array}
  // 子虚拟节点 (VNodes)，由 `createElement()` 构建而成，
  // 也可以使用字符串来生成“文本虚拟节点”。可选参数。
  [
    '先写一些文字',
    createElement('h1', '一条数据'),
    createElement(MyComponent, {
      props: {
        someProp: 'foobar'
      }
    })
  ]
)
)
```
例：
```javascript
render(h){
	return h(
	'div',
	{
	  'class': {
        foo: true,
        bar: false
      },
      style: {
        // color: 'red',
        fontSize: '14px',
        width: '100px',
        // height: '20px',
        backgroundColor: '#bf0000'
      },
      attrs: {
        id: 'foo'
      },
      // 需要手动匹配 keyCode。
      on: {
        click: bindclickFun
      },
    },
    [
      '一些内容',
      createElement('h1', '一条文字')
    ]
  )
}
```

jsx  参考：https://www.jianshu.com/p/feede4445142
例：
```javascript
// ------data
randerProp:[
        {
          prop: 'month',
          label: '日期'
        },
        {
          prop: 'district',
          label: '区域'
        },
        {
          prop: 'starLevel',
          label: '星级'
        },
        {
          prop: 'maturity',
          label: '校区成熟属性'
        },
        {
          prop: 'teacherName',
          label: '班主任'
        },
        { prop: 'validStudentLast', label: '上月活跃人数' },
        { prop: 'newStudent', label: '本月新生人数' },
        { prop: 'leaveStudent', label: '本月CR流失人数', width: 135 },
        { prop: 'validStudent', label: '本月活跃人数' },
        { prop: 'courseNum', label: '续费科目数' },
        { prop: 'studentNum', label: '续费人次数' },
        { prop: 'validCourseLast', label: '上月活跃科目数', width: 116 },
        { prop: 'validCourse', label: '本月活跃科目' },
        { prop: 'addSubject', label: '扩科数' },
        { prop: 'addSubjectRate', label: '扩科续科率', filter: 'rate' },
        { prop: 'validCourseAvg', label: '活跃人均科目数', width: 116 },
        { prop: 'referralPeopleNum', label: '转介绍人次数' },
        { prop: 'deposit', label: '定金' },
        { prop: 'signNum', label: '签单数' },
        { prop: 'inPerformance', label: '业绩' },
        { prop: 'outPerformance', label: '退费' },
        { prop: 'performance', label: '净业绩' }
      ],
        renderData: [
            {
              addSubject: 0.0,
              addSubjectRate: 0.0,
              courseNum: 0.0,
              deposit: 0.0,
              inPerformance: 8660.0,
              leaveStudent: 0.0,
              maturity: '成熟校区',
              month: '2019-01-01',
              newStudent: 0.0,
              outPerformance: 0.0,
              performance: 8660.0,
              referralPeopleNum: 0.0,
              signNum: 1.0,
              starLevel: '五星校区',
              studentNum: 1.0,
              teacherName: '小二',
              validCourse: 0.0,
              validCourseAvg: 0.0,
              validCourseLast: 0.0,
              validStudent: 0.0,
              validStudentLast: 0.0
            },
            {
              addSubject: 0.0,
              addSubjectRate: 0.0,
              courseNum: 0.0,
              deposit: 0.0,
              inPerformance: 0.0,
              leaveStudent: 0.0,
              maturity: '成熟校区',
              month: '2019-01-01',
              newStudent: 0.0,
              outPerformance: 0.0,
              performance: 0.0,
              referralPeopleNum: 0.0,
              signNum: 0.0,
              starLevel: '五星校区',
              studentNum: 0.0,
              teacherName: '王五',
              validCourse: 0.0,
              validCourseAvg: 0.0,
              validCourseLast: 0.0,
              validStudent: 0.0,
              validStudentLast: 0.0
            },
            {
              addSubject: 0.0,
              addSubjectRate: 0.0,
              courseNum: 0.0,
              deposit: 0.0,
              inPerformance: 0.0,
              leaveStudent: 15.0,
              maturity: '成熟校区',
              month: '2019-01-01',
              newStudent: 0.0,
              outPerformance: 0.0,
              performance: 0.0,
              referralPeopleNum: 1.0,
              signNum: 0.0,
              starLevel: '五星校区',
              studentNum: 0.0,
              teacherName: '张三',
              validCourse: -30.0,
              validCourseAvg: 2.0,
              validCourseLast: 0.0,
              validStudent: -15.0,
              validStudentLast: 0.0
            },
            {
              addSubject: 0.0,
              addSubjectRate: 0.0,
              courseNum: 0.0,
              deposit: 0.0,
              inPerformance: 15470.0,
              leaveStudent: 15.0,
              maturity: '成熟校区',
              month: '2019-01-01',
              newStudent: 0.0,
              outPerformance: 0.0,
              performance: 15470.0,
              referralPeopleNum: 1.0,
              signNum: 1.0,
              starLevel: '五星校区',
              studentNum: 1.0,
              teacherName: '陈四',
              validCourse: -60.0,
              validCourseAvg: 4.0,
              validCourseLast: 0.0,
              validStudent: -15.0,
              validStudentLast: 0.0
            },

          ]

render(){
	return (
        <el-table data={this.renderData}>
          {this.randerProp.map(item=>{
            if(item.prop == 'teacherName'){
              return <el-table-column prop={item.prop} label={item.label} header-align="center" align="center" {...{scopedSlots:{
              default:(props)=>{
                if( props.row.teacherName == '陈四'){
                  return h('el-button', {props:{type:'primary'}}, ['按钮'])
                } else {
                  return props.row.teacherName
                }
              }
            }}}></el-table-column>
            } else {
              return <el-table-column prop={item.prop} label={item.label} show-overflow-tooltip header-align="center" align="center" ></el-table-column>
            }
          })}
        </el-table>
	)
}
```
