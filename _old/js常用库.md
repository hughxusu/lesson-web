# jQuery

jQuery特性

* html元素选择器、html元素操作、css操作、html事件处理、js动画效果
* 链式调用、读写合一（读写函数名相同）、隐私遍历（对jQuery对象的操作，就是操作对象数组中的所有元素）
* 浏览器兼容、易扩展插件、ajax封装

```html
<html>
<head>
  <meta charset="UTF-8">
  <script type="text/javascript" src="js/jquery-1.12.3.js"></script>
  <!--远程引入-->
  <!--<script src="https://cdn.bootcss.com/jquery/1.12.4/jquery.js"></script>-->
  <script type="text/javascript">
    /*
    1. 使用jQuery核心函数: $/jQuery
    2. 使用jQuery核心对象: 执行$()返回的对象
    */
    jQuery(function () { // 绑定文档加载完成的监听
      var $btn2 = $('#btn2') // 执行jquery函数，返回jquery核心对象
      $btn2.click(function () { // 给btn2绑定点击监听
        var username = $('#username').val()
        alert(username)
      })
    })
  </script>
</head>
<body>
用户名: <input type="text" id="username">
<button id="btn1">确定(原生版)</button>
<button id="btn2">确定(jQuery版)</button>
</body>
</html>
```

## jQuery核心

jQuery核心函数

  * 简称：jQuery函数`$/jQuery`
  * jQuery库向外直接暴露的就是`$/jQuery`
  * 引入jQuery库后，直接使用`$`即可
    * 当函数用: `$(xxx)`，`$`是函数名
    * 当对象用: `$.xxx()`

jQuery核心对象

  * 简称：jQuery对象
  * 得到jQuery对象：执行jQuery函数返回的就是jQuery对象
  * 使用jQuery对象：`$obj.xxx()`

```javascript
console.log($, typeof $) // function
console.log(jQuery===$) // true
console.log($() instanceof Object) // true, 执行jQuery函数得到jQuery对象
```

### jQuery核心函数

作为一般函数调用：`$(param)`

1. 参数为函数：当DOM加载完成后，执行此回调函数

2. 参数为选择器字符串：查找所有匹配的标签，并将它们封装成jQuery对象

3. 参数为DOM对象：将dom对象封装成jQuery对象

4. 参数为html标签字符串：创建标签对象并封装成jQuery对象

作为对象使用：`$.xxx()`

1. `$.each()`：隐式遍历数组
2. `$.trim()`：去除两端的空格

```javascript
// 作为一般函数调用
$(function () { // 1.绑定文档加载完成的监听
    // 2.查找所有匹配的标签, 并将它们封装成jQuery对象
    $('#btn').click(function () {
        $(this).html() // 3.将dom对象封装成jQuery对象
        $('<input type="text" name="msg3"/><br/>') // 4.创建标签对象并封装成jQuery对象
    })
})

// 作为对象使用
var arr = [2, 4, 7]
$.each(arr, function (index, item) { // 隐式遍历数组
    console.log(index, item)
})
var str = ' my atguigu  '
console.log('---'+$.trim(str)+'---') // 去除两端的空格
```

### jQuery核心对象

jQuery对象性质：

* 执行jQuery核心函数返回jQuery对象。
* jQuery对象是一个包含所有匹配的任意多个（可能只有一个）dom元素的伪数组对象。
* jQuery对象拥有很多属性和方法，可以方便dom操作。

jQuery对象基本行为：

* size()/length：包含的DOM元素个数。
* [index]/get(index)：得到对应位置的DOM元素。
* each()：遍历包含的所有DOM元素。
* index()：得到在所在兄弟元素中的下标。

```javascript
// 包含的DOM元素个数
var $buttons = $('button')
console.log($buttons.size(), $buttons.length)

// 得到对应位置的DOM元素
console.log($buttons[1].innerHTML, $buttons.get(1).innerHTML)

// 遍历包含的所有DOM元素
$buttons.each(function(index, domEle) {
  console.log(index, domEle.innerHTML, this.innerHTML)
})

// 得到在所在兄弟元素中的下标*/
console.log($('#btn3').index())  // 2

/*
伪数组是用Object对象来代替数组
- length属性和数值下标属性
- 没有数组方法: forEach(), push(), pop(), splice()
*/
console.log($buttons instanceof Array) // false

// 自定义一个伪数组
var weiArr = {}
weiArr.length = 0
weiArr[0] = 'atguigu'
weiArr.length = 1
weiArr[1] = 123
weiArr.length = 2
for (var i = 0; i < weiArr.length; i++) {
    var obj = weiArr[i]
    console.log(i, obj)
}
```

## jQuery选择器

根据选择器的规则在整个文档中查找所有匹配的标签，并封装成jQuery对象返回。

### 基本选择器

有特定格式的字符串，来查找特定页面元素。

  - `#id`：id选择器
  - `element`：元素选择器
  - `.class`：属性选择器
  - `*`：任意标签
  - `selector1,selector2,selectorN`：取多个选择器的并集（组合选择器）
  - `selector1selector2selectorN`：取多个选择器的交集（相交选择器），不存在多个html标签的交。

