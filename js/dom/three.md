# 节点操作

![](https://www.zachgollwitzer.com/_next/image?url=https%3A%2F%2Fmedia.zachgollwitzer.com%2Fdom-tree-visual.jpg&w=1920&q=75)

DOM节点：DOM树里每一个内容都称之为节点

节点类型

* 元素节点：所有的标签比如 body、 div；html 是根节点
* 属性节点：所有的属性 比如 href
* 文本节点：所有的文本
* 其他

## 节点操作

### 查找节点

#### 查找父节点

`div.parentNode` 返回最近一级的父节点找不到返回为。

```html
<div class="father">
  <div class="son">儿子</div>
</div>
<script>
  let son = document.querySelector('.son')
  son.parentNode.style.display = 'none'
</script>
```

关闭多个二维码

```js
let close_btn = document.querySelectorAll('.close')
for (let i = 0; i < close_btn.length; i++) {
  close_btn[i].addEventListener('click', function () {
    this.parentNode.style.display = 'none'
  })
}
```

#### 查找子节点

* `div.childNodes` 获得所有子节点、包括文本节点(空格、换行)、注释节点等
* `div.children` 仅获得所有元素节点  、包括文本节点(空格、换行)、注释节点等

```html
<button>点击</button>
<ul>
  <li>1</li>
  <li>2</li>
  <li>3</li>
  <li>4</li>
  <li>5</li>
  <li>6</li>
</ul>
<script>
  let btn = document.querySelector('button')
  let ul = document.querySelector('ul')
  btn.addEventListener('click', function () {
    for (let i = 0; i < ul.children.length; i++) {
      ul.children[i].style.color = 'red'
    }
  })
  ul.children[0].style.color = 'green'
</script>
```

#### 查找兄弟节点

* `div.nextElementSibling` 返回下一个兄弟节点找不到返回为。
* `div.previousElementSibling` 返回上一个兄弟节点找不到返回为。

```html
<button>点击</button>
<ul>
  <li>第1个</li>
  <li class="two">第2个</li>
  <li>第3个</li>
  <li>第4个</li>
</ul>
<script>
  let btn = document.querySelector('button')
  let two = document.querySelector('.two')
  btn.addEventListener('click', function () {
    two.nextElementSibling.style.color = 'red'
    two.previousElementSibling.style.color = 'red'
  })
</script>
```

### 增加阶段

操作步骤：

1. 创建节点 `document.createElement(tag)` tag（标签名）
2. 添加节点
   * `div.appendChild(obj)` 追加节点 obj（标签对象）
   * `div.insertBefore(obj, aim)` 在 aim（标签对象）前添加，obj（标签对象）

```html
<ul>
    <li>第1个</li>
    <li>第2个</li>
</ul>
<script>
    let ul = document.querySelector('ul')
    let li = document.createElement('li')

    li.innerHTML = '新增标签'
    ul.appendChild(li)
    ul.insertBefore(li, ul.children[0])
</script>
```

### 克隆节点

`div.cloneNode(flag)` flag（true表示克隆后代节点，false不克隆后代节点）

```html
<ul>
    <li>我是内容11111</li>
</ul>
<script>
    let ul = document.querySelector('ul')
    let newUl = ul.cloneNode(true)
    document.body.appendChild(newUl)
</script>
```

学成在线渲染

```js
let ul = document.querySelector('ul')
for (let i = 0; i < data.length; i++) {
  let li = document.createElement('li'))
  li.innerHTML = `
    <img src=${data[i].src} alt="">
    <h4>
        ${data[i].title}
    </h4>
    <div class="info">
        <span>高级</span> • <span> ${data[i].num}</span>人在学习
    </div>
  `
  ul.appendChild(li)
}
```

### 删除节点

`div.removeChild(obj)` 删除节点 obj（标签对象）

```html
<button>点击</button>
<ul>
    <li>我是内容11111</li>
</ul>
<script>
  let btn = document.querySelector('button')
  let ul = document.querySelector('ul')
  btn.addEventListener('click', function () {
    ul.removeChild(ul.children[0])
  })
</script>
```

## 时间对象

使用 `new` 关键实例化一个对象。`Date()` 时间对象用于时间处理。

```js
let date = new Date() // 返回当前时间
let last = new Date('2023-1-1 18:30:00') // 返回指定的时间
```

时间对象常用函数

| 方法            | 作用               | 说明                 |
| --------------- | ------------------ | -------------------- |
| `getFullYear()` | 获得年份           | 获取四位年份         |
| `getMonth()`    | 获得月份           | 取值为 0 ~ 11        |
| `getDate()`     | 获取月份中的每一天 | 不同月份取值也不相同 |
| `getDay()`      | 获取星期           | 取值为 0 ~ 6         |
| `getHours()`    | 获取小时           | 取值为 0 ~ 23        |
| `getMinutes()`  | 获取分钟           | 取值为 0 ~ 59        |
| `getSeconds()`  | 获取秒             | 取值为 0 ~ 59        |

```html
<style>
    div {
        width: 400px;
        height: 50px;
        background-color: pink;
        text-align: center;
        line-height: 50px;
    }
</style>
<script>
    let arr = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六']
    let div = document.querySelector('div')
    getTime()
    setInterval(getTime, 1000)
    function getTime() {
        let date = new Date()
        let year = date.getFullYear()
        let month = date.getMonth() + 1
        let date1 = date.getDate()
        let hour = date.getHours()
        let min = date.getMinutes()
        let sec = date.getSeconds()
        let day = date.getDay()
        div.innerHTML = `今天是： ${year}年${month}月${date1}日 ${hour}:${min}:${sec} ${arr[day]}`
    }
</script>
```

### 时间戳

时间戳：是指1970年01月01日00时00分00秒起至现在的毫秒数，它是一种特殊的计量时间的方式。

创建时间戳的方法：

1. 使用 `date.getTime()` 获得时间戳。
2. `+new Date()` 
3. `Date.now() `

下课倒计时

计算天数 `d = parseInt(second/ 60/60 /24)`

计算小时 `h = parseInt(总秒数/ 60/60 %24)`

计算分数 `m = parseInt(总秒数 /60 % 60)`

计算当前秒数 `s = parseInt(second % 60);`

```js
let hour = document.querySelector('#hour')
let minutes = document.querySelector('#minutes')
let scond = document.querySelector('#scond')
time()
setInterval(time, 1000)
function time() {
  let now = +new Date()
  let last = +new Date('2021-8-29 18:30:00')
  let count = (last - now) / 1000
  let h = parseInt(count / 60 / 60 % 24)
  h = h < 10 ? '0' + h : h
  let m = parseInt(count / 60 % 60)
  m = m < 10 ? '0' + m : m
  let s = parseInt(count % 60);
  s = s < 10 ? '0' + s : s
  hour.innerHTML = h
  minutes.innerHTML = m
  scond.innerHTML = s
}
```



