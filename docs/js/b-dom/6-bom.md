# BOM

<img src="https://raw.githubusercontent.com/hughxusu/lesson-web/refs/heads/developing/_images/d-js/bom.png" style="zoom:80%;" />

BOM（Browser Object Model）是浏览器对象模型。

* `window`是浏览器内置中的全局对象，所有Web APIs的知识内容都是基于`window`对象实现。
* `window`对象下包含了`navigator`、`location`、`document`、`history`、`screen`5个属性，即所谓的 BOM （浏览器对象模型）。
* `document`是实现DOM的基础，它其实是依附于`window`的属性。

```js
window.console.dir(window)
```

> [!warning]
>
> 依附于`window`对象的所有属性和方法，使用时可以省略`window`。

## 定时器

`setTimeout(func, delay)`让代码延迟执行，`func`回调函数，`delay`延迟时间。返回定时器id值。使用`clearTimeout`可以关闭定时器。

```html
<h1></h1>
<button>
  Click me
</button>
<script>
  let h1 = document.querySelector('h1')
  let btn = document.querySelector('button')
  let timer = 0
  function fn() {
    h1.textContent = new Date().toLocaleTimeString()
    timer = setTimeout(fn, 1000)
  }
  fn()

  btn.addEventListener('click', function() {
    clearTimeout(timer)
    h1.textContent = 'Timer stopped'
  })
</script>
```

## JS执行机制

Javascript语言单线程编程语言，这由Javascript语言的使命决定——处理页面中用户的交互。

* 同一个时间只能完成一个任务。比如用户对某个`DOM`元素进行添加和删除操作，不能同时进行。应该先进行添加，之后再删除。
* 所有任务需要排队，前一个任务结束，才会执行后一个任务。

> [!note]
>
> 单线程的问题是： 如果 JS 执行的时间过长，这样就会造成页面的渲染不连贯，导致页面渲染加载阻塞的感觉。

HTML5提出Web Worker标准，允许JavaScript脚本创建多个线程，出现了同步和异步。

* 同步：前一个任务结束后再执行后一个任务，程序的执行顺序与任务的排列顺序是一致的、同步的。
  * 同步任务都在主线程上执行，形成一个执行栈。
    * js引擎模块：负责js的编译和运行。
    * html/css文档解析模块：负责页面文本的解析。
    * dom模块：负责dom在内存中的相关处理。
    * 布局和渲染模块：负责页面的布局和效果的绘制。
* 异步：在处理耗时任务是，可以完成其他任务。
  * 异步任务放入任务队列中，通过回调函数实现。
    * 定时器模块：负责定时器的管理（`setInterval`、`setTimeout`）
    * 网络请求模块：负责服务器请求
    * 事件响应模块：负责事件的管理（如：`click`、`resize`）

JS执行机制

1. 先执行执行栈中的同步任务。
2.  异步任务放入任务队列中。
3.  一旦执行栈中的所有同步任务执行完毕，系统就会按次序读取任务队列中的异步任务，于是被读取的异步任务结束等待状态，进入执行栈，开始执行。

<img src="https://raw.githubusercontent.com/hughxusu/lesson-web/refs/heads/developing/_images/d-js/1633593121990_图片1.png" style="zoom:75%;" />

```html
<div class="root"></div>
<script>
  let root = document.querySelector('.root');
  root.appendChild(document.createElement('h1')).textContent = '1';
  document.addEventListener('click', function() {
    root.appendChild(document.createElement('h1')).textContent = '4';
  });
  setTimeout(function() {
    root.appendChild(document.createElement('h1')).textContent = '3';
  }, 3000);
  root.appendChild(document.createElement('h1')).textContent = '2';
</script>
```

> [!warning]
>
> 对于耗时操作，需要主动放到异步任务中去。

## 页面导航

### location对象

`location`对象拆分并保存了URL地址的各个组成部分。

*  `href`属性获取完整的URL地址，对其赋值时用于地址的跳转。

```html
<style>
  span {
    color: red;
  }
</style>
<a href="https://www.baidu.com/">支付成功，<span>5</span> 秒之后跳转首页</a>
<script>
  let a = document.querySelector('a')
  let num = 5
  let timer = setInterval(function () {
    num--
    a.innerHTML = `支付成功，<span>${num}</span> 秒之后跳转首页`
    if (num === 0) {
      clearInterval(timer)
      location.href = 'https://www.baidu.com/'
    }
  }, 1000)
</script>
```