```javascript
// 选择id为div1的元素
$('#div1')

// 选择所有的div元素
$('div')

// 选择所有class属性为box的元素
$('.box')

// 选择所有的div和span元素
$('div,span')

// 选择所有class属性为box的div元素，相当于div与.box的交
$('div.box')

$('*')
```

### 层次选择器

层次选择器：查找子元素，后代元素，兄弟元素的选择器。

```javascript
// 选中ul下所有的的span，在给定的祖先元素下匹配所有的后代元素
$('ul span')

// 选中ul下所有的子元素span，在给定的父元素下匹配所有的子元素
$('ul>span')

// 选中class为box的下一个li，匹配所有紧接在prev元素后的next元素
$('.box+li')

// 选中ul下的class为box的元素后面的所有兄弟元素，匹配prev元素之后的所有元素
$('ul .box~*')
```

### 过滤选择器

在原有选择器匹配的元素中进一步进行过滤的选择器。

```javascript
// 选择第一个div
$('div:first')

// 选择最后一个class为box的元素
$('.box:last')

// 选择第二个和第三个li元素
$('li:gt(0):lt(2)') 
$('li:lt(3):gt(0)') // 多个过滤选择器不是同时执行, 而是依次

// 选择所有class属性不为box的div
$('div:not(.box)')  // 没有class属性也可以

// 选择内容为BBBBB的li
$('li:contains("BBBBB")')

// 选择隐藏的li
console.log($('li:hidden').length, $('li:hidden')[0])

// 选择有title属性的li元素
$('li[title]')

// 选择所有属性title为hello的li元素
$('li[title="hello"]') // hello可以不加引号
```

### 表单选择器

表单选择器：1.表单；2.表单对象属性

```javascript
// 选择不可用的文本输入框
$(':text:disabled') // 以:开始

// 显示选择爱好 的个数
console.log($(':checkbox:checked').length)

// 显示选择的城市名称
$(':submit').click(function () {
    var city = $('select>option:selected').html() // 选择的option的标签体文本
    city = $('select').val()  // 选择的option的value属性值
    alert(city)
})
```

## 工具方法

1. `$.each()`：遍历数组或对象中的数据
2. `$.trim()`：去除字符串两边的空格
3. `$.type(obj)`：得到数据的类型
4. `$.isArray(obj)`：判断是否是数组
5. `$.isFunction(obj)`：判断是否是函数
6. `$.parseJSON(json)`：解析json字符串转换为js对象/数组

```javascript
// $.each(): 遍历数组或对象中的数据
var obj = {
    name: 'Tom',
    setName: function (name) {
        this.name = name
    }
}
$.each(obj, function (key, value) {
    console.log(key, value)
})

// $.type(obj): 得到数据的类型
console.log($.type($)) // 'function'

// $.isArray(obj): 判断是否是数组
console.log($.isArray($('body')), $.isArray([]))

// $.isFunction(obj): 判断是否是函数
console.log($.isFunction($))

// $.parseJSON(json) : 解析json字符串转换为js对象/数组
var json = '{"name":"Tom", "age":12}'
console.log($.parseJSON(json))

json = '[{"name":"Tom", "age":12}, {"name":"JACK", "age":13}]'
console.log($.parseJSON(json))

/*
JSON.parse(jsonString)   json字符串转js对象/数组
JSON.stringify(jsObj/jsArr)  js对象/数组转json字符串
*/
```

## 网页操作

### 属性操作

属性操作api

* 读写合一，读取属性和写入属性值，方法名一样。当个多个元素时读取的是最后一个元素值。
* 对jQuery对象的属性操作，就是操作对象数组中的所有元素的属性。
* 操作任意属性
  * `attr()`操作属性值为非布尔值的属性
  * `removeAttr()`移除属性值
  * `prop()`专门操作属性值为布尔值的属性
* 操作class属性
  * `addClass()`添加class值
  * `removeClass()`移除class值
* 操作HTML代码/文本/值
  * `html()`获得html代码
  * `val()`获得input的value值

```javascript
// 读取第一个div的title属性
console.log($('div:first').attr('title'))

// 给所有的div设置name属性(value为atguigu)
$('div').attr('name', 'atguigu')

// 移除所有div的title属性
$('div').removeAttr('title')

// 给所有的div设置class='guiguClass'
$('div').attr('class', 'guiguClass')

// 给所有的div添加class='abc'
$('div').addClass('abc')

// 移除所有div的guiguClass的class
$('div').removeClass('guiguClass')

// 得到最后一个li的标签体文本
console.log($('li:last').html())

// 设置第一个li的标签体为"<h1>mmmmmmmmm</h1>"
$('li:first').html('<h1>mmmmmmmmm</h1>')

// 得到输入框中的value值
console.log($(':text').val()) // 读取

// 将输入框的值设置为atguigu
$(':text').val('atguigu') // 写入，读写合一

// 点击'全选'按钮实现全选
var $checkboxs = $(':checkbox')
$('button:first').click(function () {
    $checkboxs.prop('checked', true)
})

// 点击'全不选'按钮实现全不选
$('button:last').click(function () {
    $checkboxs.prop('checked', false)
})
```

