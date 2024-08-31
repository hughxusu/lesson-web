# CSS

css（层叠样式表 Cascading Style Sheets），网页是一层层结构，层次高的将会覆盖层次低的。css可以分别为每个网页层次设置样式。

## 起步

```html
<html>
	<head>
		<meta charset="utf-8" />
		<title>CSS</title>
		<!--css可以写入head中的style标签里-->
		<style type="text/css">
			/*
				CSS的语法：[选择器]:[声明块]
				选择器：通过选择器可以选中页面中指定元素，并修改其样式
				声明块：声明块是键值对结构，多个声明之间使用';'隔开
			*/
			p {
				color:red;
				font-size:40px;
			}
		</style>
	</head>
	<body>
		<!--内联样式: CSS样式写入元素style属性，联样式只对当前的元素中的内容起作用-->
    <p style="color:red;font-size:40px;">锄禾日当午，汗滴禾下土</p>
		<p>谁知盘中餐，粒粒皆辛苦</p>
	</body>
</html>
```

外部文件引用样式表

```html
<html>
	<head>
		<meta charset="utf-8" />
		<title>CSS</title>
		<!-- 
			link标签引入外部的CSS文件，外部文件中css样式会应用到当前页面中，外部样式可以利用浏览器的缓存。
		-->
		<link rel="stylesheet" type="text/css" href="style.css" />
	</head>
	<body>
		<p>谁知盘中餐，粒粒皆辛苦</p>
	</body>
</html>
```

## 选择器

### 常用选择器

```html
<html>
	<head>
		<meta charset="UTF-8">
		<title>常用选择器</title>
		<style type="text/css">
			/* 元素选择器，选择p元素和h1元素 */
			*p {
				color: red;
			}
			h1{
				color: red;
			}
			/* id选择器，id属性是唯一值 */
			#p1{
				font-size: 20px;
			}
			/* 类选择器，通过元素的class属性值选中一组元素 */
			.p2{
				color: red;
			}
			/* 选择器分组(并集选择器) */
			#p1, .p2, h1{
				background-color: yellow;
			}
			/* 通配选择器：可以用来选中页面中的所有的元素 */
			*{
				color: red;
			}
			/* 
       *复合选择器（交集选择器）可以选中同时满足多个选择器的元素 
       *对于id选择器来说，不建议使用复合选择器
      */
			span.p3#p1{
				background-color: yellow;
			}
			
		</style>
	</head>
	<body>
		<h1>悯农</h1>
		<p>锄禾日当午</p>
		<p id="p1">锄禾日当午</p>
		<p class="p2 hello">锄禾日当午</p>
		<p class="p2">锄禾日当午</p>
		<span class="p3">汗滴禾下土</span>
	</body>
</html>
```

### 父子选择器

```html
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			/*
			 * 为div中的span设置一个颜色为绿色
			 * 后代元素选择器
			 * 	- 作用：
			 * 		- 选中指定元素的指定后代元素
			 * 	- 语法：
			 * 		祖先元素 后代元素{}	
			 */
			#d1 span{
				color: greenyellow;
			}
			/*
			 * 选中id为d1的div中的p元素中的span元素
			 */
			#d1 p span{
				font-size: 50px;
			}
			/*
			 * 为div的子元素span设置一个背景颜色为黄色
			 * 子元素选择器
			 * 	- 作用：
			 * 		- 选中指定父元素的指定子元素
			 * 	- 语法：
			 * 		父元素 > 子元素
			 * IE6及以下的浏览器不支持子元素选择器
			 */
			div > span{
				background-color: yellow;
			}
		</style>
	</head>
	<body>
		<!--
			元素之间的关系
				父元素：直接包含子元素的元素
				子元素：直接被父元素包含的元素
				祖先元素：直接或间接包含后代元素的元素，父元素也是祖先元素
				后代元素：直接或间接被祖先元素包含的元素，子元素也是后代元素
				兄弟元素：拥有相同父元素的元素叫做兄弟元素			
		-->
		<div id="d1">
			<span>我是div标签中的span</span>
			<p><span>我是p标签中的span</span></p>
		</div>
		<div>
			<span>我是body中的span元素</span>
		</div>
	</body>
</html>
```

