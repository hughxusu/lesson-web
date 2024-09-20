# CSS

## 盒子模型

### 内联元素的盒子模型

1. 内联元素内容区、内边距 、边框 和外边距不能设置width和height。
2. 内联元素不支持垂直外边距

```css
.s1{
  /* 内联元素可以设置水平方向的内边距 */
  padding-left: 100px ;
  padding-right: 100px ;
  /* 内联元素可以设置垂直方向内边距，但是不会影响页面的布局 */
  padding-top: 50px;
  padding-bottom: 50px;
  /* 内联元素可以设置边框，但是垂直的边框不会影响到页面的布局 */
  border: 1px blue solid;
  /* 内联元素支持水平方向的外边距 */
  margin-left:100px ;
  margin-right: 100px;
}
.s2{
  /* 水平方向的相邻外边距不会重叠，而是求和 */
  margin-left: 100px;
}
```

修改内联元素类型

```css
a{
  /*
   * 通过display样式可以将一个内联元素变成块元素
   * 1.inline：设置为内联元素
   * 2.block：设置为块元素
   * 3.inline-block：设置为行内块元素，元素既有行内元素的特点又有块元素的特点，既可以设置宽高，又不会独占一行
   * 4.none: 不显示元素，并且元素不会在页面中继续占有位置
   */
  display: none;
}
.box{
  /*
   * 可以用来设置元素的隐藏和显示的状态
   * 1.visible 默认值，元素默认会在页面显示
   * 2.hidden 隐藏元素，但是它的位置会依然保持
   */
  visibility:hidden ;
}
```

## 表格样式

```css
table {
  width: 300px; /* 设置表格的宽度 */
  margin: 0 auto; /* 居中 */
  border:1px solid black; /* 边框 */
  /* able和td边框之间默认有一个距离，
   * 通过border-spacing属性可以设置这个距离 */
  border-spacing:0px;
  /* border-collapse可以用来设置表格的边框合并，
   * 如果设置了边框合并，则border-spacing自动失效 */
  border-collapse: collapse;
  background-color: #bfa; /* 设置背景样式 */
}

td, th{
  border: 1px solid black; /* 设置边框 */
}

tr:nth-child(even){
  background-color: #bfa; /* 设置隔行变色 */
}

tr:hover{
  background-color: #ff0; /* 鼠标移入到tr以后，改变颜色 */
}
```