### CSS操作

设置css样式/读取css值`css()`，对jQuery对象css操作，就是操作对象数组中的所有元素的css。

```javascript
// 得到第一个p标签的颜色
console.log($('p:first').css('color'))

// 设置所有p标签的文本颜色为red
$('p').css('color', 'red')

// 设置第2个p的字体颜色(#ff0011),背景(blue),宽(300px), 高(30px)
$('p:eq(1)').css({
    color: '#ff0011',
    background: 'blue',
    width: 300,
    height: 30
})
```

### 位置和尺寸

#### 位置数据

`offset()` ：相对页面左上角的坐标。

`position()`：相对于父元素左上角的坐标。

```javascript
var offset = $('.div1').offset()
console.log(offset.left, offset.top)

$('.div2').offset({
    left: 50,
    top: 100
})

var position = $('.div1').position()
console.log(position.left, position.top)
```

#### 滚动

`scrollTop()`读取/设置滚动条的Y坐标

```javascript
// 得到div或页面滚动条的坐标
$('#btn1').click(function () {
    console.log($('div').scrollTop())
    console.log($(document.documentElement).scrollTop() + $(document.body).scrollTop()) // 兼容IE/Chrome
})

// 让div或页面的滚动条滚动到指定位置
$('#btn2').click(function () {
    $('div').scrollTop(200)
    $('html,body').scrollTop(300)
})
```

#### 尺寸

```javascript
var $div = $('div')
// 内容尺寸
console.log($div.width(), $div.height())
// 内部尺寸
console.log($div.innerWidth(), $div.innerHeight())
// 外部尺寸
console.log($div.outerWidth(), $div.outerHeight()) // height+padding+border  如果是true, 加上margin
console.log($div.outerWidth(true), $div.outerHeight(true)) // width+padding+border 如果是true, 加上margin
```

### 筛选

#### 过滤

在jQuery对象中的元素对象数组中过滤出一部分元素来。

```javascript
var $lis = $('ul>li')

// ul下li标签第一个
$lis.first().css('background', 'red')
$lis[0].style.background = 'red' // 返回dom元素对象

// ul下li标签的最后一个
$lis.last().css('background', 'red')

// ul下li标签的第二个
$lis.eq(1).css('background', 'red')

// ul下li标签中title属性为hello的
$lis.filter('[title=hello]').css('background', 'red')

// ul下li标签中title属性不为hello的
$lis.not('[title=hello]').css('background', 'red')
$lis.filter('[title!=hello]').filter('[title]').css('background', 'red')

// ul下li标签中有span子标签的
$lis.has('span').css('background', 'red')
```

#### 筛选

在已经匹配出的元素集合中根据选择器查找孩子/父母/兄弟标签

* children(): 子标签中找
* find() : 后代标签中找
3. parent() : 父标签
4. prevAll() : 前面所有的兄弟标签
5. nextAll() : 后面所有的兄弟标签
6. siblings() : 前后所有的兄弟标签

```javascript
var $ul = $('ul')

// ul标签的第2个span子标签
$ul.children('span:eq(1)').css('background', 'red')

// ul标签的第2个span后代标签
$ul.find('span:eq(1)').css('background', 'red')

// ul标签的父标签
$ul.parent().css('background', 'red')

// id为cc的li标签的前面的所有li标签
var $li = $('#cc')
$li.prevAll('li').css('background', 'red')

// id为cc的li标签的所有兄弟li标签
$li.siblings('li').css('background', 'red')
```

### 文档处理

添加/替换元素
* `append(content)` 向当前匹配的所有元素内部的最后插入指定内容
* `prepend(content)` 向当前匹配的所有元素内部的最前面插入指定内容
* `before(content)` 将指定内容插入到当前所有匹配元素的前面
* `after(content)` 将指定内容插入到当前所有匹配元素的后面替换节点
* `replaceWith(content)` 用指定内容替换所有匹配的标签删除节点

删除元素
* `empty()` 删除所有匹配元素的子元素
* `remove()` 删除所有匹配的元素

```javascript
// 向id为ul1的ul下添加一个span(最后)
var $ul1 = $('#ul1')
$ul1.append('<span>append()添加的span</span>')
$('<span>appendTo()添加的span</span>').appendTo($ul1)

// 向id为ul1的ul下添加一个span(最前)
$ul1.prepend('<span>prepend()添加的span</span>')
$('<span>prependTo()添加的span</span>').prependTo($ul1)

// 在id为ul1的ul下的li(title为hello)的前面添加span
$ul1.children('li[title=hello]').before('<span>before()添加的span</span>')

// 在id为ul1的ul下的li(title为hello)的后面添加span
$ul1.children('li[title=hello]').after('<span>after()添加的span</span>')

// 将在id为ul2的ul下的li(title为hello)全部替换为p
$('#ul2>li[title=hello]').replaceWith('<p>replaceAll()替换的p</p>')
// 移除id为ul2的ul下的所有li
$('#ul2').empty()  // <p>也会删除
$('#ul2>li').remove()
```



# Zepto

