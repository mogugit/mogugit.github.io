---
title: DOM 事件深入浅出
date: 2020-06-02 13:24:53
tags:
---
#### DOM级别与DOM事件
`DOM`级别一共可以分为4个级别：`DOM0级`，`DOM1级`，`DOM2级`和 `DOM3级`，而`DOM`事件分为3个级别：`DOM0` 级事件处理，`DOM2` 级事件处理和 `DOM3` 级事件处理。如下图所示：
![ttpKa9.png](https://s1.ax1x.com/2020/06/02/ttpKa9.png)
**为什么没有DOM1级事件处理呢？**
因为1级 `DOM` 标准中并没有定义事件相关的内容，所以没有所谓的1级 `DOM` 事件模型。

<!-- more -->
#### 1.DOM0级事件
在了解 `DOM0` 级事件之前，我们有必要先了解下 `HTML` 事件处理程序，也是最早的这一种的事件处理方式，代码如下：
```html
<button type="button" onclick="showFn()"></button>

<script>
    function showFn() {
        alert('Hello World');
    }
</script>
```
以上代码我们通过直接在 `HTML` 代码里定义了一个 `onclick` 的属性触发 `showFn` 方法，这样的事件处理程序最大的缺点就是 `HTML` 于 `JS` 强耦合，我们一旦需要修改函数名就得修改两个地方。当然其优点是不需要操作 `DOM` 来完成事件的绑定。

那么什么是`DOM0`级处理事件呢？`DOM0`级事件就是将一个函数赋值给一个事件处理属性，比如：
```html
<button id="btn" type="button"></button>

<script>
    var btn = document.getElementById('btn');

    btn.onclick = function() {
        alert('Hello World');
    }

    // btn.onclick = null; 解绑事件 释放占用内存；
</script>
```
以上代码我们给 `button` 定义了一个 `id`，通过 `JS` 获取到了这个 `id` 的按钮，并将一个函数赋值给了一个事件处理属性 `onclick`，这样的方法便是 `DOM0级` 处理事件的体现。我们可以通过给事件处理属性赋值null来解绑事件。

`DOM0 级`事件处理程序的缺点在于同一个处理程序无法同时绑定多个处理函数，比如我还想在按钮点击事件上加上另外一个函数。比如：
```html
<button id="btn" type="button"></button>

<script>
    var btn = document.getElementById('btn');

    btn.onclick = function() {
        alert('Hello World');
    }  // 被覆盖，并不执行
    btn.onclick = function() {
        alert('Hello JavaScript');
    }

    // btn.onclick = null; 解绑事件 释放占用内存；
</script>
```
#### 2.DOM2级事件
`DOM2级` 事件在 `DOM0 级`事件的基础上弥补了一个处理程序无法同时绑定多个处理函数的缺点，允许给一个处理程序添加多个处理函数。代码如下：
```html
<button id="btn" type="button"></button>

<script>
    var btn = document.getElementById('btn');

    function showFn() {
        alert('Hello World');
    }

    btn.addEventListener('click', showFn, false);

    // btn.removeEventListener('click', showFn, false); 解绑事件
</script>
```
`DOM2级` 事件定义了 `addEventListener` 和 `removeEventListener` 两个方法，分别用来绑定和解绑事件，方法中包含3个参数，分别是绑定的事件处理属性名称（不包含`on`）、处理函数和是否在捕获时执行事件处理函数。如果我们还需要添加一个鼠标移入的方法，只需要：
```js
btn.addEventListener('mouseover', showFn, false);
```
这样点击按钮和鼠标移入时都将触发`showFn`方法。

需要注意的是IE8级以下版本不支持 `addEventListener` 和 `removeEventListener`，需要用attachEvent和detachEvent来实现：
```js
btn.attachEvent('onclick', showFn); // 绑定事件
btn.detachEvent('onclick', showFn); // 解绑事件
```
这里我们不需要传入第三个参数，因为IE8级以下版本只支持冒泡型事件。

#### 3.DOM3级事件
`DOM3` 级事件在 `DOM2` 级事件的基础上添加了更多的事件类型，全部类型如下：

UI事件，当用户与页面上的元素交互时触发，如：`load`、`scroll`
焦点事件，当元素获得或失去焦点时触发，如：`blur`、`focus`
鼠标事件，当用户通过鼠标在页面执行操作时触发如：`dbclick`、`mouseup`
滚轮事件，当使用鼠标滚轮或类似设备时触发，如：`mousewheel`
文本事件，当在文档中输入文本时触发，如：`textInput`
键盘事件，当用户通过键盘在页面上执行操作时触发，如：`keydown`、`keypress`
合成事件，当为IME（输入法编辑器）输入字符时触发，如：`compositionstart`
变动事件，当底层 `DOM` 结构发生变化时触发，如：`DOMsubtreeModified`

同时 `DOM3` 级事件也允许使用者自定义一些事件。

#### DOM事件流
上文中讲到了 `addEventListener` 的第三个参数为指定事件是否在捕获阶段执行，设置为 `true` 表示事件在捕获阶段执行，而设置为 `false` 表示事件在冒泡阶段执行。那么什么是事件冒泡和事件捕获呢？可以用下图来解释：

![ttpuVJ.png](https://s1.ax1x.com/2020/06/02/ttpuVJ.png)
#### 1.事件冒泡
所谓事件冒泡就是事件像泡泡一样从最开始生成的地方一层一层往上冒，比如上图中 `a` 标签为事件目标，点击 `a` 标签后同时也会触发 `p`、`li` 上的点击事件，一层一层向上直至最外层的 `html` 或  `document`。下面是代码示例：
```html
<div id="box">
    <a id="child">事件冒泡</a>
</div>

<script>
 var box = document.getElementById('box'),
 child = document.getElementById('child');

child.addEventListener('click', function() {
    alert('我是目标事件');
 }, false);

 box.addEventListener('click', function() {
    alert('事件冒泡至DIV');
 }, false);
</script>
```
上面的代码运行后我们点击 `a` 标签，首先会弹出'我是目标事件'提示，然后又会弹出'事件冒泡至 `DIV` 的提示，这便说明了事件自内而外向上冒泡了。

那么我们如何阻止事件冒泡呢？这里就涉及事件的 `Event` 对象中的 `stopPropagation` 方法，如下：
```js
child.addEventListener('click', function(e) {
    alert('我是目标事件');
    e.stopPropagation();
}, false);
```
加上 `stopPropagation` 方法后，我们再次点击 `a` 标签就不会触发 `div` 上的 `click` 事件了。

#### 2.事件捕获
和事件冒泡相反，事件捕获是自上而下执行，我们只需要将 `addEventListener` 的第三个参数改为true就行。
```html
<div id="box">
    <a id="child">事件冒泡</a>
</div>

<script>
 var box = document.getElementById('box'),
 child = document.getElementById('child');

 child.addEventListener('click', function() {
     alert('我是目标事件');
 }, true);

 box.addEventListener('click', function() {
    alert('事件冒泡至DIV');
 }, true);
</script>
```
此时我们点击 `a` 标签，首先弹出的是'事件冒泡至 `DIV`，其次弹出的是'我是目标事件'，正好与事件冒泡相反。
