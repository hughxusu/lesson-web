# CSS 特性与盒子模型

## CSS 特性

1. 继承性
2. 层叠性
3. 优先级

### 继承性

子元素有默认继承父元素样式的特点（文字控制属性都是继承属性）。

常见应用场景：直接给body标签设置统一的font-size，从而统一不同浏览器默认文字大小

如果元素有浏览器默认样式，优先显示浏览器的默认样式：

1. a 标签的文字颜色浏览器有默认样式。
2. h 系列标签的 font-size 浏览器有默认样式。

### 层叠写

1. 给同一个标签设置不同的样式，样式会层叠叠加，共同作用在标签上。
2. 给同一个标签设置相同的样式，样式会层叠覆盖，最终写在最后的样式会生效。
3. 当样式冲突时，只有当选择器优先级相同时，才能通过层叠性判断结果。

### 优先级

不同选择器具有不同的优先级，优先级高的选择器样式会覆盖优先级低选择器样式。

> 优先级公式：
>
> __继承 < 通配符选择器 < 标签选择器 < 类选择器 < id选择器 < 行内样式 < `!important`__

```html
<style>
  #box {
    color: orange;
  }

  .box {
    color: blue;
  }

  div {
    color: green !important;
  }

  body {
    color: red;
  }
</style>
<div class="box" id="box" style="color: pink;">测试优先级</div>
```

> [!warning]
>
> 1. `!important` 写在属性值的后面，分号的前面。
> 2. `!important` 不能提升继承的优先级，只要是继承优先级最低。
> 3. 实际开发中不建议使用 `!important`。

#### 权重叠加计算

如果多个复合选择器选中元素，需要通过权重叠加计算方法，判断最终哪个选择器优先级最高，该选择器的样式会生效。

<img src="https://s1.ax1x.com/2023/03/12/ppMP8KS.png" style="zoom:60%;" />

比较规则：

1. 比较上一级数字，数值大者优先；
2. 如果相同再比较下一级；
3. 如果权重完全相同，根据层叠性判断。

```html
<style>
  /* (行内, id, 类, 标签) */

  /* (0, 1, 0, 1) */
  div #one {
    color: orange;
  }

  /* (0, 0, 2, 0) */
  .father .son {
    color: skyblue;
  }

  /* 0, 0, 1, 1 */
  .father p {
    color: purple;
  }

  /* 0, 0, 0, 2 */
  div p {
    color: pink;
  } 
</style>

<div class="father">
  <p class="son" id="one">我是一个标签</p>
</div>
```

## 盒子模型

页面中每个标签元素都可以看做一个矩形的区域，称之为盒子模型。

![](https://pic1.zhimg.com/80/v2-ad08059be04698f8a70d2729cea8ec18_1440w.webp)

盒子模型包括：

1. 内容区域（content）
2. 内边距区域（padding）
3. 边框区域（border）
4. 外边距区域（margin）

```html
<style>
  div {
    width: 300px;
    height: 300px;
    background-color: pink;
    border: 1px solid #000;
    padding: 20px;
    margin: 50px;
  }
</style>
<div>盒子1</div>
<div>盒子2</div>
```

### 内容的宽度和高度

`width` 和 `height` 属性可以设置盒子模型的内容区域大小。

```html
<style>
  div {
    width: 200px;
    height: 200px;
    background-color: pink;
  }
</style>
<div>内容</div>
```

### 边框

盒子模型边框可以设置单独效果

```html
<style>
  div {
    width: 200px;
    height: 200px;
    background-color: pink;

    border: 1px solid #000;

    border-left: 5px dotted #000;
    border-right: 5px dotted #000;
    border-top: 1px solid red;
    border-bottom: 1px solid red;
  }
</style>
<div>边框</div>
```

常用边框样式：solid、dotted、dashed

边框可以单独设置一种样式：

* `border-width`
* `border-color`
* `border-style`

可以设置不同方向的值：

1. 四个值顺序：上 右 下 左（照顺时针方向）
2. 三个值顺序：上  左右 下
3. 两个值顺序：上下 左右

```html
<style>
  div {
    width: 200px;
    height: 200px;
    background-color: pink;

    border-width: 10px;
    border-width: 10px 20px 30px 40px ;
  	border-width: 10px 20px 30px ;
  	border-width: 10px 20px ;
    
    border-style: double;
  	border-style: solid dotted dashed double;
    
    border-color: red;
  	border-color: red yellow orange blue;
  }
</style>
<div>边框</div>
```

### 内边距

设置边框与内容区域之间的距离。

```html
<style>
  div {
    width: 200px;
    height: 200px;
    background-color: pink;

    padding: 100px;
    padding: 100px 200px;
    padding: 100px 200px 300px;
    padding: 100px 200px 300px 400px;
    
    padding-left: 100px;
  	padding-right: 100px;
    padding-top: 50px;
  	padding-bottom: 50px;
  }
</style>
<div>内边距</div>
```

> [!warning]
>
> 盒子模型的实际大小是：内容大小 + 内边距大小 + 边框大小

#### 內减模式

CSS 3中可以设置，不需要手动计算盒子模型的实际高度。

```html
<style>
  div {
    width: 100px;
    height: 100px;
    background-color: pink;
    border: 10px solid #000;
    padding: 20px;

   
    box-sizing: border-box;
  }
</style>
<div>內减模式</div>
```

### 外边距

设置边框以外，盒子与盒子之间的距离

```html
<style>
  div {
    width: 200px;
    height: 200px;
    background-color: pink;
		
    margin: 100px 200px 300px 400px;
    margin-top: 100px;
    
    
  }
</style>
<div>外边距</div>
```

清除浏览器默认样式

```html
<style>
  * {
    margin: 0;
    padding: 0;
  }
</style>
```

#### 版心居中

```html
<style>
  div {
    width: 800px;
    height: 600px;
    background-color: pink;
    margin: 0 auto;
  }
</style>
<div>版心居中</div>
```

### 盒子模型的特殊情况

#### 垂直重叠

垂直布局的块级元素，上下的margin会合并，最终两者距离为margin的最大值。

```html
<style>
  div {
    width: 100px;
    height: 100px;
    background-color: pink;
  }

  .one {
    margin-bottom: 60px;
  }

  .two {
    margin-top: 50px;
  }
</style>
<div class="one">box1</div>
<div class="two">box2</div>
```

#### 外边距塌陷

互相嵌套的块级元素，子元素的 margin-top 会作用在父元素上，导致父元素一起往下移动。

```html
<style>
  .father {
    width: 300px;
    height: 300px;
    background-color: pink;
  }

  .son {
    width: 100px;
    height: 100px;
    background-color: skyblue;
    margin-top: 50px;
  }
</style>

<div class="father">
  <div class="son">son</div>
</div>
```

塌陷问题的解决方法：

1. 给父元素设置 `border-top` 或者 `padding-top`；
2. 给父元素设置`overflow:hidden`；
3. 转换成行内块元素；

#### 行内元素的margin和padding

1. 水平方向的margin和padding布局中有效；
2. 垂直方向的margin和padding布局中无效；

```html

<style>
  span {
    margin: 100px; 
    padding: 100px;
  }
</style>
<span>span</span>
<span>span</span>
```

## 案例

[PxCook](https://fancynode.com.cn/pxcook) 像素测量工具

[新浪首页标题栏](https://www.sina.com.cn/)