### 伪类选择器

```html
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			/* 伪类专门用来表示元素的一种的特殊的状态，比如：访问过的超链接、普通超链接、获取焦点的文本框 */
      
			/* link 普通的链接（没访问过）
			 */
			a:link{
				color: yellowgreen;
			}
			/* visited 表示访问过的链接，由于涉及到隐私问题，所以visited伪类只能设置字体颜色 */
			a:visited{
				color: red;
			}
			/* :hover伪类表示鼠标移入的状态 */
			a:hover{
				color: skyblue;
			}
			/* :active表示的是超链接被点击的状态 */
			a:active{
				color: black;
			}
			/* 文本框获取焦点后，修改背景颜色为黄色 */
			input:focus{
				background-color: yellow;
			}
			/* p标签中选中的内容使用样式 */
			/* 火狐 */
			p::-moz-selection{
				background-color: orange;
			}
			/* 其他 */
			p::selection{
				background-color: orange;
			}
		</style>
	</head>
	<body>
		<a href="http://www.baidu.com">访问过的链接</a>
		<br />
		<a href="http://www.baidu123456.com">没访问过的链接</a>
		<p>我是一个段落</p>
		<!-- 使用input可以创建一个文本输入框 -->
		<input type="text" />
	</body>
</html>
```

### 伪元选择器

```html
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			/* 伪元素用来表示元素中的一些特殊的位置 */
      
			/* 为p中第一个字符来设置一个特殊的样式 */
			p:first-letter {
				color: red;
				font-size: 20px;
			}
			/* 为p中的第一行设置一个背景颜色为黄色 */
			p:first-line {
				background-color: yellow;
			}
			/*
			 * before表示元素最前边的部分
       * after表示元素最后边的部分
			 * content可以向before或after的位置添加一些内容
			 */
			p:before{
				content: "我会出现在整个段落的最前边";
				color: red;
			}
			p:after{
				content: "我会出现在整个段落的最后边";
				color: orange;
			}
		</style>
	</head>
	<body>
		<p>
			在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。
		</p>
	</body>
</html>
```

### 属性选择器

```html
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			/*
			 * 属性选择器：可以根据元素中的属性或属性值来选取指定元素
			 * [属性名] 选取含有指定属性的元素
			 * [属性名="属性值"] 选取含有指定属性值的元素
			 * [属性名^="属性值"] 选取属性值以指定内容开头的元素
			 * [属性名$="属性值"] 选取属性值以指定内容结尾的元素
			 * [属性名*="属性值"] 选取属性值以包含指定内容的元素
			 */
      
      /* 选择含有title的p元素 */
			p[title]{
				background-color: yellow;
			}
			/* 选择title含有属性值是hello的元素 */
			p[title="hello"]{
				background-color: yellow;
			}
			/* 选择title属性值以ab开头的元素 */
			p[title^="ab"]{
				background-color: yellow;
			}
			/* 选择title属性值以c结尾的元素 */
			p[title$="c"]{
				background-color: yellow;
			}
      /* 选择title属性值包含c的元素色 */
			p[title*="c"]{
				background-color: yellow;
			}
		</style>
	</head>
	<body>
		<!--
			title 可以给任何标签指定
			当鼠标移入元素时，元素中title属性的值将会作为提示文字显示
		-->
		<p title="hello">我是一个段落</p>
		<p>我是一个段落</p>
		<p title="hello">我是一个段落</p>
		<p title="abbc">我是一个段落</p>
		<p title="abccd">我是一个段落</p>
		<p title="abc">我是一个段落</p>
	</body>
</html>
```

### 子元素选择器

