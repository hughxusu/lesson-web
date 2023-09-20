# 浮动与定位

网页常见布局方式：

1. 文档流
2. 浮动
3. 定位

## 文档流

文档流处在网页的最底层，它表示的是一个页面中的位置，元素默认都处在文档流中。

## 浮动

块元素在文档流中默认垂直排列，如果希望块元素在页面中水平排列，可以为元素设置浮动（float属性是一个非none的值），元素浮动后，会立即脱离文档流。

* 元素脱离文档流以后，它下边的元素会立即向上移动。
* 元素浮动以后，会尽量向页面的左上或右上漂浮，直到遇到父元素的边框或者其他的浮动元素。
* 如果浮动元素上边是一个没有浮动的块元素，则浮动元素不会超过块元素。
* 浮动的元素不会超过他上边的兄弟元素，最多最多一边齐。

```html
<style>
  div {
    width: 100px;
    height: 100px;
  }

  .one {
    background-color: pink;
    float: left;
  }

  .two {
    background-color: skyblue;
    float: right;
  }
</style>
<div class="one">one</div>
<div class="two">two</div>
```

## 清除浮动

### 浮动的影响

子元素浮动了，此时子元素不能撑开标准流的块级父元素

```html
<style>
  .bottom {
    height: 100px;
    background-color: blue;
  }

  .left {
    float: left;
    width: 200px;
    height: 200px;
    background-color: red;
  }
</style>

<div class="top">
  <div class="left"></div>
</div>
<div class="bottom"></div>
```

清除浮动影响

1. 添加额外标签

```html
<style>
  .bottom {
    height: 100px;
    background-color: blue;
  }

  .left {
    float: left;
    width: 200px;
    height: 200px;
    background-color: red;
  }

  .clearfix {
    clear: both;
  }
</style>
<div class="top">
  <div class="left"></div>
  <div class="clearfix"></div>
</div>
<div class="bottom"></div>
```

2. 使用伪元素

```html
<style>
  .bottom {
    height: 100px;
    background-color: blue;
  }

  .left {
    float: left;
    width: 200px;
    height: 200px;
    background-color: red;
  }

  .clearfix::after {
    content: '';
    clear: both;
    display: block;

    height: 0;
    visibility: hidden;
  }
</style>

<div class="top clearfix">
  <div class="left"></div>
</div>
<div class="bottom"></div>
```

3. 清除浮动和高度塌陷的通用方法

```html
<style>
  .bottom {
    height: 100px;
    background-color: blue;
  }

  .left {
    float: left;
    width: 200px;
    height: 200px;
    background-color: red;
  }

  .clearfix::before,
  .clearfix::after {
    content: '';
    display: table;
    clear: both;
  }

  .clearfix{
    zoom: 1;
  }
</style>

<div class="top clearfix">
  <div class="left"></div>
</div>
<div class="bottom"></div>
```

## 综合案例

![](https://s1.ax1x.com/2023/03/21/ppUXys1.png)

## 定位

可以让元素自由的摆放在网页的任意位置，一般用于盒子之间的层叠情况。

设置定位属性 `position` ，属性值：

1. `static` 静态定位，默认值。
2. `relative` 相对定位。
3. `absolute` 绝对定位。
4. `fixed` 固定位置

设置偏移量属性值：`left` / `right` / `top` / `bottom` 

### 相对定位

相对于自己之前的位置进行移动

1. 占有原来的位置，没有脱离文档流。
2. 仍然具体标签原有的显示模式特点
3. 改变位置参照自己原来的位置

```html
<style>
  * {
    margin: 0;
    padding: 0;
  }

  .box {
    position: relative;
    left: 100px;
    top: 200px;
    width: 200px;
    height: 200px;
    background-color: pink;
  }
</style>
<div class="box">box</div>
```

### 绝对定位

相对于非静态定位的父元素进行定位移动

1. 如果父级没有设置，默认相对于浏览器可视区域进行移动。
2. 脱离文档流。

```html
<style>
  * {
    margin: 0;
    padding: 0;
  }

  .father {
    position: relative;
    width: 400px;
    height: 400px;
    background-color: pink;
  }

  .son {
    margin-left: 20px;
    width: 300px;
    height: 300px;
    background-color: skyblue;
  }

  .one {
    position: absolute;
    width: 200px;
    height: 200px;
    background-color: green;
    top: 0;
    left: 0;
  }
</style>

<div class="father">
  <div class="son">
    <div class="one"></div>
  </div>
</div>
```

使用绝对定位实现居中效果

```html
<style>
  .box {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    width: 400px;
    height: 300px;
    background-color: pink;
  }
</style>
<div class="box"></div>
```

### 固定位置

相对于浏览器进行定位移动

1. 相对于浏览器可视区域进行移动。
2. 脱离文档流。

```html
<style>
  .box {
    position: fixed;
    left: 0;
    top: 0;
    width: 200px;
    height: 200px;
    background-color: pink;
  }
</style>
<div class="box">box</div>
```

### 元素的层级关系

不同布局方式元素的层级关系：标准流 < 浮动 < 定位

不同定位之间的层级关系：

1. 相对、绝对、固定默认层级相同。
2. HTML中写在下面的元素层级更高，会覆盖上面的元素

更改定位元素的层级

```html
<style>
  div {

    width: 200px;
    height: 200px;
  }

  .one {
    position: absolute;
    left: 20px;
    top: 50px;

    z-index: 1;

    background-color: pink;
  }

  .two {
    position: absolute;
    left: 50px;
    top: 100px;

    background-color: skyblue;
  }
</style>

<div class="one">one</div>
<div class="two">two</div>
```

