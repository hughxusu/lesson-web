# BOM

<img src="https://raw.githubusercontent.com/hughxusu/lesson-web/develop/images/d-js/bom.png" style="zoom:80%;" />

BOM（Browser Object Model）是浏览器对象模型。

* `window`是浏览器内置中的全局对象，所有Web APIs都是基于`window`对象实现。
* `window`对象下包含了`navigator`、`location`、`document`和`history`等属性，即所谓的 BOM （浏览器对象模型）。
* `document`是实现DOM的基础，它其实是依附于`window`的属性。

```js
window.console.dir(window)
console.log(window)
```

> [!warning]
>
> 依附于`window`对象的所有属性和方法，使用时可以省略`window`。

## 定时器

`setTimeout(func, delay)`让代码延迟执行。`setTimeout`仅仅只执行一次，可以理解为把一段代推迟执行。

* `func`回调函数，`delay`延迟时间。
* 返回定时器id值。
* 使用`clearTimeout`可以关闭定时器。

```html
<body>
    <div>
        <p>5秒后关闭标签</p>
        <img src="https://raw.githubusercontent.com/hughxusu/lesson-web/develop/res/others/bg.png" alt="">
        <button>
            关闭定时器
        </button>
    </div>
</body>
<script>
    let counter = 4
    let timer = setInterval(() => {
        let p = document.querySelector('p')
        p.innerHTML = `${counter--}秒后关闭标签`
    }, 1000)

    let timerOut = setTimeout(() => {
        document.querySelector('div').remove()
        clearInterval(timer)
    }, 5000)

    let btn = document.querySelector('button')
    btn.addEventListener('click', () => {
        clearTimeout(timerOut)
        clearInterval(timer)
    })
</script>
```

两种定时器对比

* `setInterval`的特征是重复执行，首次执行会延时；`clearInterval`清除由`setInterval`设置的定时器。
* `setTimeout`的特征是延时执行，只执行1次；`clearTimeout`清除由`setTimeout`设置的定时器

## JS执行机制

由于Javascript的主要任务是，处理页面中用户的交互，所以Javascript语言单线程编程语言。

* 同一个时间只能完成一个任务。
* 用户操作不能同时进行，如：对某个`DOM`元素进行添加和删除操作，应该先进行添加，之后再删除。
* 所有任务需要排队，前一个任务结束，才会执行后一个任务。

> [!warning]
>
> 单线程的问题： 如果 JS 执行的时间过长，这样就会造成页面的渲染不连贯，导致页面渲染加载阻塞的感觉。

为了解决这个问题，利用多核 CPU 的计算能力，HTML5提出Web Worker标准，允许JavaScript脚本创建多个线程，出现了同步和异步。

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
2. 异步任务放入任务队列中。
3. 一旦执行栈中的所有同步任务执行完毕，系统就会按次序读取任务队列中的异步任务，于是被读取的异步任务结束等待状态，进入执行栈，开始执行。

<img src="https://raw.githubusercontent.com/hughxusu/lesson-web/develop/images/d-js/1633593121990_图片1.png" style="zoom:75%;" />

由于主线程不断的重复获得任务、执行任务、再获取任务、再执行，所以这种机制被称为事件循环（event loop）。

```html
<body>
    <div class="root"></div>
</body>
<script>
    function newH1(text) {
        let h1 = document.createElement('h1');
        h1.innerText = text;
        return h1;
    }

    let root = document.querySelector('.root');
    root.appendChild(newH1('1'));

    document.addEventListener('click', function () {
        root.appendChild(newH1('2'));
    });

    setTimeout(function () {
        root.appendChild(newH1('3'));
    }, 2000);

    root.appendChild(newH1('4'));
</script>
```

> [!warning]
>
> 对于耗时操作，需要主动放到异步任务中去。

## 页面导航

### location对象

`location`对象拆分并保存了URL地址的各个组成部分。

#### 获取地址

`href`属性获取完整的URL地址，对其赋值时用于地址的跳转。

```html
<body>
    <a href="https://www.baidu.com/">支付成功，<span>5</span> 秒之后跳转首页</a>
</body>
<script>
  	console.log(location.href)
  
    let a = document.querySelector('a')
    let num = 4
    let timer = setInterval(function () {
        a.innerHTML = `支付成功，<span>${num--}</span> 秒之后跳转首页`
        if (num === 0) {
            clearInterval(timer)
            location.href = 'https://www.baidu.com/'
        }
    }, 1000)
</script>
```

#### 解析参数

`search`属性获取地址中携带的参数，符号?后面部分。

1. 提交表单到特点地址。

```html
<body>
    <form action="e-2-target.html">
        <input type="text" name="username">
        <button>提交</button>
    </form>
</body>
```

2. 在跳转页查看参数

```js
document.write(`<h1>${location.search}</h1>`)
```

#### 哈希值

