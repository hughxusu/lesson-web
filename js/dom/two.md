# DOM 事件基础

事件：在编程时系统内发生的动作或命令。

事件监听：就是让程序检测是否有事件产生，一旦有事件触发，就立即调用一个函数做出响应，也称为注册事件

```js
let btn = document.querySelector('button')
btn.addEventListener('click', function () {
  alert('hello, world!')
})
```

> [!note]
>
> 事件监听三要素：
>
> 1. 事件源：绑定事件的dom元素。
> 2. 事件类型：方式触发事件的方式，如：鼠标单击click等。
> 3. 事件调用的函数：触发事件后的任务。

## 事件监听

### 关闭二维码

```js
let close_btn = document.querySelector('.close_btn')
let erweima = document.querySelector('.erweima')
close_btn.addEventListener('click', function () {
  erweima.style.display = 'none'
})
```

### 五连抽

```js

let arr = ['温迪', '琴', '钟离', '刻晴', '芭芭拉']
function getRandom(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min
}

let start = document.querySelector('.start')
let end = document.querySelector('.end')
let qs = document.querySelector('.qs')

let timer = 0
let random = 0

start.addEventListener('click', function () {
  timer = setInterval(function () {
    random = getRandom(0, arr.length - 1)
    qs.innerHTML = arr[random]
  }, 25)
  start.disabled = true

  if (arr.length === 1) {
    start.disabled = true
    end.disabled = true
  }
})

end.addEventListener('click', function () {
  clearInterval(timer)
  arr.splice(random, 1)
  start.disabled = false
})
```

## 事件类型

| 分类     | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| 鼠标事件 | `click`  鼠标点击 <br>`mouseenter` 鼠标进入<br>`mouseleave` 鼠标离开 |
| 焦点事件 | `focus` 获得焦点 <br>`blur ` 失去焦点                        |
| 键盘事件 | `Keydown` 键盘按下触发 <br>`Keyup` 键盘抬起触发              |
| 文本事件 | `input` 用户输入事件                                         |

### 搜索框

```js
let search = document.querySelector('input')
let list = document.querySelector('.result-list')
search.addEventListener('focus', function () {
  list.style.display = 'block'
  this.classList.add('search')
})
search.addEventListener('blur', function () {
  list.style.display = 'none'
  this.classList.remove('search')
})
```

### 输入框

```js
let area = document.querySelector('#area')
let useCount = document.querySelector('.useCount')
area.addEventListener('input', function () {
  useCount.innerHTML = area.value.length
})
```

### 全选和反选

```js
```