```html
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			/* :first-child 可以选中第一个子元素 */
			p:first-child{
				background-color: yellow;
			}
			/* :last-child 可以选中最后一个子元素 */
			p:last-child{
				background-color: yellow;
			}
			/*
			 * :nth-child 可以选中任意位置的子元素
			 * even 表示偶数位置的子元素
			 * odd 表示奇数位置的子元素
			 */
			p:nth-child(odd){
				background-color: yellow;
			}
			/*
			 * :first-of-type
			 * :last-of-type
			 * type是在当前类型的子元素中排列
			 */
			p:first-of-type{
				background-color: yellow;
			}
			p:last-of-type{
				background-color: yellow;
			}
		</style>
	</head>
	<body>
		<span>我是span</span>
		<p>我是一个p标签</p>
		<p>我是一个p标签</p>
		<span>hello</span>
	</body>
</html>
```

### 兄弟元素选择器

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			/* 
       * 后一个兄弟元素选择器，可以选中一个元素后紧挨着的指定的兄弟元素
			 * span的后一个p标签
			 */
			span + p {
				background-color: yellow;
			}
			/* 前一个兄弟元素选择器 */
			span ~ p{
				background-color: yellow;
			}
		</style>
	</head>
	<body>
		<p>我是一个p标签</p>
		<span>我是一个span</span>
		<p>我是一个p标签</p>
	</body>
</html>
```

### 否定元素选择器

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			/* 否定伪类，可以从已选中的元素中剔除出某些元素 */
			p:not(.hello){
				background-color: yellow;
			}
		</style>
	</head>
	<body>
		<p>我是一个p标签</p>
		<p>我是一个p标签</p>
		<p class="hello">我是一个p标签</p>
	</body>
</html>
```

## 样式关系

### 样式继承

在CSS中，祖先元素上的样式，也会被他的后代元素所继承，但是背景相关的样式、边框相关的样式和定位相关的样式都不会被继承。

### 选择器优先级

选择器定义的样式，由选择器的优先级（权重）决定，优先级高的优先显示。

* 		内联样式 ， 优先级  1000
* 		id选择器，优先级   100
* 		类和伪类， 优先级   10
 * 		元素选择器，优先级 1 
 * 		通配* ，    优先级 0
 * 		继承的样式，没有优先级

交集选择器将多种选择器的优先级相加然后在比较，选择器优先级计算不会超过他的最大的数量级，如果选择器的优先级一样；并集选择器的优先级是单独计算；

`!important`则此时该样式将会获得一个最高的优先级。

a标签伪类选择器的优先级是一样的。

```css
.p1{
  color: yellow;
  background-color: greenyellow !important;
}
```

## 样式内容

### 长度单位

```css
/*
 * 长度单位：1.像素px 2.百分比% 3.em和百分比类似，它是相对于当前元素的字体大小来计算的
 * 当元素的宽度的值为auto时，此时指定内边距不会影响可见框的大小，而是会自动修改宽度，以适应内边距 
 */
.box{
  width: 300px;
  height: 300px;
  width: auto;
  background-color: red;
}
.box1{
  font-size: 20px;
  width: 2em;
  height: 50%;
  background-color: yellow;
}
```

### 颜色

```css
.box1{
  width: 100px;
  height: 100px;
  background-color: rgb(161, 187, 215);
  background-color: rgb(100%, 50%, 50%);
  background-color: #abc;
}
```

### 字体

```css
.p1{
  color: red; /* 字体颜色使用color设置*/
  font-size: 30px;
  /*
   * 字体式可以同时指定多个字体，多个字体之间使用‘,’分开
   * 当采用多个字体时，浏览器会优先使用前边的字体，如果前边没有在尝试下一个
   */
  font-family: arial , 微软雅黑;
}
```

#### 字体的分类

在网页中将字体分成5大类：1.serif（衬线字体）2.sans-serif（非衬线字体）3.monospace （等宽字体）4.cursive （草书字体）5.fantasy （虚幻字体）

