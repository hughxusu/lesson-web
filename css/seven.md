# 过渡与动画

## 过渡

让元素的样式逐渐变化，常常配合动画使用。`transition: 过渡属性 过渡时长 `：

* 过渡属性：具体属性名称，`all` 全部属性
* 过渡时长:  变化的时长，单位为秒

```html
<style>
  .box {
    width: 200px;
    height: 200px;
    background-color: pink;
    transition: width 1s, background-color 3s;
    /* transition: all 1s; */
  }

  .box:hover {
    width: 600px;
    background-color: red;
  }
</style>
<div class="box"></div>
```

## 2D 变换

通过平面转换来控制，包括：位移、旋转、缩放，来实现动画效果。

平面转换属性`transform`

![](https://www.w3.org/TR/css-transforms-1/examples/compound_transform.svg)

* `translate` 平移
* `rotate` 旋转
* `scale` 缩放

### 平移

```html
<style>
  .father {
    width: 500px;
    height: 300px;
    margin: 100px auto;
    border: 1px solid #000;
  }

  .son {
    width: 200px;
    height: 100px;
    background-color: pink;
    transition: all 0.5s;
  }

  .father:hover .son {
    transform: translate(100px, 50px);

    /* transform: translate(100%, 50%); */
    /* transform: translate(-100%, 50%); */
    /* transform: translate(100px); */
    /* transform: translateY(100px); */
  }
</style>

<div class="father">
  <div class="son"></div>
</div>
```

* `translate` 可以同时设置水平距离和垂直距离
* `translate` 可以设置百分比，参照物为盒子自身尺寸。
* `translate` 可以设置负值（X轴正向为右，Y轴正向为下），负值方向相反。
* 单独设置某个方向的移动距离：`translateX()` 和 `translateY()`

使用绝对定位和 `translate` 实现元素居中

```html
<style>
  .father {
    position: relative;
    width: 500px;
    height: 300px;
    margin: 100px auto;
    border: 1px solid #000;
  }

  .son {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    width: 200px;
    height: 100px;
    background-color: pink;          
  }
</style>
<div class="father">
  <div class="son"></div>
</div>
```

### 旋转

```html
<style>
  img {
    width: 250px;
    transition: all 2s;
  }

  img:hover {
    /* 顺 */
    transform: rotate(360deg);
    /* 逆 */
    /* transform: rotate(-360deg); */
  }
</style>
<img src="./images/rotate.png" alt="">
```

* 默认圆点是盒子中心点
* `transform-origin` 原点水平位置 原点垂直位置
  * 方位名词：left、top、right、bottom、center
  * 像素单位数值
  * 百分比（参照盒子自身尺寸计算）

```html
<style>
  img {
    width: 250px;
    border: 1px solid #000;
    transition: all 2s;
    transform-origin: right bottom;
  }

  img:hover {
    transform: rotate(360deg);
  }
</style>
<img src="./images/bg.png" alt="">
```

使用 `transform` 复合属性实现多形态转换

```html
<style>
  .box {
    width: 800px;
    height: 200px;
    border: 1px solid #000;
  }

  img {
    width: 200px;
    transition: all 3s;
  }

  .box:hover img {
    transform: translate(600px) rotate(360deg);
  }
</style>
<img src="./images/bg.png" alt="">
```

> [!warning]
>
> * 旋转会改变网页元素的坐标轴向
> * 先写旋转，则后面的转换效果的轴向以旋转后的轴向为准，会影响转换结果

### 缩放

```html
<style>
  .box {
    width: 300px;
    height: 300px;
    margin: 100px auto;
    background-color: pink;

  }

  .box img {
    width: 100%;
    transition: all 0.5s;
  }

  .box:hover img {
    transform: scale(0.8);
  }
</style>
<div class="box">
  <img src="./images/bg.png" alt="">
</div>
```

* scale 值大于1表示放大，scale值小于1表示缩小

### 背景渐变

```html
<style>
  .box {
    width: 300px;
    height: 200px;
    background-image: linear-gradient(
      transparent,
      rgba(0,0,0, .6)
    );
  }
</style>
<div class="box"></div>
```

* `linear-gradient` ：（颜色1，颜色2）

## 3D 变换

`transform` 属性实现元素在空间内的位移、旋转、缩放等效果。

![](https://image.zhangxinxu.com/image/blog/201209/3d_axes.png)

通常情况认为z轴垂直与屏幕。

### 开始透视效果

`perspective` 开启近大远小的空间变换效果

* 取值为像素， 合理值一般在800 – 1200。
* 该属性通常添加给父级。

### 平移

```html
<style>
  body {
    perspective: 800px;
  }

  .box {
    width: 200px;
    height: 200px;
    margin: 100px auto;
    background-color: pink;
    transition: all 0.5s;
  }

  .box:hover{
    transform: translate3d(100px, 100px, 200px);
    /* transform: translateX(100px); */
    /* transform: translateY(100px); */
    /* transform: translateZ(200px); */
  }
</style>
<div class="box"></div>
```

* `translateX / translateY / translateZ / translate3d` 实现平移效果。
* 像素单位数值和百分比。

### 旋转

z 轴旋转

```html
<style>
  .box {
    width: 300px;
    margin: 100px auto;
  }

  img {
    width: 300px;
    transition: all 2s;
  }

  .box img:hover {
    transform: rotateZ(360deg);
  }
</style>
<div class="box">
  <img src="./images/bg.png" alt="">
</div>
```

x 轴旋转

```html
<style>
  .box {
    width: 300px;
    margin: 100px auto;
  }

  img {
    width: 300px;
    transition: all 2s;
  }

  .box {
    /* 透视效果：近大远小，近实远虚 */
    perspective: 1000px;
  }

  .box img:hover {
    transform: rotateX(60deg);
  }
</style>
<div class="box">
  <img src="./images/bg.png" alt="">
</div>
```

y 轴旋转

```html
<style>
  .box {
    width: 300px;
    margin: 100px auto;
  }

  img {
    width: 300px;
    transition: all 2s;
  }

  .box {
    /* 透视效果：近大远小，近实远虚 */
    perspective: 1000px;
  }

  .box img:hover {
    transform: rotateY(60deg);
    transform: rotateY(-60deg);
  }
</style>
<div class="box">
  <img src="./images/bg.png" alt="">
</div>
```

立体旋转

```html
<style>
  .cube {
    position: relative;
    width: 200px;
    height: 200px;
    margin: 100px auto;
    transition: all 2s;
    transform-style: preserve-3d;
  }

  .cube div {
    position: absolute;
    left: 0;
    top: 0;
    width: 200px;
    height: 200px;
    line-height: 200px;
    text-align: center;
    font-size: 48px;
    font-weight: bold;
  }

  .front {
    background-color: skyblue;
    transform: translateZ(200px);
  }

  .back {
    background-color: pink;
  }

  .cube:hover {
    transform: rotateY(90deg);
  }
</style>
<div class="cube">
  <div class="front">前面</div>
  <div class="back">后面</div>
</div>
```

[CSS图形变换示例](https://front-end-tools.com/en/generatetransform/)

## 动画

使盒子模型在不同的状态简切换。

动画实现的方法

1. 定义动画

   * 开始结束方式

   ```css
   @keyframes 动画名称 {
     from {}
     to {}
   } 
   ```

   * 百分百方式

   ```css
   @keyframes 动画名称 {
     0% {}
     50% {}
     100% {}
   } 
   ```

2. 使用动画

```css
animation: 动画名称 动画时长;
```

案例

```html
<style>
  .box {
    width: 200px;
    height: 100px;
    background-color: pink;
    animation: change 1s;
  }


  @keyframes change {
    from {
      width: 200px;
    }
    to {
      width: 600px;
    }
  }

  @keyframes change {
    0% {
      width: 200px;
    }
    50% {
      width: 500px;
      height: 300px;
    }
    100% {
      width: 800px;
      height: 500px;
    }
  } 
</style>
<div class="box"></div>
```

### 动画属性

```css
animation: 动画名称 动画时长 速度曲线 延迟时间 重复次数 动画方向 执行完毕时状态;
```

案例

```html
<style>
  .box {
    width: 200px;
    height: 100px;
    background-color: pink;
    animation: change 1s linear; 
    /* 延迟1秒 */
    animation: change 1s 1s;
		/* 重复3次 */
    animation: change 1s 1s 3;
    /* 无限循环 */
    animation: change 1s infinite;
		/* 双方向变化 */
    animation: change 1s infinite alternate;
    /* 默认值, 动画停留在最初的状态 */
    animation: change 1s backwards;
    /* 动画停留在结束状态 */
    animation: change 1s forwards;
  }

  @keyframes change {
    from {
      width: 200px;
    }
    to {
      width: 600px;
    }
  }
</style>
<div class="box"></div>
```

单个动画属性

| 属性                        | 作用               | 取值                                                |
| --------------------------- | ------------------ | --------------------------------------------------- |
| `animation-name`            | 动画名称           |                                                     |
| `animation-duration`        | 动画时长           |                                                     |
| `animation-delay`           | 延迟时间           |                                                     |
| `animation-fill-mode`       | 动画执行完毕时状态 | `forwards` 最后一帧状态 <br/>`backwards` 第一帧状态 |
| `animation-timing-function` | 速度曲线           | `steps(n)` 逐帧动画                                 |
| `animation-iteration-count` | 重复次数           | `infinite` 为无限循环                               |
| `animation-direction`       | 动画执行方向       | `alternate` 为反向                                  |
| `animation-play-state`      | 暂停动画           | `paused`为暂停，通常配合`:hover`使用                |

案例

```html
<style>
  .box {
    width: 200px;
    height: 100px;
    background-color: pink;

    animation-name: change;
    animation-duration: 1.5s;
    animation-iteration-count: infinite;
    animation-direction: alternate;

  }

  .box:hover {
    animation-play-state: paused;
  }


  @keyframes change {
    from {
      width: 200px;
    }
    to {
      width: 600px;
    }
  }
</style>
<div class="box"></div>
```

### 补间动画和逐帧动画

![](https://assets.hongkiat.com/uploads/css3-animation-steps/second-hand.gif)

使用逐帧动画

```html
<style>
  .box {
    width: 140px;
    height: 140px;
    border: 1px solid #000;
    background-image: url(./images/bg.png);
    animation: run 1s steps(12) infinite;
  }

  @keyframes run {
    from {
      background-position: 0 0;
    } 
    to {
      background-position:  -1680px 0;
    }
  }
</style>
<div class="box"></div>
```

多组动画

```html
<style>
  .box {
    width: 140px;
    height: 140px;
    background-image: url(./images/bg.png);
    animation: 
      run 1s steps(12) infinite,
      move 4s forwards;
  }
  @keyframes run {
    from {
      background-position: 0 0;
    } 
    to {
      background-position:  -1680px 0;
    }
  }

  @keyframes move {
    from {
      transform: translateX(0);
    }
    to {
      transform: translateX(800px);
    }
  }
</style>
<div class="box"></div>
```

