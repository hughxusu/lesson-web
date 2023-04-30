# CSS 基础

CSS：层叠样式表（Cascading style sheets）

![](https://s1.ax1x.com/2023/02/08/pSRy1b9.jpg)

示例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        p {
            color: red;
            font-size: 24px;
            width: 480px;
            background-color: pink;
        }
    </style>
</head>
<body>
    <p>在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。</p>
</body>
</html>
```

CSS 语法规则

<img src="https://www.w3school.com.cn/i/css/selector.gif"  />



CSS 引入方式：

* 内嵌式：CSS 写在style标签中。
* 外联式：CSS 写在一个单独的 .css文 件中。
* 行内式：CSS 写在标签的style属性中。

```html
<p style="color: blue;">在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。</p>
```

> [!warning]
>
> 不需要死记硬背样式，编程中多尝试，熟能生巧。

## 常规选择器

### 标签选择器

选择页面中所有的标签来设置样式。

```html
<style>
  p {
    color: red;
  }
</style>
<p>在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。</p>
<p>这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。</p>
```

### 类型选择器

通过类名指定标签设置样式

```html
<style>
  .red {
    color: red;
  }
</style>
<p class="red">在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。</p>
<p>这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。</p>
```

### id选择器

通过 id 属性值设置样式。

```html
<style>
  #first {
    color: red;
  }
</style>
<p id="first">在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。</p>
<p>这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。</p>
```

> [!warning]
>
> 每个标签上的 `id`  属性值是唯一的，一个 `id` 选择器只能选中一个标签。

### 通配符选择器

设置页面中所有标签的样式。

```html
<style>
  * {
    color: red;
    margin: 0;
    padding: 0;
  }
</style>
<p id="first">在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。</p>
<p>这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。</p>
```

> [!warning]
>
> 1. 类选择器最常用，其次是标签选择器。
> 2. id一般配合js使用，除非特殊情况，否则不要使用id设置样式。
> 3. 通配符选择器一般用于去除页面自有样式。

## 文字与文本

### 文字样式

#### 文字大小

```html
<style>
  p {
    font-size: 24px;
  }
</style>
<p>在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。</p>
```

#### 文字粗细

```html
<style>
  p {
    font-weight: 100;
  }
</style>
<p>在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。</p>
```

* 关键词：normal / bold
* 数值：100 ~ 900 的整百数

#### 文字样式

是否斜体：正常（normal）/ 倾斜（italic）

```html
<style>
  p {
    font-style: italic;
  }
</style>
<p>在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。</p>
```

#### 文字字体

<img src="https://picx.zhimg.com/v2-eb5a7c60bd080db96536f0691a2c3a40_1440w.jpg?source=172ae18b" style="zoom: 100%;" />

* 无衬线字体（sans-serif）
  1. 文字笔画粗细均匀，并且首尾无装饰。
  2. 网页中大多采用无衬线字体。
  3. 常见字体：黑体、Arial
* 衬线字体（serif）
  1. 文字笔画粗细不均，并且首尾有笔锋装饰。
  2. 出版印刷中应用广泛。
  3. 常见字体：宋体、Times New Roman

![](https://s1.ax1x.com/2023/02/26/pp9PqII.jpg)

* 等宽字体（monospace）
  1. 每个字母或文字的宽度相等。
  2. 一般用于程序代码编写，有利于代码的阅读和编写。
  3. 常见字体：Consolas、fira code

```html
<style>
  p {
    /* 如果没有字体类型，就按任意一种非衬线字体系列显示 */
    font-family: STSong, STHeiti, sans-serif;
  }
</style>
<p>在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。</p>
```

> 问题：给同一个标签设置了相同的样式，此时浏览器会如何渲染呢?

#### 字体符合属性

将所有样式统一写在一个属性中。

```html
<style>
  p {
    font: italic 36px bold STSong, STHeiti, sans-serif;
  }
</style>
<p>在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。</p>
```

> [!warning]
>
> 1. 文字样式必须写在第一位，字体必须写在最后。
> 2. 只能省略样式和文字粗细。

### 文本样式

#### 文本缩进

```html
<style>
  p {
    font-size: 36px;
    text-indent: 72px;
  }
</style>
<p>在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。</p>
<p>这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。</p>
```

单位 em（1em = 当前标签一个字的大小）

#### 文本对齐方式

```html
<style>
  p {
    font-size: 36px;
    text-align: center;
  }
</style>
<p>在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。</p>
<p>这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。</p>
```

对齐选项：`left/right/center`

#### 文本修饰

```html
<style>
  p {
    font-size: 36px;
    text-decoration: underline;
  }
</style>
<p>在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。</p>
```

修饰选择：`underline`—下划线；`line-through`—删除线；`overline`—上划线；`none`—无修饰；

#### 行高

控制一行的上下行间距

<img src="https://s1.ax1x.com/2023/02/26/pp9ZeQx.png" style="zoom:35%;" />

```html
<style>
  p {
    font-size: 36px;
    line-height: 1.5;
  }
</style>
<p>在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。</p>
<p>这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。</p>
```

可以使用像素和倍数两种值。

> [!warning]
>
> 当行高等于标签高度时文字可以居中。

## Chrome 调试工具

<img src="https://img-blog.csdnimg.cn/20210312203831572.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODM2NDQy,size_16,color_FFFFFF,t_70" style="zoom: 45%;" />

## 样式中的颜色

已知文字可以设置颜色

| 颜色的表示方式 | 表示含有                            | 属性值                   |
| -------------- | ----------------------------------- | ------------------------ |
| 关键词         | 预定义颜色名称                      | `red`、`blue`            |
| rgb            | 红绿蓝三原色，每项取值范围是：0~255 | `rgb(50, 150, 250)`      |
| rgba           | 三原色加透明度，透明度取值：0~1     | `rgb(50, 150, 250, 0.5)` |
| 十六进制       | 十六进制字符                        | `#e9f322`                |

1. 关键词：常见英文单词。
2. `rgb(50, 150, 250)`：数字顺序为—红、绿、蓝。
3. 1—完全不透明；0—完全透明；可简写`rgb(50, 150, 250, .5)`

<img src="https://www.webhostdesignpost.com/img/html-hex-colors.png" style="zoom:80%;" />

4. 十六进制表示法：

* 两个数字为一组，每个数字的取值范围:0~9, a, b, c, d, e, f
* 如果三组中，每组数字都相同，此时可以每组可以省略只写一个数字。例如：\#ffaabb 改写成 #fab
* 常见值：`#fff`—白色；`#000`—黑色

## 案例

<img src="https://s1.ax1x.com/2023/02/26/pp9u0uq.png" style="zoom: 45%;" />