```css
p {
  font-family: arial, 微软雅黑, 华文彩云, serif;
}
```

#### 字体的其他样式

```css
.p1{
  color: red;
  font-size: 30px; /* 设置文字大小 */
  font-family: "微软雅黑";
  /* 设置文字斜体：1.normal，2.italic，3.oblique一般与italic相同 */
  font-style: italic;
  /* 文本加粗：1.normal，2.bold */
  font-weight: bold;
  /* 设置小型大写字母：1.normal，2.small-caps */
  font-variant: small-caps ;
}
.p3{
  /*
   * font可以同时设置字体相关的所有样式，不同的值之间使用空格隔开
   * 使用font设置字体样式时，没有顺序要求，不写则使用默认值，但是文字大小和字体必须写，
   * 且字体必须是最后一个样式，大小必须是倒数第二个样式
   */
  font: small-caps bold italic 60px "微软雅黑";
}
```

#### 行间距

```css
/* 通过行高来间接修改行间距：行间距 = 行高 - 字体大小 */
.p1{
  font-size: 20px;
  /*
	 * 设置行高：
   * 1.直接就收一个大小
   * 2.可以指定一个百分数，则会相对于字体去计算行高
   * 3.可以直接传一个数值，则行高会设置字体大小相应的倍数
   */
  line-height: 200px;
  line-height: 200%;
  line-height: 2;
}
```

#### 文本样式

```css
.p1 {
  /*
* 设置文本的大小写：
* 1.none默认值 2.capitalize单词首字母大写 3.uppercase所有字母大写 4.lowercase所有字母小写
   */
  text-transform: lowercase;
}
.p2 {
  /*
   * 设置文本修饰：
   * 1.none默认值 2.underline下划线 3.overline上划线 4.line-through删除线
   */
  text-decoration: line-through;
}
a {
  /* 超链接会默认添加下划线：text-decoration默认值是underline */
  text-decoration: none; 	/* 去除超链接下划线需要设置该样式为none */
}
.p3 {
  /* 字符间距 */
  letter-spacing: 10px;
  /* 设置单词之间的距离 */
  word-spacing: 120px;
}
.p4{
  /*
   * 设置文本对齐方式：
   * 1.left默认值，左对齐 2.right右对齐 3.center居中 4.justify两端对齐
   */
  text-align: justify ;
}
.p5{
  font-size: 20px;
  /*
   * 首行缩进（可以将一些不想显示的文字隐藏起来）
   * 1.正值，向右侧缩进指定像素
   * 2.负值，向左移动指定像素
   * 这个值一般都会使用em作为单位
   */
  text-indent: -99999px;
}
```

## 盒子模型

