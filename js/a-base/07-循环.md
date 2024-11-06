# 循环控制

> 日复一日，圈复一圈

## 断点调试

Chrome 打开调试界面在 sources 中设置代码断点。

<img src="https://s1.ax1x.com/2023/04/21/p9EsgTx.png" style="zoom:67%;" />

## `While` 循环

`while` 循环语法

```js
while (condition) { // 条件为 Ture 执行循环，条件为 False 退出循环
  statement_block
}
following_block
```

> [!note]
>
> 循环三要素：
>
> 1. 变量起始值。
> 2. 终止条件（没有终止条件，循环会一直执行，造成死循环）
> 3. 变化量（用自增或者自减）

`while` 循环流程图

<img src="https://cdn.programiz.com/sites/tutorial2program/files/javascript-while-loop.png" style="zoom:50%;" />

```js
// 1. 变量起始值
let i = 1
// 2. 终止条件
while (i <= 3) {
  document.write(`hello, world <br>`)
  // 3. 变化量
  i++
}
```

### 循环计数

利用循环进行统计计算，是循环在程序开发中的一个重要应用。

> [!tip]
>
> 计算0 ~ 100之间所有偶数求和。

```js
let i = 1
let sum = 0
while (i <= 100) {
  if (i % 2 === 0) {
    sum = sum + i
  }
  i++
}
document.write(`<h1>${sum}</h1>`)
```

### `break` 和 `continue`

`break` 和 `continue` 是专门在循环中使用的关键字：

- `break` 某一条件满足时，退出循环，不再执行后续重复的代码。
- `continue` 某一条件满足时，不执行后续重复的代码。

<img src="https://static.runoob.com/images/mix/python-while.webp" style="zoom: 50%;" />

#### `break`

```js
let i = 0
while (i < 5) {
  if (i === 3) {
    i++
    break
  }
  document.write(`当前序号为${i}`)
  i++
}
```

#### `continue`

```js
let i = 0
while (i < 5) {
  if (i === 3) {
    i++
    continue
  }
  document.write(`当前序号为${i}`)
  i++
}
```

> [!warning]
>
> `break` 和 `continue` 只针对当前所在循环有效。

### 综合练习

> [!tip]
>
> 需求：用户可以选择存钱、取钱、查看余额和退出功能。

```js
let money = 100
while (true) {
  let str = prompt(`请您选择操作:
  	1. 存钱
  	2. 取钱
  	3. 查看余额
    4. 退出
  `)

  if (str === '4') {
    break
  }

  switch (str) {
    case '1':
      let inMoney = +prompt('请您输入存钱的金额:')
      money += inMoney
      break
    case '2':
      let outMoeny = +prompt('请您输入取钱的金额:')
      money -= outMoeny
      break
    case '3':
      alert(`您卡上的余额是${money}元`)
      break
    default:
      alert(`输入选择错误！`)
      break
  }
}
```

## `for` 循环

`for` 循环语法

```js
for (init; condition; update) {
  statement_block
}
following_block
```

`for` 循环语法

<img src="https://cdn.programiz.com/sites/tutorial2program/files/javascript-for-loop.png" style="zoom: 50%;" />

```js
for (let i = 1; i <= 10; i++) {
  document.write(`hello, world <br>`)
}
```

循环计数

```js
let sum = 0  
for (let i = 1; i <= 100; i++) {
  if (i % 2 === 0) {
    sum += i
  }
}
document.write(`<h1>${sum}</h1>`)
```

### `break` 和 `continue`

`break` 和 `continue` 在 `for` 循环中使用。

#### `break`

```js
for (let i = 0; i < 5; i++) {
  if (i === 3) {
    break
  }
  document.write(`当前序号为${i}`)
}
```

#### `continue`

```js
for (let i = 0; i < 5; i++) {
  if (i === 3) {
    continue
  }
  document.write(`当前序号为${i}`)
}
```

> [!tip]
>
> [打印九九乘法表](https://jennifercodingworld.files.wordpress.com/2016/06/e4b998e6b395e8a1a8.jpeg)

```html
<style>
  div {
    display: inline-block;
    width: 96px;
    height: 25px;
    line-height: 25px;
    margin: 5px;
    padding: 0 10px;
    border: 1px solid black;
    color: black;
    border-radius: 5px;
    box-shadow: 2px 2px 2px rgba(0, 0, 0, .2);
    text-align: center;
  }
</style>
<script>
  for (let i = 1; i <= 9; i++) {
    for (let j = 1; j <= i; j++) {
      document.write(`<div> ${j} x ${i} = ${j * i} </div>`)
    }
    document.write('<br>')
  }
</script>
```

> [!note]
>
> 程序中的计数原则：
>
> - 自然计数法，从1开始计数。
> - 程序计数法从0开始计数。在编写程序时，应该尽量养成习，循环计数从0开始。