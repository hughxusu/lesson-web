# 数组

数组（Array）是一种可以按顺序保存数据的数据类型。

```js
let colors = ['red', 'green', 'blue', 'yellow', 'white', 'black']
person = ['李四', 18, True, 1.82]
```

列表中的每个元素都对应唯一的索引值。

<img src="https://www.runoob.com/wp-content/uploads/2014/05/positive-indexes-1.png" style="zoom: 50%;" />

1. 数组可以存储任意类型的数据。
2. 元素：数组中保存的每个数据都叫数组元素。
3. 下标：数组中数据的编号。
4. 长度：数组中数据的个数，通过数组的 `length` 属性获得。

```js
let colors = ['red', 'green', 'blue', 'yellow', 'white', 'black']
console.log(colors.length)
```

## 列表的操作

列表用于存储多个数据，常用的操作可以归类为：添加、删除、修改、读取等。

### 读取

通过下标获取数据。

```js
let colors = ['red', 'green', 'blue', 'yellow', 'white', 'black']
console.log(colors[0])
```

### 修改

通过下标获取数据。

```js
colors = ['red', 'green', 'blue', 'yellow', 'white', 'black']
colors[0] = 'lightpink'
```

### 添加

`push` 方法将一个或多个元素添加到数组的末尾，并返回该数组的新长度。

```js
let colors = ['red', 'green']
console.log(colors.push('blue', 'skyblue'))
console.log(colors)
```

`unshift` 方法将一个或多个元素添加到数组的开头，并返回该数组的新长度。

```js
let colors = ['red', 'green']
console.log(colors.unshift('pink', 'blue'))
console.log(colors)
```

### 删除

`pop` 方法从数组中删除最后一个元素，并返回该元素的值。

```js
let colors = ['red', 'green', 'blue', 'yellow', 'white', 'black']
color = colors.pop()
```

`shift` 方法从数组中删除第一个元素，并返回该元素的值。

```js
let colors = ['red', 'green', 'blue', 'yellow', 'white', 'black']
color = colors.shift()
```

`splice(start, deleteCount)`

* `start` 起始位置，指定修改的开始位置。
* `deleteCount` 可选值，表示要移除的数组元素的个数，如果省略则默认从指定的起始位置删除到最后。

```js
let colors = ['red', 'green', 'blue', 'yellow', 'white', 'black']
colors.splice(1, 2)
colors.splice(1)
```

### 遍历

> [!tip]
>
> 查找数组中的最大值

```js
let arr = [2, 6, 1, 77, 52, 25, 7];
let max = arr[0];
for (let i = 1; i < arr.length; i++) {
  if (max < arr[i]) {
    max = arr[i];
  }
}
```

## 综合案例

<img src="https://s1.ax1x.com/2023/04/24/p9m0TW8.jpg" style="zoom: 50%;" />

```html
<style>
    * {
        margin: 0;
        padding: 0;
    }

    .box {
        display: flex;
        width: 700px;
        height: 300px;
        border-left: 1px solid black;
        border-bottom: 1px solid black;
        margin: 50px auto;
        justify-content: space-around;
        align-items: flex-end;
        text-align: center;
    }

    .box>div {
        display: flex;
        width: 50px;
        background-color: skyblue;
        flex-direction: column;
        justify-content: space-between;
    }

    .box div span {
        margin-top: -20px;
    }

    .box div h4 {
        margin-bottom: -35px;
        width: 70px;
        margin-left: -10px;
    }
</style>
<script>
    let arr = [150, 100, 240, 200, 127, 280]
    document.write(`<div class="box">`)
    for (let i = 0; i < arr.length; i++) {
        document.write(`
             <div style="height: ${arr[i]}px;">
                <span>${arr[i]}</span>
                <h4>代理商${i + 1}</h4>
            </div>
        `)
    }
    document.write(`</div>`)
</script>
```

