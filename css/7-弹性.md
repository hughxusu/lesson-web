

# 弹性布局

## Flex 布局

 Flex 布局被称为弹性布局，且避免浮动布局中脱离文档流现象发生。

设置方式 `display: flex`

<img src="https://s1.ax1x.com/2023/04/12/ppjQUYV.png" style="zoom:67%;" />

1. 弹性容器
2. 弹性盒子
3. 主轴
4. 交叉轴

```html
<style>
  * {
    margin: 0;
    padding: 0;
  }

  .box {
    display: flex;
    height: 200px;
    width: 800px;
    border: 1px solid #000;
    margin: auto;
  }

  .box div {
    width: 100px;
    height: 100px;
    background-color: pink;
  }
</style>
<div class="box">
  <div>1</div>
  <div>2</div>
  <div>3</div>
</div>
```

### 主轴对齐方式

`justify-content` 属性可以调解主轴的对齐方式

| 属性值          | 作用                                               |
| --------------- | -------------------------------------------------- |
| `flex-start`    | 默认值，起点开始依次排列                           |
| `flex-end`      | 终点开始依次排列                                   |
| `center`        | 沿主轴居中排列                                     |
| `space-around`  | 弹性盒子沿主轴均匀排列，空白间距均分在弹性盒子两侧 |
| `space-between` | 弹性盒子沿主轴均匀排列，空白间距均分在相邻盒子之间 |
| `space-evenly`  | 弹性盒子沿主轴均匀排列，弹性盒子与容器之间间距相等 |

```css
.box {
  display: flex;
  justify-content: center;
  justify-content: space-between;
  justify-content: space-evenly;
  justify-content: space-around;

  height: 200px;
  width: 800px;
  margin: auto;
  border: 1px solid #000;
}
```

### 交叉轴对齐方式

`align-items` 可以调解交叉轴的对齐方式

`align-self` 可以调解单个盒子交叉轴的对齐方式

| 属性值       | 作用                                       |
| ------------ | ------------------------------------------ |
| `flex-start` | 默认值，起点开始依次排列                   |
| `flex-end`   | 终点开始依次排列                           |
| `center`     | 沿侧轴居中排列                             |
| `stretch`    | 默认值，弹性盒子沿着主轴线被拉伸至铺满容器 |

```html
<style>
  * {
    margin: 0;
    padding: 0;
  }

  .box {
    display: flex;
    align-items: center;
    align-items: stretch;
    height: 300px;
    width: 800px;
    margin: auto;
    border: 1px solid #000;
  }

  .box div {
    width: 100px;  
    height: 100px;
    background-color: pink;
  }
</style>
<div class="box">
  <div>1</div>
  <div>2</div>
  <div>3</div>
</div>
```

修改单个交叉轴对齐方式

```css
.box div:nth-child(2) {
	align-self: center;
}
```

### 伸缩比

```html
<style>
  * {
    margin: 0;
    padding: 0;
  }

  .box {
    display: flex;
    height: 300px;
    border: 1px solid #000;
    margin: auto;
    width: 800px;
  }

  .box div {
    height: 200px;
    margin: 0 20px;
    background-color: pink;
  }

  .box div:nth-child(1) {
    width: 50px;
  }

  .box div:nth-child(2) {
    flex: 3;
  }

  .box div:nth-child(3) {
    flex: 1;
  }
</style>
<div class="box">
  <div>1</div>
  <div>2</div>
  <div>3</div>
</div>
```

### 主轴方向

主轴默认是水平方向，交叉轴默认是垂直方向。

可以通过 `flex-direction` 改变主轴方向。

| 属性值           | 作用           |
| ---------------- | -------------- |
| `row`            | 水平（默认值） |
| `column`         | 垂直           |
| `row-reverse`    | 从右向左       |
| `column-reverse` | 从下向上       |

```html
<style>
  * {
    margin: 0;
    padding: 0;
  }

  .box {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
    height: 800px;
    width: 300px;
    margin: auto;
    border: 1px solid #000;
  }

  .box div {
    width: 100px;  
    height: 100px;
    background-color: pink;
  }
</style>
<div class="box">
  <div>1</div>
  <div>2</div>
  <div>3</div>
</div>
```

## 视口

视口表示浏览器中显示文档的区域。

css 可以设置视口相对单位：

1. 1 vw = 1 / 100 视口宽度
2. 1 vh = 1 /100 视口高度

可以使实现不同宽度的设备中，网页元素尺寸等比缩放效果。

```html
<style>
  * {
    margin: 0;
    padding: 0;
  }

  .box {
    margin: auto;
    width: 300px;
    height: 300px;
    border: 1px solid black;
  }

  .sub {
    width: 10vw;
    height: 10vh;
    background-color: skyblue;
  }
</style>

<div class="box">
  <div class="sub"></div>
</div>
```

