# 事件对象

事件对象也是个对象，这个对象里有事件触发时的相关信息。

在事件绑定的回调函数的第一个参数就是事件对象，一般命名为event、ev、e。

```js
let btn = document.querySelector('button')
btn.addEventListener('mouseenter', function (e) {
  console.log(e)
})
```

## 事件对象的属性

常见属性

* type 获取当前的事件类型
* clientX/clientY获取光标相对于浏览器可见窗口左上角的位置
* offsetX/offsetY获取光标相对于当前DOM元素左上角的位置
* key用户按下的键盘键的值现在不提倡使用keyCode

```html
<style>
  body {
    height: 3000px;
  }

  div {
    width: 300px;
    height: 300px;
    background-color: pink;
    margin: 100px;
  }
</style>
<div>
</div>
<script>
  document.addEventListener('click', function (e) {
    console.log('clientX:' + e.clientX, 'clientY:' + e.clientY)
    console.log('pageX:' + e.pageX, 'pageY:' + e.pageY)
    console.log('offsetX:' + e.offsetX, 'offsetY:' + e.offsetY)
  })
</script>
```

### 案例

图片跟随鼠标移动

```html
<style>
  img {
    position: absolute;
    top: 0;
    left: 0;
  }
</style>
<img src="./images/tianshi.gif" alt="">
<script>
  let img = document.querySelector('img')
  document.addEventListener('mousemove', function (e) {
    img.style.left = e.pageX - 50 + 'px'
    img.style.top = e.pageY - 40 + 'px'
  })
</script>
```

## 事件流

事件流指的是事件完整执行过程中的流动路径，假设页面里有个div，当触发事件时，会经历两个阶段，分别是捕获阶段、冒泡阶段。

<img src="https://s1.ax1x.com/2023/05/29/p9Xt8JK.png" style="zoom:50%;" />

事件冒泡概念：当一个元素的事件被触发时，同样的事件将会在该元素的所有祖先元素中依次被触发。这一过程被称为事件冒泡。

```html
<style>
    .father {
        margin: 100px auto;
        width: 500px;
        height: 500px;
        background-color: pink;
    }

    .son {

        width: 200px;
        height: 200px;
        background-color: purple;
    }
</style>
<div class="father">
    <div class="son"></div>
</div>
<script>
    let fa = document.querySelector('.father')
    let son = document.querySelector('.son')
    fa.addEventListener('click', function () {
        alert('我是爸爸')
    })
    son.addEventListener('click', function () {
        alert('我是儿子')
    })
    document.addEventListener('click', function () {
        alert('我是爷爷')
    })
</script>
```

是否使用事件捕获机制

`addEventListener` 第三个参数为 `true` 代表捕获阶段触发。

### 阻止事件冒泡

因为默认就有冒泡模式的存在，所以容易导致事件影响到父级元素。若想把事件就限制在当前元素内，就需要阻止事件流动。阻止事件流动需要拿到事件对象。

`e.stopPropagation()`

鼠标经过事件：

* mouseover 和 mouseout 会有冒泡效果
* mouseenter  和 mouseleave   没有冒泡效果

```html
<style>
    .father {
        margin: 100px auto;
        width: 500px;
        height: 500px;
        background-color: pink;
    }

    .son {

        width: 200px;
        height: 200px;
        background-color: purple;
    }
</style>
<div class="father">
    <div class="son"></div>
</div>
<script>
    let fa = document.querySelector('.father')
    let son = document.querySelector('.son')
    fa.addEventListener('mouseenter', function () {
        console.log(111)
    })
</script>
```

### 阻止默认行为

阻止标签的默认行为

`e.preventDefault()`

```html
<a href="http://www.baidu.com">跳转到百度</a>
<script>
    let a = document.querySelector('a')
    a.addEventListener('click', function (e) {
        e.preventDefault()
    })
</script>
```

### 事件注册的区别

传统on注册

* 同一个对象，后面注册的事件会覆盖前面注册(同一个事件)
* 直接使用null覆盖偶就可以实现事件的解绑
* 都是冒泡阶段执行的

事件监听注册（L2）语法

* addEventListener
* 后面注册的事件不会覆盖前面注册的事件（同一个事件）
* 可以通过第三个参数去确定是在冒泡或者捕获阶段执行
* 必须使用removeEventListener
* 匿名函数无法被解绑

```html
<button>点击</button>
<script>
    let btn = document.querySelector('button')
    btn.onclick = function () {
        alert('第一次')
    }
    btn.onclick = function () {
        alert('第二次')
    }
    btn.onclick = null
  	
    btn.addEventListener('click', add)
    function add() {
        alert('第一次')
        btn.removeEventListener('click', add)
    }
</script>
```

## 事件委托

事件委托其实是利用事件冒泡的特点， 给父元素添加事件，子元素可以触发。

```html
<ul>
    <li>我是第1个小li</li>
    <li>我是第2个小li</li>
    <li>我是第3个小li</li>
    <li>我是第4个小li</li>
    <li>我是第5个小li</li>
</ul>
<script>
    let ul = document.querySelector('ul')
    ul.addEventListener('click', function (e) {
        e.target.style.color = 'red'
    })
</script>
```

## 表格案例

```js
let arr = [
  { stuId: 1001, uname: '欧阳霸天', age: 19, gender: '男', salary: '20000', city: '上海' },
  { stuId: 1002, uname: '令狐霸天', age: 29, gender: '男', salary: '30000', city: '北京' },
  { stuId: 1003, uname: '诸葛霸天', age: 39, gender: '男', salary: '2000', city: '北京' },
]

let tbody = document.querySelector('tbody')
let add = document.querySelector('.add')
let uname = document.querySelector('.uname')
let age = document.querySelector('.age')
let gender = document.querySelector('.gender')
let salary = document.querySelector('.salary')
let city = document.querySelector('.city')

function render() {
  tbody.innerHTML = ''
  for (let i = 0; i < arr.length; i++) {
    // 1.创建tr  
    let tr = document.createElement('tr')
    // 2.tr 里面放内容
    tr.innerHTML = `
    <td>${arr[i].stuId}</td>
    <td>${arr[i].uname}</td>
    <td>${arr[i].age}</td>
    <td>${arr[i].gender}</td>
    <td>${arr[i].salary}</td>
    <td>${arr[i].city}</td>
    <td>
      <a href="javascript:" id="${i}">删除</a>
    </td>
    `
    tbody.appendChild(tr)
  }
}
render()

add.addEventListener('click', function () {
  arr.push({
    stuId: arr[arr.length - 1].stuId + 1,
    uname: uname.value,
    age: age.value,
    gender: gender.value,
    salary: salary.value,
    city: city.value
  })
  
  render()
  uname.value = age.value = salary.value = ''
  gender.value = '男'
  city.value = '北京'
})

tbody.addEventListener('click', function (e) {
  if (e.target.tagName === 'A') {
    arr.splice(e.target.id, 1)
    render()
  }
})
```

手风琴特效

```js
let lis = document.querySelectorAll('li')
for (let i = 0; i < lis.length; i++) {
  lis[i].addEventListener('mouseenter', function () {
    for (let j = 0; j < lis.length; j++) {
      lis[j].style.width = '100px'
    }
    this.style.width = '800px'
  })
  lis[i].addEventListener('mouseleave', function () {
    for (let j = 0; j < lis.length; j++) {
      lis[j].style.width = '240px'
    }
  })
}
```



