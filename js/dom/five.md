# 高级特性

## 其它事件

### 滚动事件

当页面进行滚动时触发的事件，监听整个页面滚动，监听某个元素的内部滚动直接给某个元素加即可。

事件名 `scroll`

```html
<style>
    body {
        height: 3000px;
    }

    div {
        overflow: auto;
        width: 200px;
        height: 200px;
        background-color: pink;
    }
</style>
<div>
    大段文字.....
</div>
<script>
    let div = document.querySelector('div')
    // window.addEventListener('scroll', function () {
    //     console.log(111)
    // })
    div.addEventListener('scroll', function () {
        console.log(111)
    })
</script>
```

## 事件加载

 加载外部资源加载完毕时触发的事件，不光可以监听整个页面资源加载完毕，也可以针对某个资源绑定load事件。

```js
window.addEventListener('load', function () {
    let div = document.querySelector('div')
    console.log(div)
})
```

当初始的 HTML 文档被完全加载和解析完成之后，DOMContentLoaded 事件被触发，而无需等待样式表 、图像等完全加载。
