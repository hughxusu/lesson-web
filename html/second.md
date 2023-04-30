# 高级HTML标签

## 列表标签

### 无序列表

在网页中表示一组无顺序之分的列表

```html
<ul>
  <li>香蕉</li>
  <li>苹果</li>
  <li>哈密瓜</li>
</ul>
```

> [!warning]
>
> `ul` 标签中只允许包含 `li` 标签，`li` 标签可以包含任意内容.

### 有序列表

在网页中表示一组有顺序之分的列表

```html
<ol>
  <li>香蕉</li>
  <li>苹果</li>
  <li>哈密瓜</li>
</ol>
```

### 自定义列表

含有标题的列表

```html
<dl>
    <dt>水果</dt>
    <dd>香蕉</dd>
    <dd>苹果</dd>
    <dd>哈密瓜</dd>
</dl>
```

> [!warning]
>
> `dl` 标签中只允许包含 `dt` 或 `dd` 标签，`dt` 或 `dd` 可以包含任意内容。
>
> 各种列表之间可以相互嵌套。

## 表格标签

标签

| 标签名    | 说明     |
| --------- | -------- |
| `table`   | 表格整体 |
| `tr`      | 表格行   |
| `td`      | 单元格   |
| `caption` | 表格标题 |
| th        | 表头     |
| thead     | 表格头部 |
| tbody     | 表格主体 |
| tfoot     | 表格底部 |

表格属性

* `border` 边框
* `width` / `height` 表格宽/高

案例 1

```html
<table border="1" width="300" height="100">
  <tr>
    <td>姓名</td>
    <td>平时成绩</td>
    <td>考试成绩</td>
  </tr>
  <tr>
    <td>张三</td>
    <td>70</td>
    <td>26</td>
  </tr>
  <tr>
    <td>李四</td>
    <td>68</td>
    <td>30</td>
  </tr>
</table>
```

案例 2

```html
<table border="1" width="300" height="100">
  <caption>成绩单</caption>
  <tr>
    <th>姓名</th>
    <th>平时成绩</th>
    <th>考试成绩</th>
  </tr>
  <tr>
    <td>张三</td>
    <td>70</td>
    <td>26</td>
  </tr>
  <tr>
    <td>李四</td>
    <td>68</td>
    <td>30</td>
  </tr>
</table>
```

案例 3

```html
<table border="1" width="300" height="100">
  <caption>成绩单</caption>
  <thead>
    <tr>
      <th>姓名</th>
      <th>平时成绩</th>
      <th>考试成绩</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>张三</td>
      <td>70</td>
      <td>26</td>
    </tr>
    <tr>
      <td>李四</td>
      <td>68</td>
      <td>30</td>
    </tr>
  </tbody>
  <tfoot>
    <td>
      平均
    </td>
    <td>
      69
    </td>
    <td>
      27
    </td>
  </tfoot>
</table>
```

#### 合并单元格

将水平或垂直多个单元格合并成一个单元格：跨行合并或跨列合并

合并单元格步骤：

1. 确定需要合并的单元格。
2. 通过左上原则，确定需要删除的单元格
   * 上下合并：只保留最上的，删除其它。
   * 左右合并：只保留最左的，删除其它。
3. 保留的单元格设置：
   * 跨行合并 `rowspan` 
   * 跨列合并 `colspan`

```html
<table border="1" width="300" height="100">
  <caption>成绩单</caption>
  <thead>
    <tr>
      <th>姓名</th>
      <th>平时成绩</th>
      <th>考试成绩</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>张三</td>
      <td rowspan="2">70</td>
      <td>26</td>
    </tr>
    <tr>
      <td>李四</td>
      <td>30</td>
    </tr>
  </tbody>
  <tfoot>
    <td>
      平均
    </td>
    <td colspan="2">
      69
    </td>
  </tfoot>
</table>
```

> [!warning]
>
> 只有同一个结构标签中的单元格才能合并，不能跨结构标签合并（thead、tbody、tfoot）

## 表单标签

用于收集用户信息

### `Input` 标签

通过 `type` 属性值修改标签的类型。

| type属性值 | 说明                         |
| ---------- | ---------------------------- |
| `text`     | 文本框，输入单行文本         |
| `password` | 密码框，用于输入密码         |
| `radio`    | 单选框                       |
| `checkbox` | 多选框                       |
| `file`     | 文件选择，用于上传文件       |
| `submit`   | 提交按钮                     |
| `reset`    | 重置按钮                     |
| `button`   | 普通按钮，需要配合 `js` 使用 |

