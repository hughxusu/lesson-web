# JavaScript 介绍

JavaScript 是一种运行在浏览器的编程语言，控制页面与用户的交互。JavaScript 是一门脚本语言。

![](https://s1.ax1x.com/2023/02/08/pSRy1b9.jpg)JavaScript 是由 Brendan Eich （布兰登·艾克） 在1995年推出。

<img src="https://cdn.vox-cdn.com/thumbor/bKFaEEWhT9n08enNW4KnB8DMIpw=/1400x1400/filters:format(jpeg)/cdn.vox-cdn.com/uploads/chorus_asset/file/15800415/brendan-eich-mozilla-firefox-square.0.1467740722.jpg" style="zoom: 33%;" />

JavaScript 作用

* 网页特效 （监听用户的一些行为让网页作出对应的反馈）
* 表单验证 （针对表单数据的合法性进行判断）
* 数据交互 （获取后台的数据, 渲染到前端）
* 服务端编程（node.js）

## 相关概念

### JavsScript 引擎

JavaScript 解析引擎简写为 JavaScript 引擎，用于对 JavaScript 的解析与执行。

V8 引擎是由 Google 公司使用 C++ 开发的一种高性能的 JavaScript 引擎，用于 Chrome 浏览器的 JavaScript 解析。V8 引擎是一款开源软件。

#### 浏览器中的 JavaScript 运行时环境

浏览器中的 JavaScript 运行时环境由浏览器提供的内置 API（WEB API）及 JavaScript 解析引擎组成。

![](https://s1.ax1x.com/2023/04/16/p99slHe.png)

1. Chorme 提供的 JavaScript 解析引擎 V8 负责解析和执行 JavaScript 代码。
2. 内置 API 是由运行环境（Chrome 浏览器）提供的编程接口。

###  JavaScript 组成 

<img src="https://s1.ax1x.com/2023/04/16/p99sc3q.jpg" style="zoom: 80%;" />

* ECMAScript
  * 规定了 js 基础语法核心知识，比如:变量、分支语句、循环语句、对象等等。
  * 1997 年，JavaScript 作为一个草案提交给欧洲计算机制造商协会（ECMA）。该协会制定了 ECMAScript 规范。
* Web APIs
  * DOM 操作文档，比如对页面元素进行移动、大小、添加删除等操作
  * BOM 操作浏览器，比如页面弹窗，检测窗口宽度、存储数据到浏览器等等

参考网站：[MDN](https://developer.mozilla.org/zh-CN/)

## JavaScript 在网页中的位置

 JavaScript 可以添加在 html 方式：

1. 内部 JavaScript

```html
<script>
  alert('hello, world')
</script>
```

> [!warning]
>
> 内联 JavaScript 需要写在 html 底部。

2. 外部 JavaScript

   * 定义 js 文件

   ```js
   alert('hello, world')
   ```

   * 在 html 引用 js 文件

   ```html
   <script src="./demo.js">
   </script>
   ```

> [!warning]
>
> script 标签中间代码会被忽略。

3. 内联  JavaScript

```html
<button onclick="alert('hello, world')">点击</button>
```



