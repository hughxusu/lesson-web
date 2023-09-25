# 其它常用属性

## 装饰属性

### 垂直对齐方式

浏览器文字类型元素排版中存在用于对齐的基线

<img src="https://s1.ax1x.com/2023/02/26/pp9ZeQx.png" style="zoom:35%;" />

`vertical-align` 可以修改行内块元素垂直对齐属性：

* `baseline` 默认值，基线对齐
* `top` 顶部对象
* `middle` 中部对齐
* `bottom` 底部对齐

```html
<style>
  img {
    vertical-align: middle;
  }
</style>
<img src="../images/1.jpg" alt=""><input type="text">
```

### 光标类型

设置鼠标光标在元素上时显示的样式，`cursor` 属性：

* `default` 默认值，通常是箭头。
* `pointer` 点击效果。
* `text` 输入框效果
* `move` 拖动效果

```html
<style>
  div {
    width: 200px;
    height: 200px;
    background-color: pink;
    cursor: text;
  }
</style>
<div>div</div>
```

### 边框圆角

给盒子增加圆角 `border-radius` ，取值为像素和百分比

```html
<style>
  .box {
    margin: 50px auto;
    width: 200px;
    height: 200px;
    background-color: pink;
    border-radius: 10px;
    /* border-radius: 10px 20px 40px 80px; */
    /* border-radius: 10px 40px 80px; */
    /* border-radius: 10px 80px; */
  }
</style>

<div class="box"></div>
```

特殊样式

1. 设置为圆形：盒子必须是正方形 `border-radius:50%`
2. 胶囊形：盒子要求是长方形 `border-radius` 盒子高度的一半

```html
<style>
  /* 圆形 */
  .one {
    width: 200px;
    height: 200px;
    background-color: pink;
    border-radius: 50%;
  }

  /* 胶囊形 */
  .two {
    width: 400px;
    height: 200px;
    background-color: skyblue;
    border-radius: 100px;
  }
</style>
<div class="one"></div>
<div class="two"></div>
```

### 溢出属性

溢出是指盒子内容部分超出盒子范围的区域。

`overflow` 设置溢出部分显示效果。

* `visible` 默认值，溢出部分可见。
* `hidden` 溢出部分隐藏。
* `scroll` 无论是否溢出都显示滚动条。
* `auto` 根据溢出，显示或隐藏滚动条。

```html
<style>
  div {
    width: 200px;
    height: 200px;
    background-color: pink;
    overflow: hidden;
  }
</style>
```

### 元素隐藏

让元素本身在屏幕中不可见。

1. `visibility: hidden ` 隐藏元素本身，保留元素位置。
2. `display:none` 元素消失。

```html
<style>
  div {
    width: 200px;
    height: 200px;
  }

  .one {
    visibility: hidden;
    /* display: none; */
    background-color: pink;
  }

  .two {
    background-color: green;
  }
</style>
<div class="one">one</div>
<div class="two">two</div>
```

### 元素透明度

```html
<style>
  div {
    width: 400px;
    height: 400px;
    background-color: green;
    opacity: 0.5;
  }
</style>
<div>在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。</div>
```

### 边框合并

让相邻表格边框进行合并，得到细线边框效果。 

属性 `border-collapse: collapse;`

```html
<style>
  table {
    border: 1px solid #000;
    border-collapse: collapse;
  }

  th,
  td {
    border: 1px solid #000;
  }
</style>
<table>
  <thead>
    <tr>
      <th>姓名</th>
      <th>成绩</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>张三</td>
      <td>95</td>
    </tr>
    <tr>
      <td>李四</td>
      <td>100</td>
    </tr>
  </tbody>
</table>
```

## 精灵图

项目中将多张小图片，合并成一张大图片，这张大图片称之为精灵图。通过`background-position` 来控精灵图的显示位置。

```html
<style>
  span {
    display: inline-block;
    width: 18px;
    height: 24px;
    background-image: url(./images/taobao.png);
    background-repeat: no-repeat;
    background-position: -3px 0;
  }

  b {
    display: block;
    width: 25px;
    height: 21px;
    background-image: url(./images/taobao.png);
    background-repeat: no-repeat;
    background-position: 0 -90px;
  }
</style>
<span></span>
<b></b>
```

### 背景图片大小

设置背景图片的大小，`background-size:` + 宽度 高度：

1. 像素值
2. 百分百值
3. `contain` 将背景图片等比缩放到盒子模型内。
4. `cover` 将背景图片等比缩放填满盒模型。

```html
<style>
  .box {
    width: 400px;
    height:300px;
    background-color: pink;
    background-image: url(./images/1.jpg);
    background-repeat: no-repeat;
    background-size: cover;
  }
</style>
<div class="box"></div>
```

通用背景设置

`background` : color image repeat position/size

## 阴影

### 文字阴影

文字添加阴影效果：`text-shadow: h-shadow v-shadow blur color`

* `h-shadow` 必要，水平偏移量。
* `v-shadow` 必要，垂直偏移量。
* `blur` 可选，模糊度。
* `color` 可选，颜色。

```html
<style>
  .box {
    text-shadow: 10px 10px 20px red;
  }
</style>
<div class="box">文字阴影</div>
```

###  盒子阴影

给盒子模型添加阴影效果：`box-shadow: h-shadow v-shadow blur spread color inset `

* `h-shadow` 必要，水平偏移量。
* `v-shadow` 必要，垂直偏移量。
* `blur` 可选，模糊度。
* `spread` 可选，阴影扩大。
* `color` 可选，颜色。
* `inset` 可选，内阴影。

```html
<style>
  .box {
    width: 200px;
    height: 200px;
    background-color: pink;
    box-shadow: 5px 10px 20px 10px green inset;
  }
</style>
<div class="box"></div>
```

## 字体图标

向使用文字一样使用图标

[字体图标网站](https://www.iconfont.cn/)

```html
<style>
  .iconfont {
    font-size: 80px;
    color: skyblue;
  }
</style>
<i class="iconfont icon-wuxiaomingchengqingchu"></i>
```