案例

```html
<body>
用户名：<input type="text"> <br>
密码：<input type="password"> <br>
单选框：<input type="radio"> <br>
多选框: <input type="checkbox"> <br>
上传文件：<input type="file"> <br>
</body>
```

#### 文本框/密码

在网页中显示输入单行文本的表单控件

```html
<input type="text" placeholder="请输入用户名" value="张三" name="nick"> <br>
<input type="password" placeholder="请输入密码">
```

| 属性        | 说明                                         |
| ----------- | -------------------------------------------- |
| placeholder | 占位符，用于提示用于输入文本框内容。         |
| value       | 用户输入的内容，提交之后会发送给后端服务器。 |
| name        | 数据提交的关键字。                           |

密码框和文本框类型。

#### 单选框/复选框

在网页中显示多选一的单选表单控件

```html
性别: <input type="radio" name="gender">男 <input type="radio" name="gender" checked>女 <br>
<input type="checkbox" checked> 选项
```

| 属性    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| name    | 分组，有相同name属性值的单选框一组，一组同时只能有一个被选中 |
| checked | 默认选中                                                     |

#### 文件选择

在网页中显示文件选择的表单控件

```html
<input type="file" multiple>
```

| 属性     | 说明       |
| -------- | ---------- |
| multiple | 多文件选择 |

#### 按钮

```html
<form action="">
用户名: <input type="text">
<br>
密码: <input type="password">
<br>
<input type="submit" value="免费注册">
<input type="reset">
<input type="button" value="普通按钮">
</form>
```

| 属性   | 说明                         |
| ------ | ---------------------------- |
| submit | 提交按钮，用于提交数据。     |
| reset  | 重置，恢复表单的默认值。     |
| button | 不同按钮，需要培训 js 使用。 |

> [!warning]
>
> * 如果需要实现以上按钮功能，需要配合form标签使用。
> * form使用方法：用form标签把表单标签一起包裹起来即可。
> * action 提交后端的地址。

### `button` 标签

与 input 标签的按钮类型，可以为按钮添加文字。

```html
<button>按钮</button>
<button type="submit">提交按钮</button>
<button type="reset">重置按钮</button>
<button type="button">普通按钮</button>
```

### 下拉菜单

在网页中提供多个选择项的下拉菜单表单控件

* `select` 标签：下拉菜单的整体。
* `option` 标签：下拉菜单的每一项。

```html
<select>
<option selected>朝阳</option>
<option>海淀</option>
<option>石景山</option>
</select>
```

| 属性     | 说明                 |
| -------- | -------------------- |
| selected | 下拉菜单的默认选中项 |

### `textarea`

在网页中提供可输入多行文本的表单控件

```html
<textarea cols="60" rows="30"></textarea>
```

| 属性 | 说明                     |
| ---- | ------------------------ |
| cols | 规定了文本域内可见宽度。 |
| rows | 规定了文本域内可见行数。 |

> [!warning]
>
> 实际开发时针对于样式效果推荐用CSS设置。

### `label` 标签

常用于绑定内容与表单标签的关系

用法 1：

1. 使用label标签把内容(如:文本)包裹起来。
2. 在表单标签上添加id属性。
3. 在label标签的for属性中设置对应的id属性值。

用法 2：

1. 直接使用label标签把内容(如:文本)和表单标签一起包裹起来。
2. 需要把label标签的for属性删除即可。

```html
性别: 
<input type="radio" name="gender" id="man"> <label for="man">男</label>
<label><input type="radio" name="gender"> 女</label>
```

### 表单案例

<img src="https://s1.ax1x.com/2023/02/22/pSvDkAs.png" style="zoom:60%;" />

```html
用户名：<input type="text"> <br>
密码： <input type="password"> <br>
确认密码：<input type="password"> <br>
性别：<label><input type="radio" name="gender" checked>男</label> <label><input type="radio" name="gender">女</label> <br>
爱好：<label><input type="checkbox" checked>体育</label><label><input type="checkbox">旅游</label> <label><input type="checkbox">读书</label><br>
头像：<input type="file"><br>
地区：
<select>
<option>朝阳</option>
<option>海淀</option>
<option>石景山</option>
</select>
```

## 语义化标签

实际开发中用于布局的标签

* `div` 独占一行
* `span` 一行可以显示多个

<img src="https://s1.ax1x.com/2023/02/22/pSvr4sO.jpg" style="zoom:50%;" />

