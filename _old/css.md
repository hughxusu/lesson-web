# CSS

css（层叠样式表 Cascading Style Sheets），网页是一层层结构，层次高的将会覆盖层次低的。css可以分别为每个网页层次设置样式。

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

### 溢出

子元素默认是存在于父元素的内容区中，理论上子元素最大可以等于父元素内容区大小，如果子元素超过了父元素内容区，则超出大小会在父元素以外的位置显示，超出父元素的内容，称为溢出的内容，父元素默认是将溢出内容，显示在父元素外边。

```css
.box1{
  /*
   * 	设置父元素如何处理溢出内容：
   * 	1.visible 默认值，不会对溢出内容做处理，元素会在父元素以外的位置显示
   * 	2.hidden 溢出的内容，会被修剪，不会显示
   * 	3.scroll 会为父元素添加滚动条，通过拖动滚动条来查看完整内容，该属性不论内容是否溢出，都会添加水平和垂直双方向的滚动条
   * 	4.auto 会根据需求自动添加或隐藏滚动条
   */
  overflow: auto;
}
```

## 文档流

文档流处在网页的最底层，它表示的是一个页面中的位置，元素默认都处在文档流中。

* 块元素
  1. 块元素在文档流中会独占一行，块元素会自上向下排列。
  2. 块元素在文档流中默认宽度是父元素的100%。
  3. 块元素在文档流中的高度默认被内容撑开。
* 内联元素
  1. 内联元素在文档流中只占自身的大小，会默认从左向右排列，如果一行中不足以容纳所有的内联元素，则换到下一行，继续自左向右。
  2. 在文档流中，内联元素的宽度和高度默认都被内容撑开.

## 浮动

块元素在文档流中默认垂直排列，如果希望块元素在页面中水平排列，可以为元素设置浮动（float属性是一个非none的值）。

* 元素浮动后，会立即脱离文档流。

   * 	元素脱离文档流以后，它下边的元素会立即向上移动。
   * 	元素浮动以后，会尽量向页面的左上或右上漂浮，直到遇到父元素的边框或者其他的浮动元素。
   * 	如果浮动元素上边是一个没有浮动的块元素，则浮动元素不会超过块元素。
   * 	浮动的元素不会超过他上边的兄弟元素，最多最多一边齐。

```css
.box1{
  /* 1.none 默认值，元素在文档流中 2.left左侧浮动 2.right右侧浮动 */
  float: left;
}
```

### 浮动与文字

浮动的元素不会盖住文字，文字会自动环绕在浮动元素的周围。

### 内联元素浮动

1. 在文档流中，子元素的宽度默认占父元素的全部。
2. 当元素设置浮动以后，会完全脱离文档流。块元素脱离文档流以后，高度和宽度都被内容撑开。
3. 内联元素脱离文档流以后会变成块元素

### 清除浮动

```css
.box2{
  /*
   * 清除掉浮动元素对当前元素产生的影响：
   * 1.none 默认值，不清除浮动
   * 2.left 清除左侧浮动元素影响
   * 3.right 清除右侧浮动元素影响
   * 4.both 清除两侧浮动元素影响
   * 清除的是影响最大的元素，浮动清除浮动后，元素会回到其它元素浮动前位置
   */
  clear: left;
}
```

### 高度塌陷

在文档流中，父元素的高度默认被子元素撑开的。当为子元素设置浮动以后，会导致子元素无法撑起父元素的高度，形成父元素的高度塌陷。

#### 解决高度塌陷的三种方式

方式一

利用元素隐含属性Block Formatting Context简称BFC，该属性默认关闭。当开启元素的BFC以后，元素将会具有如下的特性：

1. 父元素的垂直外边距不会和子元素重叠	
2. 开启BFC的元素不会被浮动元素所覆盖
3. 开启BFC的元素可以包含浮动的子元素

元素BFC需要间接开启

* 设置元素浮动（导致高度塌陷）
* 设置元素绝对定位
* 设置元素为inline-block可以解决问题，但是会导致宽度丢失
* 将元素的overflow设置为一个非visible的值（推荐方式）

```css
.box1{
  overflow: hidden; /* 给父元素开启BFC模式解决高度塌陷问题 */
}
```

方式二

在高度塌陷的父元素的最后添加一个空白的div，清除其浮动，由于并没有浮动所以可以撑开父元素的高度，这种方会在页面中添加多余的结构。
```html
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			.box1{
				border: 1px solid red;
			}
			.box2{
				width: 100px;
				height: 100px;
				background-color: blue;
				float: left;
			}
			.clear{
				clear: both; /* 清除浮动元素对当前元素产生的影响 */
			}
		</style>
	</head>
	<body>
		<div class="box1">
			<div class="box2"></div>
			<div class="clear"></div> <!--处理高度塌陷div-->
		</div>
	</body>
</html>
```

