# 选择器进阶

## 复合选择器

* 后代选择器
* 子代选择器
* 并集选择器
* 交集选择器

### 后代选择器

根据 HTML 标签的嵌套关系，选择父元素后代中满足条件的元素。

```html
<style>
  div p {
    color: red;
  }
</style>

<p>这是一个p标签</p>
<div>
  <p>这是div的后代P</p>
</div>
```

> [!warning]
>
> 后代选择器包括：儿子、孙子、重孙子......

### 子代选择器

根据 HTML 标签的嵌套关系，选择父元素子代中满足条件的元素。

```html
<style>
  div>a {
    color: red;
  }
</style>
<div>
  父级
  <a href="#">这是div里面的a</a>
  <p>
    <a href="#">这是div里面的p里面的a</a>
  </p>
</div>
```

### 并集选择器

同时选择多组标签，设置相同的样式

```html
<style>
  p, div, span, h1 {
    color: red;
  }
</style>
<body>
  <p>ppp</p>
  <div>div</div>
  <span>span</span>
  <h1>h1</h1>
  <h2>h2</h2>
</body>
```

### 交集选择器

选中页面中同时满足多个选择器的标签

```html
<style>
  p.box {
    color: red;
  }
  .box.next{
    color: green;
  }
</style>
<body>
  <p class="box">这是p标签:box</p>
  <p>单独p标签</p>
  <div class="box">这是div标签:box</div>
  <p class="box next">包含了box和next类的p标签</p>
</body>
```

## 伪类与伪元素选择器

### 伪类选择器

1. `a:link {}` 未访问过的状态
2. `a:visited {}` 访问过的状态
3. `a:hover {}` 悬浮状态
4. `a:active {}` 鼠标按下的状态

```html
<style>
  a:hover {
    color: red;
    background-color: green;
  }
  div:hover {
    color: green;
  }
</style>
<body>
  <a href="#">这是超链接</a>
  <div>这是一个div</div>
</body>
```

5. `input:focus` 用于选中元素获取焦点时状态，常用于表单控件

```html
<style>
  input:focus {
    background-color: pink;
  }
</style>
<input type="text">
<input type="password">
<input type="button">
```

### 属性选择器

通过元素上的HTML属性来选择元素，常用于选择 input 标签

```html
<style>
  input[type='text']  {
    background-color: pink;
  }

  input[type="password"] {
    background-color: skyblue;
  }
</style>

<input type="text">
<input type="password">
```

### 结构伪类选择器

根据元素在HTML中的结构关系查找元素

| 选择器               | 说明                  |
| -------------------- | --------------------- |
| `:first-child`       | 选择第一个子元素      |
| `:last-child`        | 选择最后一个子元素    |
| `:nth-child(n)`      | 选择第 n 个子元素     |
| `:nth-last-child(n)` | 选择倒数第 n 个子元素 |

n 的可选值：

1. 整数：1、2、3 ……
2. 偶数：2n、even
3. 奇数：2n + 1、2n-1、odd
4. 找到前 n 个：-n + 5
5. 找到第 n 个以后：n + 5

```html
<style>
  li:first-child {
  	background-color: green;
  }
  
  li:last-child {
  	background-color: green;
  }
  
  li:nth-child(5) {
  	background-color: green;
  }
  
  li:nth-last-child(n+1) {
    background-color: blue;
  }
</style>
<ul>
  <li>这是第1个li</li>
  <li>这是第2个li</li>
  <li>这是第3个li</li>
  <li>这是第4个li</li>
  <li>这是第5个li</li>
  <li>这是第6个li</li>
  <li>这是第7个li</li>
  <li>这是第8个li</li>
</ul>
```

> [!tip]
>
> 自己了解选择器 `nth-of-type` 的使用。

### 伪元素

一般页面中的非主体内容可以使用伪元素：

* `::before` 在最前面加一个伪元素。
* `::after` 在最后面加一个伪元素。

```html
<style>
  .father {
    width: 300px;
    height: 300px;
    background-color: pink;
  }

  .father::before {
    content: '北方';
  }

  .father::after {
    content: '大学';
  }
</style>
<div class="father">工业</div>
```

> [!warning]
>
> 1. 必须设置content属性才能生效。
> 2. 伪元素默认是行内元素。

## Emmet语法

[emmet快速语法](https://code.z01.com/emmet/)：通过简写语法，快速生成代码。

