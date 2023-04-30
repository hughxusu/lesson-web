# 背景与元素显示模式

## 背景属性

设置一个元素的背景，包括：

1. 背景颜色
2. 背景图片（设置背景图片的位置和平铺样式）

### 背景颜色

```html
<style>
  div {
    width: 400px;
    height: 400px;
    background-color: rgba(0, 0, 0, ;
  }
</style>
<div>div</div>
```

### 背景图片

```html
<style>
  div {
    width: 400px;
    height: 400px;
    background-color: pink;
    background-image: url(./images/bg.png);
  }
</style>
<div></div>
```

### 背景平铺

```html
<style>
  div {
    width: 400px;
    height: 400px;
    background-color: pink;
    background-image: url(./images/bg.png);
    background-repeat: no-repeat;
  }
</style>
<div></div>
```

`background-repeat` 取值：

1. `repeat` 水平和垂直方向都平铺。
2. `no-repeat` 不平铺。
3. `repeat-x` 沿着水平方向（x轴）平铺。
4. `repeat-y` 沿着垂直方向（y轴）平铺。

### 背景位置

```html
<style>
  div {
    width: 400px;
    height: 400px;
    background-color: pink;
    background-image: url(./images/bg.png);
    background-repeat: no-repeat;
    background-position: right bottom;
  }
</style>
<div></div>
```

`background-position` 取值：

1. `top`, `bottom`, `left`, `right`, `center`
2. 像素值，可以取负。

### 背景属性

```html
<style>
  div {
    width: 400px;
    height: 400px;
    background: pink url(./images/bg.png) 20px center no-repeat;
  }
</style>
<div></div>
```

## 元素显示模式

1. 块级元素
2. 行内元素
3. 行内块元素

### 块级元素

* 独占一行
* 宽度默认是父元素的宽度，高度默认由内容撑开
* 可以设置宽高
* 常见元素：div、p、h

### 行内元素

* 一行可以显示多个
* 宽度和高度默认由内容撑开
* 不可以设置宽高
* 常见元素：a、span 、b、u、i、s、strong、ins、em、del

### 行内块元素

* 一行可以显示多个
* 可以设置宽高
* 常见元素：input、textarea、button、select

### 修改显示模式

```html
<style>
  div {
    width: 300px;
    height: 300px;
    background-color: pink;
    display: inline;
  }
</style>
<div>div1</div>
<div>div1</div>
```

> [!warning]
>
> 1. p标签中不要嵌套div、p、h等块级元素
> 2. a标签内部可以嵌套任意元素，a标签不能嵌套a标签

## 案例

[Element Plus](https://element-plus.gitee.io/zh-CN/) 组件库

模拟其中链接的样式
