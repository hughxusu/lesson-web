# BOM

BOM（Browser Object Model）是浏览器对象模型。

* window 是浏览器内置中的全局对象，我们所学习的所有 Web APIs 的知识内容都是基于 window 对象实现的
* window 对象下包含了 navigator、location、document、history、screen 5个属性，即所谓的 BOM （浏览器对象模型）
* document 是实现 DOM 的基础，它其实是依附于 window 的属性。
* 注：依附于 window 对象的所有属性和方法，使用时可以省略 window

## 定时器

## JS执行机制

## 浏览器基本操作

### location对象

### navigator对象

### histroy对象

## 本地存储

随着互联网的快速发展，基于网页的应用越来越普遍，同时也变的越来越复杂，为了满足各种各样的需求，会经常性在本地存储大量的数据，HTML5规范提出了相关解决方案。

* 数据存储在用户浏览器中
* 设置、读取方便、甚至页面刷新不丢失数据
* 容量较大，sessionStorage和localStorage约 5M 左右

### localStorage

1. 生命周期永久生效，除非手动删除 否则关闭页面也会存在
2. 可以多窗口（页面）共享（同一浏览器可以共享）
3. 以键值对的形式存储使用

### sessionStorage

