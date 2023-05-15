# 节点操作

![](https://www.zachgollwitzer.com/_next/image?url=https%3A%2F%2Fmedia.zachgollwitzer.com%2Fdom-tree-visual.jpg&w=1920&q=75)

DOM节点：DOM树里每一个内容都称之为节点

节点类型

* 元素节点：所有的标签比如 body、 div；html 是根节点
* 属性节点：所有的属性 比如 href
* 文本节点：所有的文本
* 其他

## 查找节点

### 查找父节点

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

### 查找子节点

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

### 查找兄弟节点

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

