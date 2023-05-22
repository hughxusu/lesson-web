# 事件对象

事件对象也是个对象，这个对象里有事件触发时的相关信息。

在事件绑定的回调函数的第一个参数就是事件对象，一般命名为event、ev、e。

```js
let btn = document.querySelector('button')
btn.addEventListener('mouseenter', function (e) {
  console.log(e)
})
```

## 事件对象的属性

常见属性

* type 获取当前的事件类型
* clientX/clientY获取光标相对于浏览器可见窗口左上角的位置
* offsetX/offsetY获取光标相对于当前DOM元素左上角的位置
* key用户按下的键盘键的值现在不提倡使用keyCode

```html
<style>
  body {
    height: 3000px;
  }

  div {
    width: 300px;
    height: 300px;
    background-color: pink;
    margin: 100px;
  }
</style>
<div>
</div>
<script>
  document.addEventListener('click', function (e) {
    console.log('clientX:' + e.clientX, 'clientY:' + e.clientY)
    console.log('pageX:' + e.pageX, 'pageY:' + e.pageY)
    console.log('offsetX:' + e.offsetX, 'offsetY:' + e.offsetY)
  })
</script>
```

### 案例

图片跟随鼠标移动

```html
<style>
  img {
    position: absolute;
    top: 0;
    left: 0;
  }
</style>
<img src="./images/tianshi.gif" alt="">
<script>
  let img = document.querySelector('img')
  document.addEventListener('mousemove', function (e) {
    img.style.left = e.pageX - 50 + 'px'
    img.style.top = e.pageY - 40 + 'px'
  })
</script>
```

