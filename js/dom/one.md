# Web API 基本知识

使用 JS 去操作html和浏览器

分类：DOM（文档对象模型）、BOM（浏览器对象模型）

## DOM

DOM（Document Object Model——文档对象模型）是用来呈现以及与任意 HTML 或 XML 文档交互的API。

DOM作用是开发网页内容特效和实现用户交互。

DOM树：将 HTML 文档以树状结构直观的表现出来，称之为文档树或 DOM 树，文档树直观的体现了标签与标签之间的关系。

![](https://www.zachgollwitzer.com/_next/image?url=https%3A%2F%2Fmedia.zachgollwitzer.com%2Fdom-tree-visual.jpg&w=1920&q=75)

DOM对象：浏览器根据html标签生成的 JS对象。

* 浏览器根据html标签生成的 JS对象
* 修改这个对象的属性会自动映射到标签身上

DOM对象使得网页节点当做对象来处理。

`document` 对象，是 DOM 里提供的一个对象，它提供的属性和方法都是用来访问和操作网页内容，网页所有内容都在 `document` 里面。

### 获取 DOM 对象

` document.querySelector('')` 选择匹配的第一个元素。

参数：包含一个或多个有效的CSS选择器字符串。

返回值：CSS选择器匹配的第一个元素,一个 HTMLElement对象，如果没有匹配到，则返回null。

```html
<button>点击</button>
<script>
  let btn = document.querySelector('button')
  console.dir(btn)
  btn.innerHTML = '提交'
</script>
```

` document.querySelectorAll('')` 选择匹配的第一个元素。

参数：包含一个或多个有效的CSS选择器字符串。

返回值：得到的是一个伪数组（有长度有索引号的数组，但是没有 `pop()` `push()` 等数组方法）。

其它获取 DOM 元素方法

`document.getElementById()` 根据 id 获取一个元素。

`document.getElementsByTagName()` 根据标签获取一类元素。

`document.getElementsByClassName()` 根据类元素获取页面元素。

使用不同的选择器获取元素

```html
<div>盒子1</div>
<div>盒子2</div>
<div class="three">盒子3</div>
<ul>
  <li>1</li>
  <li>2</li>
  <li>3</li>
</ul>
<span>就1个</span>
<script> 
  let div = document.querySelector('div')
  let div = document.querySelector('.three')
  console.log(div)

  let li = document.querySelector('ul li:last-child')
  console.log(li)
 
  let lis = document.querySelectorAll('ul li')
  console.log(lis) 
  for (let i = 0; i < lis.length; i++) {
    console.log(lis[i])
  }
  
  let span = document.querySelectorAll('span')
  console.log(span)
</script>
```

### 修改元素内容

`document.write()`  只能将文本内容追加到前面的位置。

`innerText` ：将文本内容添加/更新到任意标签位置，文本中包含的标签不会被解析。

`innerHTML`：将文本内容添加/更新到任意标签位置，文本中包含的标签会被解析。

```html
<div>
  hello, world
</div>
<script>
  let box = document.querySelector('div')
  box.innerText = 'hello, html'
  box.innerHTML = '<strong>hello, js</strong>'
</script>
```

### 修改元素属性

还可以通过 JS 设置/修改标签元素属性，比如通过 `src` 更换图片，最常见的属性比如：`href`、`title`、`src` 等。

```html
<img src="./images/1.webp" alt="">
<script>
  let pic = document.querySelector('img')
  pic.src = './images/3.webp'
  pic.title = '标签'
</script>
```

### 修改元素样式

#### 通过 `style` 修改元素样式

```html
<style>
  div {
    width: 300px;
    height: 300px;
    background-color: blue;
  }
</style>

<div></div>
<script>
  let box = document.querySelector('div')
  box.style.backgroundColor = 'skyblue'
  box.style.width = '400px'
  box.style.marginTop = '100px'
  document.getElementsByClassName()
</script>
```

#### 通过 `className` 修改元素样式

```html
<style>
  div {
    width: 200px;
    height: 200px;
    background-color: pink;
  }

  .active {
    width: 300px;
    height: 300px;
    background-color: hotpink;
    margin-left: 100px;
  }
</style>
<div class="one"></div>
<script>
  let box = document.querySelector('div')
  box.className = 'one active'
</script>
```

#### 通过 `classList` 修改元素样式

为了解决className 容易覆盖以前的类名，可以通过 `classList` 方式追加和删除类名。

```html
<style>
  div {
    width: 200px;
    height: 200px;
    background-color: pink;
  }

  .active {
    width: 300px;
    height: 300px;
    background-color: hotpink;
    margin-left: 100px;
  }
</style>
<div class="one"></div>
<script>
  let box = document.querySelector('div')
  box.classList.add('active')
  box.classList.remove('one')
  box.classList.toggle('one')
</script>
```

### 修改表单属性

表单很多情况，也需要修改属性，正常的有属性有取值，跟其他的标签属性没有任何区别。

```html
<input type="text" value="请输入">
<button disabled>按钮</button>
<input type="checkbox" name="" id="" class="agree">
<script>
  let input = document.querySelector('input')
  input.value = 'hello, world'
  input.type = 'password'

  let btn = document.querySelector('button')
  btn.disabled = false
  
  let checkbox = document.querySelector('.agree')
  checkbox.checked = true
</script>
```

## 定时器

`setInterval` 每隔一段时间调用这个函数。

```html
<script>
  setInterval(function () {
    console.log('高薪就业')
  }, 1000)

  function show() {
    console.log('月薪过2万')
  }
  let timer = setInterval(show, 1000)
  clearInterval(timer)
</script>
```

### 确认用户协议

```html
<textarea name="" id="" cols="30" rows="10">
  用户注册协议
  欢迎注册成为京东用户！
</textarea>
<br>
<button class="btn" disabled>我已经阅读用户协议(6)</button>
<script>
  let btn = document.querySelector('.btn')
  let i = 6
  let timer = setInterval(function () {
    i--
    btn.innerHTML = `我已经阅读用户协议(${i})`
    if (i === 0) {
      clearInterval(timer)
      btn.disabled = false
      btn.innerHTML = '我同意该协议'
    }
  }, 1000)
</script>
```