* `search`属性获取地址中携带的参数，符号?后面部分。
* `hash`属性获取地址中的啥希值，符号#后面部分。
* `reload`方法用来刷新当前页面，传入参数`true`时表示强制刷新。

```html
<style>
  .timer {
    margin-top: 60px;
    text-align: center;
  }
  #startButton {
    padding: 8px 12px;
    font-size: 16px;
    cursor: pointer;
    background-color: skyblue;
    border: 0;
  }
  #startButton:disabled {
    background-color: #ccc;
    cursor: not-allowed;
  }
</style>
<div class="timer">
  <h1 id="seconds">10</h1>
  <button id="startButton">renew</button>
</div>
<script>
  let seconds = document.querySelector('#seconds');
  let startButton = document.querySelector('#startButton');
  let intervalId;
  let aim = +new Date() + 10 * 1000;

  function time() {
    let now = Date.now();
    let count = Math.round((aim - now) / 1000);
    if (count <= 0) {
      clearInterval(intervalId);
      startButton.disabled = false;
      count = 0;
    }
    seconds.innerHTML = count;
  }
  time();
  setInterval(time, 1000);

  startButton.onclick = function () {
    location.reload();
  }
</script>
```

### navigator对象

`navigator`对象记录了浏览器自身相关信息。

* 通过`userAgent`属性可以检测浏览器的版本及平台。

```html
<h1></h1>
<script>
  let h1 = document.querySelector('h1')
  h1.innerText = navigator.userAgent
</script>
```

### histroy对象

`history`对象与浏览器地址栏操作相对应，如前进、后退、历史记录等。

* `back()`后退
* `forward()`前进
* `go(param)`跳转功能，`param`为跳转页数，1向前跳转一页，-1向后跳转一页。

```html
<button class="forward">前进</button>
<button class="back">后退</button>
<script>
  let forward = document.querySelector('.forward')
  let back = document.querySelector('.back')
  forward.addEventListener('click', function () {
    history.forward()
  })
  back.addEventListener('click', function () {
    history.back()
  })
</script>
```

## 本地存储

随着互联网的快速发展，基于网页的应用越来越复杂，为了满足各种需求，经常会在本地存储大量的数据，HTML5规范提出了相关解决方案。

* 数据存储在用户浏览器中。
* 设置、读取方便、甚至页面刷新不丢失数据
* 容量较大，`sessionStorage`和`localStorage`约5M左右

### localStorage

1. 生命周期永久生效，除非手动删除否则关闭页面也会存在。
2. 可以多页面共享数据。
3. 以键值对的形式存储使用。

存储数据：`localStorage.setItem(key, value)`，`key`数据名字，`value`保存数据值。获取数据：`localStorage.getItem(key)`，`key`数据名字。

```html
<style>
  input, button {
    padding: 4px;
    font-size: 16px;
  }

  input {
    width: 200px;
  }
</style>
<input value="山中无历日，寒尽不知年">
<button>save</button>
<h1></h1>
<script>
  let btn = document.querySelector('button')
  let h1 = document.querySelector('h1')
  let input = document.querySelector('input')
  btn.addEventListener('click', function () {
    let msg = input.value
    if (!msg) return
    localStorage.setItem('msg', msg)
    h1.innerHTML = msg
  })
  let msg = localStorage.getItem('msg')
  if (msg) {
    h1.innerHTML = msg
  }
</script>
```

### 引用数据类型的存储

本地只能存储字符串，无法存储复杂数据类型。需要将复杂数据类型转换成JSON字符串，在存储到本地。

`JSON.stringify(obj)`对象转换成json字符串，`obj`对象数据；`JSON.parse(str)`将json字符串转换成数据，`str`字符串数据。

```html
<style>
  button {
    font-size: 16px;
    padding: 6px;
  }
</style>
<button id="save">save item</button>
<button id="button">load item</button>
<h1></h1>
<script>
  let user = {
    name: 'Tom',
    age: 18,
    isMale: true
  }
  JSON.stringify(user)

  let saveBtn = document.querySelector('#save')
  saveBtn.addEventListener('click', function () {
    localStorage.setItem('user', JSON.stringify(user))
  })

  let loadBtn = document.querySelector('#button')
  let h1 = document.querySelector('h1')
  loadBtn.addEventListener('click', function () {
    let user = JSON.parse(localStorage.getItem('user'))
    if (!user) return
    console.log(user)
    h1.innerText = `user name is ${user.name}, age is ${user.age}, isMale is ${user.isMale}`
  })
</script>
```



