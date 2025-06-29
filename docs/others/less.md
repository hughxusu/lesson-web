# less

Less是一个CSS预处理器，Less文件后缀是 .less。扩充了 CSS 语言，使 CSS 具备一定的逻辑性、计算能力。

> [!warning]
>
> 浏览器不识别Less代码，网页只能引入对应的CSS文件。

Easy Less vscode插件，less文件保存自动生成css文件。

<img src="https://s1.ax1x.com/2023/04/15/p9pbAP0.png" style="zoom:80%;" />

## 语法

### 注释

* 单行注释 `//`
* 块注释 `/* */`

```less
// 单行注释

/* 
    块注释
    第二行
    第三行
*/
```

### 运算

less 的值可以使用四则运算，加、减、乘直接书写计算表达式，除法需要添加小括号或 . ，表达式存在多个单位以第一个单位为准。

```less
.box {
    width: 100 + 10px;
    height: 100 - 20px;
    margin: 12 * 2px;
    font-size: (32 / 2px);
    margin-top: 24 ./ 2px;
}
```

### 后代选择器

less 语法可以使用嵌套来实现后代选择器。

```less
.father {
    width: 100px;
    .son {
        color: pink;
    }
}
```

& 不生成后代选择器，表示当前选择器，通常配合伪类或伪元素使用。

```less
.father {
    width: 100px;
    .son {
        color: pink;
        &:hover {
            color: green;
        }
    }

    &:hover {
        color: orange;
    }
}
```

### 变量

可以统一定义属性值，在样式中使用。

1. 定义变量 `@name: value`
2. 使用变量 `property: @name`

```less
// 1. 定义变量
@primary: skyblue;

// 2. 使用变量
.box {
    color: @primary;
}

.father {
    background-color: @primary;
}

.aa {
    color: @primary;
}
```

### 导入其它样式

less 中可以引用其他 less 文件，使用 `@import path`

```less
@import './base.less';

.box {
    background-color: @primary;
}
```

### 导出设置

可以控制 less 文件导出的路径，使用注释来控制文件导出。

```less
// out: main.css

.box {
    color: red;
}
```

禁止导出文件

```less
// out: false

@primary: red;
```