方式三

通过after伪类，添加一个空白的块元素，清除浮动。原理div的一样，可以达到一个相同的效果，但不会在页面中添加多余的div。
```html
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			.box1{
				border: 1px solid red;
			}
			.box2{
				width: 100px;
				height: 100px;
				background-color: blue;
				float: left;
			}
			.clearfix:after{ /* 给父元素添加伪类解决高度塌陷问题 */
				content: ""; /* 添加一个内容 */
				display: block; /* 转换为一个块元素 */
				clear: both; /* 清除两侧的浮动 */
			}
			
			.clearfix{ /* 在IE6中不支持after伪类 */
				zoom:1;
			}
		</style>
	</head>
	<body>
		<div class="box1 clearfix">
			<div class="box2"></div>
		</div>
	</body>
</html>
```

## 定位

定位指的是将指定的元素摆放到页面的任意位置，通过定位可以任意的摆放元素。

`position`属性设置元素定位：1.static默认值，没有开启定位 2.relative相对定位 3.absolute绝对定位 4.fixed固定定位（绝对定位的一种）

### 相对定位

* 当开启了元素的相对定位以后，而不设置偏移量时，元素不会发生任何变化。
* 相对定位是相对于元素在文档流中原来的位置进行定位。
* 相对定位的元素不会脱离文档流。
* 相对定位会使元素提升一个层级。
* 相对定位不会改变元素的块或内联性质
* 相对定位设置，通过left、right、top、bottom四个属性来设置元素的偏移量，只需要使用两个就可以对一个元素进行定位。

```css
position: relative;
left: 200px;
```

### 绝对定位

   * 	开启绝对定位，会使元素脱离文档流
   * 	开启绝对定位以后，如果不设置偏移量，则元素的位置不会发生变化
   * 	绝对定位是相对于最近开启定位的祖先元素进行定位（开启了子元素的绝对定位都会同时开启父元素的==相对定位==）
   * 		如果所有的祖先元素都没有开启定位，则会相对于浏览器窗口进行定位
   * 	绝对定位会使元素提升一个层级
   * 	绝对定位会改变元素的性质，内联元素变成块元素，块元素的宽度和高度默认都被内容撑开
   * 	==上面三种清除浮动的方法对绝对定位无效==

```css
.box2{
  width: 200px;
  height: 200px;
  position: absolute;
  left: 100px;
  top: 100px;
}
```

### 固定定位

固定定位也是一种绝对定位，它的大部分特点都和绝对定位一样，不同点是：

* 固定定位永远都会相对于浏览器窗口进行定位。
* 固定定位会固定在浏览器窗口某个位置，不会随滚动条滚动。
* IE6不支持固定定位

```css
.box2{
  width: 200px;
  height: 200px;
  position: fixed;
  left: 0px;
  top: 0px;
}
```

### 元素的层级

* 如果定位元素的层级是一样，则下边的元素会盖住上边元素。
* 通过z-index属性可以用来设置元素的层级，可以为z-index指定一个正整数作为值，该值将会作为当前元素的层级，层级越高，越优先显示。
* 对于没有开启定位的元素不能使用z-index
* 父元素的层级再高，也不会盖住子元素。

```css
.box2{
  width: 200px;
  height: 200px;
  position: absolute; /* 开启绝对定位 */
  top: 100px; /* 设置偏移量 */
  left: 100px;
  z-index: 25; /* 设置元素层级 */
  opacity: 0.5;
  filter: alpha(opacity=50);
}

.box3{
  /*
   * opacity设置元素背景透明度
   * 0-1之间的值：0表示完全透明；1表示完全不透明
   */
  opacity: 0.5;
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

### clearfix

```css
.box2 {
  width: 200px;
  height: 200px;
  background-color: yellow;
  /* 子元素和父元素相邻的垂直外边距会发生重叠，子元素的外边距会传递给父元素
   * 使用空的table标签可以隔离父子元素的外边距，阻止外边距的重叠 */
  margin-top: 100px;
}

/* 解决父子元素的外边距重叠 */
.box1:before{
  content: "";
	display: table; /* 可以将一个元素设置为表格显示 */
}

/* 解决父元素高度塌陷 */
.clearfix:after{
  content: "";
  display: block;
  clear: both;
}

/* 既可以解决高度塌陷，又可以确保父元素和子元素的垂直外边距不会重叠 */
.clearfix:before,
.clearfix:after{
  content: "";
  display: table;
  clear: both;
}

.clearfix{
  zoom: 1;
}
```