![](https://www.runoob.com/images/box-model.gif)

* Margin(外边距) - 清除边框外的区域，外边距是透明的。
* Border(边框) - 围绕在内边距和内容外的边框。
* Padding(内边距) - 清除内容周围的区域，内边距是透明的。
* Content(内容) - 盒子的内容，显示文本和图像。
* Width - 内容区的宽度
* Height - 内容区的高度

### 边框

```css
.box1{
  width: 300px; /* 内容区的宽度 */
  height: 300px; /* 内容区的高度 */
  /*
   * border-width: 边框的宽度
   * border-color: 边框颜色
   * border-style: 边框的样式
   */
  border-width:10px; /* 设置边框的宽度 */
  /*
   * border-width设置模式
   * 四个值顺序：上 右 下 左（照顺时针方向）
   * 三个值顺序：上  左右 下
   * 两个值顺序：上下 左右
   */
  border-width: 10px 20px 30px 40px ;
  border-width: 10px 20px 30px ;
  border-width: 10px 20px ;
  border-width: 10px;
  /* border-xxx-width： top right bottom left 指定边的宽度 */
  border-left-width:100px ;
  /* 设置边框的颜色，设置模式如上 */
  border-color: red;
  border-color: red yellow orange blue;
  border-color: red yellow orange;
  border-color: red yellow;
  /*
   * 设置边框的样式（设置模式上）：
   * 1.none默认值 2.solid实线 3.dotted点状边框 4.dashed虚线 5.double双线
   */
  border-style: double;
  border-style: solid dotted dashed double;
}
.box{
  /* 边框简写样式，没有顺序要求 */
  border: red solid 10px;
  /* 可以单独设置四个边的样式 */
  border-left: red solid 10px;
  border-top: red solid 10px;
  border-bottom: red solid 10px;
  border-left: red solid 10px;
}
```

### 内边距

```css
.box1{
  /* 内边距设置，设置模式与边框类似 */
  padding-top: red solid 100px;

  padding: 100px;
  padding: 100px 200px;
  padding: 100px 200px 300px;
  padding: 100px 200px 300px 400px;
}
```

### 外边距

```css
.box1{
  /* 设置内外边距，设置模式与边框类似 */
  margin-top: 100px;
  /* 外边距设置负值，元素会向反方向移动 */
  margin-left: -150px;
  /* 可以设置为auto，一般只设置给水平方向 */
  margin-left: auto;
  margin-right: auto;
  margin: 0 auto;
}
```

外边距的垂直重叠（网页中相邻的垂直方向的外边距会发生重叠）

   * 		兄弟元素之间相邻外边距会取最大值，不是取和。
   * 		父子元素垂直外边距相邻了，则子元素的外边距会设置给父元素。

### 浏览器的默认样式

浏览器给一般元素都设置默认的margin和padding。

```css
/* 清除浏览器的默认样式 */
* {
  margin: 0;
  padding: 0;
}
```

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

## 背景

### 背景图片

 使用`background-image`来设置背景图片：

* 如果背景图片大于元素，默认会显示图片的左上角
* 如果背景图片和元素一样大，则会将背景图片全部显示
* 如果背景图片小于元素大小，则会默认将背景图片平铺以充满元素
* 可以同时为一个元素指定背景颜色和背景图片，背景颜色将会作为背景图片的底色。

使用`background-repeat`设置背景图片的重复方式：

* repeat，默认值，背景图片会双方向重复（平铺）
* no-repeat ，背景图片不会重复，有多大就显示多大
* repeat-x， 背景图片沿水平方向重复
* repeat-y，背景图片沿垂直方向重复

```css
.box1{
  width: 1024px;
  height: 724px;
  margin: 0 auto;
  /* 设置背景样式 */
  background-color: #bfa;
  background-image:url(img/1.png);
  background-repeat: repeat-y;
}

.box1{
	/* 相对路径文件夹 */	
	background-image:url(../img/2.jpg);
}
```

### 背景图片位置

通过`background-position`可以调整背景图片在元素中的位置

* 可选值：该属性可以使用 top right left bottom center中的两个值
* 以直接指定两个偏移量：第一个值是水平偏移量（正值向右移动/负值向左移动）；第二个是垂直偏移量（正值向下移动/负值向上移动）

通过`background-attachment`设置背景图片是否随页面一起滚动：

1.scroll，默认值，背景图片随着窗口滚动；2.fixed，背景图片会固定在某一位置，不随页面滚动

```css
.box1{
  height: 500px;
  margin:  0 auto;
  background-color: #bfa; /* 设置一个背景颜色 */
  background-image: url(img/4.png); /* 设置一个背景图片 */
  background-repeat: no-repeat; /* 设置一个图片不重复 */
  background-position: -80px -40px;
}
body{
  background-image: url(img/3.png);
  background-repeat: no-repeat;
  background-attachment:fixed ;
}
```

### 背景简写

`background`可以同时设置所有背景相关的样式，没有顺序的要求。

```css
body {
  background: #bfa url(img/3.png) center center no-repeat fixed;
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