`hash`属性获取地址中的啥希值，符号#后面部分。

```html
<body>
    <ul>
        <li>
            <a href="#/contact">联系我们</a>
        </li>
        <li>
            <a href="#/shop">商场</a>
        </li>
        <li>
            <a href="#/collection">合集</a>
        </li>
        <li>
            <a href="#/project">项目</a>
        </li>
    </ul>
    <h1 class="content">
    </h1>
</body>
<script>
    let content = document.querySelector('.content')
    let ul = document.querySelector('ul')
    ul.addEventListener('click', (e) => {
        if (e.target.tagName === 'A') {
            content.innerHTML = e.target.innerHTML
        }
    })
</script>
```

> [!warning]
>
> 该方法常用于不刷新页面，显示页面的不同部分，在Vue等框架中使用。

#### 刷新页面

`reload`方法用来刷新当前页面，传入参数`true`时表示强制刷新。

> [!warning]
>
> 强制刷新会清空缓存，重新从网站下载数据。

```html
<body>
    <div class="timer">
        <h1 id="seconds">10</h1>
        <button id="refresh">刷新</button>
    </div>
</body>
<script>
    let seconds = document.querySelector('#seconds');
    let startButton = document.querySelector('#refresh');
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
<body>
    <h1></h1>
</body>
<script>
    let h1 = document.querySelector('h1')
    h1.innerText = navigator.userAgent
</script>
```

### histroy对象

`history`对象与浏览器地址栏操作相对应，如前进、后退、历史记录等。

* `back()`后退
* `forward()`前进

```html
<body>
    <div>
        <button class="forward">前进</button>
        <button class="back">后退</button>
    </div>
</body>
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

* `go(param)`跳转功能，`param`为跳转页数，1向前跳转一页，-1向后跳转一页。

```js
let forward = document.querySelector('.forward')
let back = document.querySelector('.back')
forward.addEventListener('click', function () {
    history.go(1)
})
back.addEventListener('click', function () {
    history.go(-1)
})
```

## 本地存储

随着互联网的快速发展，基于网页的应用越来越复杂，为了满足各种需求，经常会在本地存储大量的数据，HTML5规范提出了相关解决方案。

* 数据存储在用户浏览器中。
* 设置、读取方便、甚至页面刷新不丢失数据
* 容量较大，`sessionStorage`和`localStorage`约5M左右

### localStorage

`localStorage`的特点：

1. 生命周期永久生效，除非手动删除否则关闭页面也会存在。
2. 可以多页面共享数据。
3. 以键值对的形式存储使用。

`localStorage`的使用

* 存储数据：`localStorage.setItem(key, value)`，通过`key`（数据名字），保存`value`（数据值）。

```html
<body>
    <input value="山中无历日，寒尽不知年">
    <button>save</button>
</body>
<script>
    let btn = document.querySelector('button')
    let input = document.querySelector('input')
    btn.addEventListener('click', function () {
        let msg = input.value
        if (!msg) return
        localStorage.setItem('msg', msg)
        h1.innerHTML = msg
    })
</script>
```

* 获取数据：`localStorage.getItem(key)`，通过`key`获取数据。

```html
<body>
    <h1></h1>
</body>
<script>
    let h1 = document.querySelector('h1')
    let msg = localStorage.getItem('msg')
    if (msg) {
        h1.innerHTML = msg
    }
</script>
```

* 删除数据：`localStorage.removeItem(key)`，通过`key`删除数据。

```html
<body>
    <button>删除</button>
</body>
<script>
    let btn = document.querySelector('button')
    btn.addEventListener('click', function () {
        localStorage.removeItem('msg')
    })
</script>
```

### 引用数据类型的存储

本地只能存储字符串，无法存储复杂数据类型。需要将复杂数据类型转换成JSON字符串，在存储到本地。

* `JSON.stringify(obj)`对象转换成json字符串，`obj`对象数据。
* `JSON.parse(str)`将json字符串转换成数据，`str`字符串数据。

```html
<body>
    <button id="save">保存</button>
    <button id="load">读取</button>
    <h1></h1>
</body>
<script>
    let user = {
        name: '李白',
        age: 61,
        isMale: true
    }
    JSON.stringify(user)

    let save = document.querySelector('#save')
    save.addEventListener('click', function () {
        localStorage.setItem('user', JSON.stringify(user))
    })

    let load = document.querySelector('#load')
    let h1 = document.querySelector('h1')
    load.addEventListener('click', function () {
        let user = JSON.parse(localStorage.getItem('user'))
        if (!user) return
        console.log(user)
        h1.innerText = `姓名： ${user.name}, 年龄： ${user.age}, 性别： ${user.isMale ? '男' : '女'}`
    })
</script>
```

## 综合案例

将任务清单保存到`localStorage`，通过读取`localStorage`数据刷新清单界面。

```js
```



