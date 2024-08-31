# js高级

## 执行上下文

代码按位置分类

  * 全局代码
  * 函数(局部)代码

全局执行上下文

1. 在执行全局代码前将window确定为全局执行上下文。

2. 对全局数据进行预处理。
   * var定义的全局变量赋值为undefined，添加为window的属性。
   * function声明的全局函数，添加为window的方法。
   * this赋值为window。

3. 开始执行全局代码。

函数执行上下文

1. 在调用函数，准备执行函数体之前，创建对应的函数执行上下文对象，存在于栈中。
2. 对局部数据进行预处理
   * 形参变量赋值给实参，添加为执行上下文的属性。
   * arguments赋值(实参列表)，添加为执行上下文的属性。
   * var定义的局部变量赋值为undefined，添加为执行上下文的属性。
   * function声明的函数，添加为执行上下文的方法。
   * this赋值为调用函数的对象。

3. 开始执行函数体代码

### 执行上下文栈

1. 在全局代码执行前，JS引擎就会创建一个栈来存储管理所有的执行上下文对象。
2. 在全局执行上下文(window)确定后，将其添加到栈中(压栈)。
3. 在函数执行上下文创建后，将其添加到栈中(压栈)。
4. 在当前函数执行完后，将栈顶的对象移除(出栈)。
5. 当所有的代码执行完后，栈中只剩下window。

![](https://upload-images.jianshu.io/upload_images/599584-58d31e5b80737ca0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 作用域与作用域链

作用域

  * 全局作用域
  * 函数作用域
  * 块作用域(ES6)

作用域链

  * 多个上下级关系的作用域形成的链, 它的方向是从下向上的(从内到外)

  * 查找变量时就是沿着作用域链来查找的

  * 查找一个变量的查找规则

    1. 在当前作用域下的执行上下文中查找对应的属性, 如果有直接返回, 否则进入2

    2. 在上一级作用域的执行上下文中查找对应的属性, 如果有直接返回, 否则进入3

    3. 再次执行2的相同操作, 直到全局作用域, 如果还找不到就抛出找不到的异常

![](https://images2015.cnblogs.com/blog/1013598/201609/1013598-20160910103037879-1633973943.gif)

## 闭包

一个函数与其相关的引用环境组合的一个整体(实体)

闭包产生的条件：

1. 函数嵌套。
2. 内部函数引用了外部函数的数据(变量/函数)。
3. 执行外部函数。

闭包的作用：

1. 使用函数内部的变量在函数执行完后，仍然存活在内存中。
2. 让函数外部可以操作到函数内部的数据。

闭包的生命周期：

* 产生：在嵌套内部函数定义执行完时就产生了(不是在调用)。
* 死亡：在嵌套的内部函数成为垃圾对象时。

==闭包如果不能及时释放会造成占用内存不释放。==

### 常见闭包

将函数作为另一个函数的返回值

```javascript
function fn1() {
    var a = 2
    function fn2() {
        a++
        console.log(a)
    }
    return fn2
}
var f = fn1()
f() // 3
f() // 4
f = null // 释放闭包资源
```

将函数作为实参传递给另一个函数调用

```javascript
function showDelay(msg, time) {
    setTimeout(function () {
        alert(msg)
    }, time)
}
showDelay('atguigu', 2000)
```

## 线程机制与事件机制

### 进程与线程

进程：程序的一次执行，它占有一片独有的内存空间

线程： CPU的基本调度单位，是程序执行的一个完整流程

进程与线程：

* 一个进程中一般至少有一个运行的线程：主线程。
* 一个进程中也可以同时运行多个线程，我们会说程序是多线程运行的。
* 一个进程内的数据可以供其中的多个线程直接共享。
* 多个进程之间的数据是不能直接共享的。

浏览器运行模式

* 单进程：firefox、老版IE
* 多进程：chrome、新版IE

5. 浏览器都是多线程运行

### 浏览器内核

浏览器内核：支持浏览器运行的最核心的程序。

不同的浏览器内核：

* Chrome、Safari：webkit
* firefox：Gecko
* IE：Trident
* 360、搜狗等国内浏览器：Trident + webkit

内核由很多模块组成：

* 主线程模块：
  * js引擎模块：负责js的编译和运行
  * html/css文档解析模块：负责页面文本的解析
  * dom/css模块：负责dom/css在内存中的相关处理
  * 布局和渲染模块：负责页面的布局和效果的绘制
* 分线程模块：
  * 定时器模块：负责定时器的管理
  * 网络请求模块：负责服务器请求(常规/Ajax)
  * 事件响应模块：负责事件的管理

#### js执行模式

JavaScript作为浏览器脚本语言，主要用途是与用户互动，以及操作DOM。这决定了它只能是单线程，否则会带来很复杂的同步问题。JavaScript代码的分类：

  * 初始化代码
  * 回调代码

js引擎执行代码的基本流程：

1. 先执行初始化代码：包含一些特别的代码、回调函数（异步执行）
   * 设置定时器
   * 绑定事件监听
   * 发送ajax请求

2. 后面在某个时刻才会执行回调代码

#### 定时器

1. 定时器并不能保证真正定时执行，一般会有一点延迟（可接受），也有可能延迟很长时间（不能接受）

2. 定时器回调函数是在主线程执行的，js是单线程。

### 事件循环模型

<img src="https://pic4.zhimg.com/80/v2-64476c110e4efcd85df76dc49b510abb_1440w.jpg" style="zoom:45%;" />

代码分类：

  * 初始化执行代码（同步代码）：包含绑定dom事件监听，设置定时器，发送ajax请求的代码。
  * 回调执行代码（异步代码）：处理回调逻辑

js引擎执行代码的基本流程：初始化代码 => 回调代码

模型的2个重要组成部分：

  * 事件（定时器/DOM事件/Ajax）管理模块
  * 回调队列

模型的运转流程

1. 执行初始化代码，将事件回调函数交给对应模块管理。
2. 当事件发生时，管理模块会将回调函数及其数据添加到回调列队中。
3. 只有当初始化代码执行完后（可能要一定时间），才会遍历读取回调队列中的回调函数执行。

### Web Workers

H5规范提供了js分线程的实现，Web Workers

<img src="http://p1.meituan.net/kuailv/1aae5f5a2fd5220d14bf377ffd82937840137.png" style="zoom:90%;" />

相关API

1. Worker：构造函数, 加载分线程执行的js文件
2. Worker.prototype.onmessage：用于接收另一个线程的回调函数。
3. Worker.prototype.postMessage：向另一个线程发送消息。

主线程处理函数

```javascript
var input = document.getElementById('number')
document.getElementById('btn').onclick = function () {
    var number = input.value
    var worker = new Worker('worker.js') // 创建一个Worker对象
    
    worker.onmessage = function (event) { // 绑定接收消息的监听
        alert(event.data)
    }
    worker.postMessage(number) // 向分线程发送消息
}
```

分线程处理函数

```javascript
function fibonacci(n) {
  return n<=2 ? 1 : fibonacci(n-1) + fibonacci(n-2)  // 递归调用
}

this.onmessage = function (event) {
  var number = event.data // 分线程接收到主线程发送的数据
  var result = fibonacci(number)
  postMessage(result) // 分线程向主线程返回数据
}
```

## DOM

### 入门

DOM，全称Document Object Model文档对象模型。JS中通过DOM来对HTML文档进行操作。

* 文档：文档表示的就是整个的HTML网页文档

- 对象：对象表示将网页中的每一个部分都转换为了一个对象。
- 模型：使用模型来表示对象之间的关系，这样方便我们获取对象。

通过文档对象构成的整体树状结构模型就是文档对象模型，其中的每一个节点都可以用来操作文档。

![](https://upload.wikimedia.org/wikipedia/commons/5/5a/DOM-model.svg)

#### 节点

节点Node，是构成我们网页的最基本的组成部分，网页中的 每一个部分都可以称为是一个节点。比如:html标签、属性、文本、注释、整个文档等都是一个节点。

虽然都是节点，但是实际上他们的具体类型是不同的。比如:标签我们称为元素节点、属性称为属性节点、文本称为文本节点、文档称为文档节点。

节点的类型不同，属性和方法也都不尽相同。

常用节点分为四类

* 文档节点：整个HTML文档
* 元素节点：HTML文档中的HTML标签 
* 属性节点：元素的属性
* 文本节点：HTML标签中的文本内容

|          | nodeName  | nodeType | nodeValue |
| -------- | --------- | -------- | --------- |
| 文档节点 | #document | 9        | null      |
| 元素节点 | 标签名    | 1        | null      |
| 属性节点 | 属性值    | 2        | 属性值    |
| 文本节点 | #text     | 3        | 文本内容  |

#### 示例

```javascript
/*
* 浏览器已经为我们提供 文档节点 对象这个对象是window属性
* 	可以在页面中直接使用，文档节点代表的是整个网页
*/
//console.log(document);

//获取到button对象
var btn = document.getElementById("btn");

//修改按钮的文字
btn.innerHTML = "I'm Button";
```

### 文档加载

```html
<html>
<head>
    <meta charset="UTF-8">
    <script type="text/javascript">
        /*
         * 浏览器在加载一个页面时，按照自上向下顺序加载
         * 如果将script标签写到页面的上边，在代码执行时，页面还没有加载
         * 页面中DOM对象也没有加载，会导致无法获取到DOM对象
         *
         * onload事件在整个页面加载完成之后才触发，
         * 可以确保代码执行时所有DOM对象均已加载完毕
         */
        window.onload = function(){
            //获取id为btn的按钮
            var btn = document.getElementById("btn");
            //为按钮绑定一个单击响应函数
            btn.onclick = function(){
                alert("hello");
            };
        };
    </script>
</head>
<body>
    <button id="btn">点我一下</button>
</body>
</html>
```

### 事件

事件指的就是 用户与浏览器交互的一瞬间。通过为指定事件绑定回调函数的形式来处理事件，当指定事件触发以后回调函数就会被调 用，这样页面就可以完成和用户的交互了。

```html
<html>
	<body>
		<!--在事件对应的属性中设置一些js代码-->
		<button id="btn1" onmousemove="alert('讨厌，你点我干嘛！');">我是一个按钮</button>
		<button id="btn">我是一个按钮</button>
		<script type="text/javascript">
			// 获取按钮对象
			var btn = document.getElementById("btn");
			
			//绑定一个单击事件
			btn.onclick = function(){
				alert("你还点~~~");
			};
		</script>
	</body>
</html>
```

#### 事件对象

* 当事件的响应函数被触发时，浏览器每次都会将一个事件对象作为实参传递进响应函数。
* 在事件对象中封装了当前事件相关的一切信息，如：鼠标的坐标 、按键信息、 鼠标滚动方向等

```javascript
window.onload = function(){
    var areaDiv = document.getElementById("areaDiv");

    // 会在鼠标在元素中移动时被触发
    areaDiv.onmousemove = function(event){

        // 在IE8及以下的浏览器中，是将事件对象作为window对象的属性保存的
        event = event || window.event;

        // clientX可以获取鼠标指针的水平坐标
        var x = event.clientX;
        // clientY可以获取鼠标指针的垂直坐标
        var y = event.clientY;
    };
};
```

#### 事件的绑定

```javascript
window.onload = function(){
    var btn01 = document.getElementById("btn01");

    /*
     * 	只能为一个元素的一个事件绑定一个响应函数，不能绑定多个，
     * 	如果绑定了多个，则后边会覆盖掉前边
     */
    btn01.onclick = function(){
        alert(1);
    };

    /*
     * addEventListener() 可以为元素绑定响应函数
     * 参数：
     * 1. 事件的字符串，不要on
     * 2. 回调函数，当事件触发时该函数会被调用
     * 3. 是否在捕获阶段触发事件，需要一个布尔值，一般都传false
     *
     * 可以同时为一个元素的相同事件同时绑定多个响应函数，
     * 当事件被触发时，响应函数将会按照函数的绑定顺序执行
     */
    btn01.addEventListener("click",function() {
        alert(1);
    },false);

    btn01.addEventListener("click",function() {
        alert(2);
    },false);

    /*
     * attachEvent() 在IE8中使用
     * 参数：
     * 1. 事件的字符串，要on
     * 2. 回调函数
     *
     * 可以绑定多个处理函数，执行顺序和addEventListener()相反
     */
    btn01.attachEvent("onclick",function() {
        alert(1);
    });

    btn01.attachEvent("onclick",function() {
        alert(2);
    });
};
```

#### 事件的传播

<img src="https://zh.javascript.info/article/bubbling-and-capturing/eventflow.svg" style="zoom:85%;" />

将事件传播分成了三个阶段：

1. 捕获阶段：在捕获阶段时从最外层的祖先元素，向目标元素进行事件的捕获，但是默认此时不会触发事件。
2. 目标阶段：事件捕获到目标元素，捕获结束开始在目标元素上触发事件
3. 冒泡阶段：事件从目标元素向他的祖先元素传递，依次触发祖先元素上的事件

如果希望在捕获阶段就触发事件，可以将addEventListener()的第三个参数设置为true，一般情况下我们不会希望在捕获阶段触发事件，所以这个参数一般都是false

```javascript
window.onload = function(){
    var box1 = document.getElementById("box1");
    var box2 = document.getElementById("box2");

    bind(box1,"click",function(){
        alert("我是box1的响应函数")
    });
    bind(box2,"click",function(){
        alert("我是box2的响应函数")
    });
};

function bind(obj , eventStr , callback){
    if(obj.addEventListener){
        // this是绑定事件的对象，可以直接传递
        obj.addEventListener(eventStr , callback , true); // 捕获阶段就触发事件
    }else{

        // this是window
        obj.attachEvent("on"+eventStr , function(){
            /*
             * 在匿名函数中调用回调函数
             * this由调用方式决定(callback.call(obj))
             */
            callback.call(obj);
        });
    }
}
```

##### 事件冒泡

* 冒泡就是事件的向上传导，当后代元素上的事件被触发时，其祖先元素的相同事件也会被触发
* 在开发中大部分情况冒泡都是有用的，如果不希望发生事件冒泡可以通过事件对象来取消冒泡

```javascript
box.onmousemove = function(event) {
  event.cancleBubble = true; // 通过event事件取消冒泡，只阻止当前层
  
  event.stopPropagation() // 取消事件冒泡，阻止所有层
  event.preventDefault()	// 阻止默认事件
}
```

##### 事件的委派

* 指将事件统一绑定给元素的共同的祖先元素，这样当后代元素上的事件触发时，会一直冒泡到祖先元素从而通过祖先元素的响应函数来处理事件。
* 事件委派是利用了冒泡，通过委派可以减少事件绑定的次数，提高程序的性能。

```html
<html>
<head>
    <meta charset="utf-8" />
    <script type="text/javascript">
        window.onload = function(){
            var u1 = document.getElementById("u1");

            // 为ul绑定一个单击响应函数，子元素li可以相应
            u1.onclick = function(event){
                event = event || window.event;
                // 表示的触发事件的对象
                if(event.target.className == "link"){
                    alert("我是ul的单击响应函数");
                }
            };
        };
    </script>
</head>
<body>

<ul id="u1" style="background-color: #bfa;">
    <li>
        <p>我是p元素</p>
    </li>
    <li><a href="javascript:;" class="link">超链接一</a></li>
    <li><a href="javascript:;" class="link">超链接二</a></li>
    <li><a href="javascript:;" class="link">超链接三</a></li>
</ul>
</body>
</html>
```

#### 事件处理

##### 拖拽

拖拽的流程：

1. 当鼠标在被拖拽元素上按下时，开始拖拽 onmousedown
2. 当鼠标移动时被拖拽元素跟随鼠标移动 onmousemove
3. 当鼠标松开时，被拖拽元素固定在当前位置 onmouseup

```html
<html>
<head>
    <meta charset="UTF-8">
    <style type="text/css">
        #box1{
            width: 100px;
            height: 100px;
            background-color: red;
            position: absolute;
        }
    </style>
    <script type="text/javascript">
        window.onload = function() {
            var box1 = document.getElementById("box1");
            drag(box1);
        };

        function drag(obj) {
            // 为拖动对象绑定鼠标点击
            obj.onmousedown = function(event) {
                // IE支持
                obj.setCapture && obj.setCapture();
                event = event || window.event;

                // 鼠标点击相对div的偏移量
                var ol = event.clientX - obj.offsetLeft;
                var ot = event.clientY - obj.offsetTop;

                // 将onmousemove事件绑定到document
                document.onmousemove = function(event){
                    event = event || window.event;
                    var left = event.clientX - ol;
                    var top = event.clientY - ot;

                    obj.style.left = left+"px";
                    obj.style.top = top+"px";
                };

                // 为document绑定一个鼠标松开事件
                document.onmouseup = function(){
                    // 鼠标松开时，取消document的onmousemove事件和onmouseup事件
                    document.onmousemove = null;
                    document.onmouseup = null;
                    // 当鼠标松开时，取消对事件的捕获
                    obj.releaseCapture && obj.releaseCapture();
                };

                // return false来取消默认行为，IE8无效
                return false;
            };
        }
    </script>
</head>
<body>
我是一段文字

<div id="box1"></div>
</body>
</html>
```

##### 滚轮事件

```html
<html>
<head>
    <meta charset="UTF-8">
    <style type="text/css">
        #box1{
            width: 100px;
            height: 100px;
            background-color: red;
        }
    </style>
    <script type="text/javascript">
        window.onload = function() {
            var box1 = document.getElementById("box1");

            /*
             * 绑定一个鼠标滚轮滚动的事件
             * onmousewheel鼠标滚轮滚动的事件，会在滚轮滚动时触发
             *
             * 在火狐中需要使用DOMMouseScroll来绑定滚动事件
             * 注意该事件需要通过addEventListener()函数来绑定
             */
            box1.onmousewheel = function(event) {
                event = event || window.event;

                // 判断鼠标滚轮滚动的方向event.detail火狐支持
                if(event.wheelDelta > 0 || event.detail < 0) {
                    box1.style.height = box1.clientHeight - 10 + "px";ß
                } else {
                    box1.style.height = box1.clientHeight + 10 + "px";
                }

                // IE8支持
                event.preventDefault && event.preventDefault();

                // 取消默认行为
                return false;
            };

            // 为火狐绑定滚轮事件
            bind(box1, "DOMMouseScroll", box1.onmousewheel);
        };

        function bind(obj, eventStr, callback) {
            if (obj.addEventListener) {
                // 大部分浏览器兼容的方式
                obj.addEventListener(eventStr, callback , false);
            } else {
                // IE8及以下
                obj.attachEvent("on"+eventStr, function() {
                    // 在匿名函数中调用回调函数
                    callback.call(obj);
                });
            }
        }
    </script>
</head>
<body style="height: 2000px;">

<div id="box1"></div>
</body>
</html>
```

##### 键盘事件

键盘事件一般都会绑定给一些可以获取到焦点的对象或者是document

```javascript
window.onload = function() {
    /*
     * 键盘事件：
     * 	onkeydown 按键被按下
     *  - onkeydown按键不松，事件会一直触发
     * 	- 第一次和第二次之间会间隔稍微长一点，其他的会非常的快
     *
     * 	onkeyup
     * 	- 按键被松开
     */
    document.onkeydown = function(event) {
        event = event || window.event;

        /*
         * altKey、ctrlKey、shiftKey
         * 这个三个用来判断alt、ctrl、shift是否被按下，
         * 按下则返回true，否则返回false
         */
        if(event.keyCode === 89 && event.ctrlKey){
            console.log("ctrl和y都被按下了");
        }
    };

    document.onkeyup = function(){
        console.log("按键松开了");
    };
};
```

### DOM查询

#### 基本方法

获取元素节点对象，通过document对象调用 

* `getElementById()`通过id属性获取一个元素节点对象 
* `getElementsByTagName()`通过标签名获取一组元素节点对象
* `getElementsByName()`通过name属性获取一组元素节点对象

获取元素节点的子节点，通过具体的元素节点调用

* `getElementsByTagName()`方法，返回当前节点的指定标签名后代节点 
* `childNodes`属性，表示当前节点的所有子节点
* `firstChild`属性，表示当前节点的第一个子节点
* `lastChild`属性，表示当前节点的最后一个子节点

获取父节点和兄弟节点，通过具体的节点调用

* `parentNode`属性，表示当前节点的父节点
* `previousSibling`属性，表示当前节点的前一个兄弟节点
* `nextSibling`属性，表示当前节点的后一个兄弟节点

元素节点的属性获取

* 元素对象.属性名 
  * `element.value`
  * `element.id`
  * `element.className`

* 设置，元素对象.属性名=新的值 
  * `element.value = “hello”`
  * `element.id = “id01”` 
  * `element.className = “newClass"`

其他属性

* `nodeValue`属性获取和设置文本节点的内容
* `innerHTML`元素节点通过该属性获取和设置标签内部的html代码

```javascript
var btn01 = document.getElementById("btn01");
var lis = document.getElementsByTagName("li");
var inputs = document.getElementsByName("gender");

var city = document.getElementById("city");
var cns = city.childNodes;
var cns2 = city.children;

var phone = document.getElementById("phone");
var fir = phone.firstChild;

var bj = document.getElementById("bj");
var pn = bj.parentNode;

var and = document.getElementById("android");
var ps = and.previousSibling;

var um = document.getElementById("username");
var value = um.value
```

#### 其他方法

使用CSS选择器进行查询

- `querySelector()`
- `querySelectorAll()`

这两个方法都是用document对象来调用，两个方法使用相同， 都是传递一个选择器字符串作为参数，方法会自动根据选择器 字符串去网页中查找元素。不同的地方是`querySelector()`只会返回找到的第一个元素，而`querySelectorAll()`会返回所有符合条件的元素。

```javascript
var div = document.querySelector(".box1 div");
var box1 = document.querySelectorAll(".box1");
```

### DOM修改

节点的修改

* 创建节点：document.createElement(标签名)
* x删除节点：父节点.removeChild(子节点)
* 替换节点：父节点.replaceChild(新节点 , 旧节点)

插入节点

* 父节点.appendChild(子节点)
* 父节点.insertBefore(新节点 , 旧节点)

```javascript
var li = document.createElement("li");
var gzText = document.createTextNode("广州");

li.appendChild(gzText);
city.insertBefore(li , bj);
city.replaceChild(li , bj);
bj.parentNode.removeChild(bj);

// 设置#bj内的HTML代码
bj.innerHTML = "昌平";
```

### DOM操作CSS

语法：元素.style.样式名 = 样式值

```javascript
/*
 * 如果CSS的样式名中含有-，需要将这种样式名修改为驼峰命名法
 * 去掉-，然后将-后的字母大写
 *
 * 通过style属性设置的样式都是内联样式，内联样式有较高的优先级
 * 所以通过JS修改的样式往往会立即显示
 *
 * 如果在样式中写了!important，则此时样式会有最高的优先级，
 * 即使通过JS也不能覆盖该样式，此时将会导致JS修改样式失效
 */
box1.style.width = "300px";
box1.style.height = "300px";
box1.style.backgroundColor = "yellow";
```

#### 读取样式表

```javascript
/*
 * getComputedStyle()
 * 获取元素当前的样式，window的方法，直接使用
 * 参数
 * 1. 要获取样式的元素
 * 2. 可以传递一个伪元素，一般都传null
 *
 * 该方法会返回一个对象，对象中封装了当前元素对应的样式
 * 可以通过对象.样式名来读取样式
 * 如果样式没有设置，则会获取到真实的值，
 * 比如：没有设置width，它不会获取到auto，而是一个长度
 *
 * 但是该方法不支持IE8及以下的浏览器
 *
 * 读取到的样式不能修改
 */
var obj = getComputedStyle(box1, null);
```

#### 其他样式操作

属性读取返回是一个数字（不带px），可以直接进行计算

```javascript
/*
 * clientWidth
 * clientHeight
 * - 这两个属性可以获取元素的可见宽度和高度
 * - 会获取元素宽度和高度，包括内容区和内边距
 * - 这些属性都是只读的，不能修改
 */
alert(box1.clientWidth);
alert(box1.clientHeight);

/*
 * offsetWidth
 * offsetHeight
 * - 获取元素的整个的宽度和高度，包括内容区、内边距和边框
 */
alert(box1.offsetWidth);

/*
 * offsetParent
 * - 可以用来获取当前元素的定位父元素
 * - 会获取到离当前元素最近的开启了定位的祖先元素
 *   如果所有的祖先元素都没有开启定位，则返回body
 */
var op = box1.offsetParent;

/*
 * offsetLeft
 * - 当前元素相对于其定位父元素的水平偏移量
 * offsetTop
 * - 当前元素相对于其定位父元素的垂直偏移量
 */
alert(box1.offsetLeft);

/*
 * scrollWidth
 * scrollHeight
 * - 可以获取元素整个滚动区域的宽度和高度
 */
alert(box4.clientHeight);
alert(box4.scrollWidth);

/*
 * scrollLeft
 * - 可以获取水平滚动条滚动的距离
 * scrollTop
 * - 可以获取垂直滚动条滚动的距离
 */
alert(box4.scrollLeft);
alert(box4.scrollTop);

alert(box4.clientHeight);

// 当满足scrollHeight - scrollTop == clientHeight，说明垂直滚动条滚动到底了
// 当满足scrollWidth - scrollLeft == clientWidth，说明水平滚动条滚动到底
```

#### 类的操作

```html
<html>
<head>
    <meta charset="UTF-8">
    <style type="text/css">
        .b1{
            width: 100px;
            height: 100px;
            background-color: red;
        }

        .b2{
            height: 300px;
            background-color: yellow;
        }
    </style>
    <script type="text/javascript">
        window.onload = function() {
            var box = document.getElementById("box");
            var btn01 = document.getElementById("btn01");

            btn01.onclick = function() {
                toggleClass(box,"b2");
            };
        };

        /*
         * 向元素中添加指定的class属性值
         * - obj 要添加class属性的元素
         * - cn 要添加的class值
         */
        function addClass(obj , cn) {
            if (!hasClass(obj , cn)) {
                // class属性来间接的修改样式
                obj.className += " " + cn;
            }
        }

        // 判断元素中是否含有指定的class属性，存在返回true，否则返回false
        function hasClass(obj , cn){
            var reg = new RegExp("\\b" + cn + "\\b");
            return reg.test(obj.className);
        }

        // 删除一个元素中的指定的class属性
        function removeClass(obj , cn){
            var reg = new RegExp("\\b" + cn + "\\b");
            obj.className = obj.className.replace(reg , "");
        }

        // toggleClass可以用来切换一个类
        function toggleClass(obj , cn){
            if (hasClass(obj , cn)) {
                removeClass(obj , cn); // 如果元素中具有该类，则删除
            } else {
                addClass(obj , cn); // 如果元素中没有该类，则添加
            }
        }

    </script>
</head>
<body>
<button id="btn01">点击按钮以后修改box的样式</button>

<br /><br />

<div id="box" class="b1 b2"></div>
</body>
</html>
```

## BOM

BOM浏览器对象模型，在BOM中提供了一组对象，用来完成对浏览器的操作。

1. Window代表的是整个浏览器的窗口，同时window也是网页中的全局对象。
2. Navigator代表的当前浏览器的信息，通过该对象可以来识别不同的浏览器。
3. Location代表当前浏览器的地址栏信息，通过Location可以获取地址栏信息，或者操作浏览器跳转页面。
4. History代表浏览器的历史记录，可以通过该对象来操作浏览器的历史记录。由于隐私原因，该对象不能获取到具体的历史记录，只能操作浏览器向前或向后翻页，而且该操作只在当次访问时有效。
5. Screen代表用户的屏幕的信息，通过该对象可以获取到用户的显示器的相关的信息。

这些BOM对象在浏览器中都是作为window对象的属性保存的，可以通过window对象来使用，也可以直接使用。

```javascript
console.log(window.navigator);
console.log(navigator);
console.log(location);
console.log(history);
```

### Navigator

代表的当前浏览器的信息，通过该对象可以来识别不同的浏览器。一般我们只会使用userAgent来判断浏览器的信息，这个字符串中包含有用来描述浏览器信息的内容。由于历史原因，Navigato对象中的其他大部分属性都已经不能辅助识别浏览器。

```javascript
var ua = navigator.userAgent;
console.log(ua);

if(/firefox/i.test(ua)) { // 正则表达式
    alert("你是火狐！！！");
} else if (/chrome/i.test(ua)) {
    alert("你是Chrome");
} else if (/msie/i.test(ua)) {
    alert("你是IE浏览器~~~");
} else if ("ActiveXObject" in window) {
    alert("你是IE11，枪毙了你~~~");
}
```

### History

可以用来操作浏览器向前或向后翻页

```javascript
// 可以获取当前访问的链接数量
alert(history.length);

// 回退到上一个页面，作用和浏览器的回退按钮一样
history.back();

// 跳转下一个页面，作用和浏览器的前进按钮一样
history.forward();

/*
 * go() 可以用来跳转到指定的页面
 * 1:表示向前跳转一个页面 相当于forward()
 * 2:表示向前跳转两个页面
 * -1:表示向后跳转一个页面
 * -2:表示向后跳转两个页面
 */
history.go(-2);
```

### Location

封装了浏览器的地址栏的信息

```javascript
// 自动跳转到该路径，并且会生成相应的历史记录
location = "http://www.baidu.com";

// 跳转页面，作用和直接修改location一样
location.assign("http://www.baidu.com");

/*
 * 用于重新加载当前页面，作用和刷新按钮一样
 * 如果在方法中传递一个true，会强制清空缓存刷新页面
 */
location.reload(true);

/*
 * 使用一个新的页面替换当前页面，调用完毕也会跳转页面
 * 不会生成历史记录，不能使用回退按钮回退
 */
location.replace("01.BOM.html");
```

### Window

#### 定时调用

```javascript
window.onload = function() {
    var count = document.getElementById("count");

    /*
     * setInterval() 定时器
     * 参数：
     * 1. 回调函数，该函数会每隔一段时间被调用一次
     * 2. 每次调用间隔的时间，单位是毫秒
     *
     * 返回值：
     * 返回一个Number类型的数据，作为定时器的唯一标识
     */
    var num = 1;
    var timer = setInterval(function() {
        count.innerHTML = num++;
        if(num == 11){
            // 关闭定时器
            clearInterval(timer);
        }

    },1000);
};
```

#### 延时调用

```javascript
// 延时调用
var timer = setTimeout(function(){
    console.log(num++);
},3000);

// 使用clearTimeout()来关闭一个延时调用
clearTimeout(timer);
```

## Json对象

```javascript
var json = '{"name":"孙悟空","age":18,"gender":"男"}';
var arr ='[{"name":"孙悟空","age":18,"gender":"男"},{"name":"孙悟空","age":18,"gender":"男"}]';

// 可以将以JSON字符串转换为js对象
var obj = JSON.parse(json);
var list = JSON.parse(arr);

var obj3 = {name:"猪八戒" , age:28 , gender:"男"};

// JS对象转换为JSON字符串
var str = JSON.stringify(obj3);
```

### eval处理Json对象

```javascript
var str = '{"name":"孙悟空","age":18,"gender":"男"}';

/*
 * eval()
 * - 这个函数可以用来执行一段字符串形式的JS代码，并将执行结果返回
 * - 如果使用eval()执行的字符串中含有{},它会将{}当成是代码块
 * 	 如果不希望将其当成代码块解析，则需要在字符串前后各加一个()
 * - 字符串转换为对象
 */
var obj = eval("(" + str + ")");
```

# ES规范

ES是由ECMA组织制定和发布的脚本语言规范， JavaScript是ECMA的实现。JS包含三个部分：

* ECMAScript（核心）
* 浏览器端扩展：BOM（浏览器对象模型）、DOM（文档对象模型）
* 服务器端扩展：Node

## ES5

### 严格模式

这种模式使得Javascript在更严格的语法条件下运行，提升代码的严谨性和安全性。使用：在全局或函数的第一条语句定义为：`'use strict'`

语法和行为改变：
* 必须用var声明变量
* 禁止自定义的函数中的this指向window
* 创建eval作用域
* 对象不能有重名的属性

### Json对象

```javascript
var obj = {
    name : 'kobe',
    age : 39
};
obj = JSON.stringify(obj); // js对象(数组)转换为json对象(数组)
obj = JSON.parse(obj); // json对象(数组)转换为js对象(数组)
```

### Object扩展

```javascript
var obj = {name : 'curry', age : 29}
// 以指定对象为原型创建新的对象
obj1 = Object.create(obj, { 
    sex : { // 为新的对象指定新的属性, 并对属性进行描述
        value : '男', // 指定值
        writable : true, // 标识当前属性值是否是可修改的, 默认为false
        configurable: true, // 标识当前属性是否可以被删除 默认为false
      	enumerable: true // 标识当前属性是否能用for in 枚举 默认为falsed
    }
});


var obj = {firstName: 'curry', lastName: 'stephen'};
// 为指定对象定义扩展多个属性
Object.defineProperties(obj2, {
    fullName : {
        get : function () { // 用来获取当前属性值得回调函数
            return this.firstName + '-' + this.lastName
        },
        set : function (data) { // 修改当前属性值触发的回调函数
            var names = data.split('-');
            this.firstName = names[0];
            this.lastName = names[1];
        }
    }
});

var obj = {
    firstName : 'kobe',
    lastName : 'bryant',
    get fullName(){ // 用来得到当前属性值的回调函数，增加了fullName属性
        return this.firstName + ' ' + this.lastName
    },
    set fullName(data){ // 用来监视当前属性值变化的回调函数
        var names = data.split(' ');
        this.firstName = names[0];
        this.lastName = names[1];
    }
};
```

### 数组扩展

```javascript
var arr = [1, 4, 6, 2, 5, 6];

console.log(arr.indexOf(6)); // 得到值在数组中的最后一个下标，输出2
console.log(arr.lastIndexOf(6)); // 得到值在数组中的最后一个下标，输出5

// 遍历数组
arr.forEach(function (item, index) {
    console.log(item, index);
});

// 遍历数组返回一个新的数组，返回加工之后的值
var arr1 = arr.map(function (item, index) {
    return item + 10
});

// 遍历过滤出一个新的子数组，返回条件为true的值
var arr2 = arr.filter(function (item, index) {
    return item > 4
});
```

### 函数扩展

```javascript
var obj = {name:'kobe', age:39};
function fun(age) {
    this.name = 'kobe';
    this.age = age;
}
var obj = {};
fun.bind(obj, 12)();
```

## ES6+

### 主要特性

#### `let`关键字

`let`声明变量

* 作用：与var类似，用于声明一个变量。
* 特点：在块作用域内有效；不能重复声明；不会预处理，不存在提升。
* 应用：循环遍历加监听，使用`let`取代`var`。

#### `const`关键字

定义一个常量

* 特点：不能修改；其它特点同`let`。
* 应用：保存不用改变的数据。

#### 解构赋值

```javascript
// 对象结构赋值，变量需要和属性同名
let obj = {name : 'kobe', age : 39};
let {name, age} = {name : 'kobe', age : 39}; 
let {age} = obj; // 结构部分属性

// 数组结构
let arr = ['abc', 23, true];
let [a, b] = arr; // 根据变量的位置结构取值，只取前两个值
let [, a, b] = arr; // 取后两个值
```

#### `...`算符

```javascript

// 用来取代arguments但比arguments灵活，只能是最后部分形参参数；扩展运算符。
function fun(...values) {
    values.forEach(function (item, index) { // value是一个数组
        console.log(item, index);
    })
}
fun(1, 2, 3);

// 拼接组数
let arr = [2,3,4,5,6];
let arr1 = ['abc', ...arr, 'fg']; 

// 结构对象
let obj1 = { left: 100, top: 100 };
let obj2 = { width: 200, height: 200 };
let obj3 = {
  ...obj1,
  ...obj2
}
```

#### 模板字符串

模板字符串，简化字符串的拼接。模板字符串必须用\`\`包含，变化的部分使用`${xxx}`定义

```javascript
let obj = {name:'anverson', age:41};
console.log(`我叫: ${obj.name}, 我的年龄是: ${obj.age}`);
```

#### 对象简化写法

简化的对象写法：省略同名的属性值；省略方法的`function`
```javascript
let x = 3;
let y = 5;

// 普通写法
let obj = {
   x: x,
   y: y,
   getPoint: function () {
       return this.x + this.y
   }
};

// 简化写法
let obj = {
    x,
    y,
    getPoint() {
        return this.x + this.y
    }
};
```

#### 箭头函数

定义匿名函数。箭头函数没有自己的this，箭头函数的this不是调用的时候决定的，而是在定义的时候处在的对象就是它的this。即：箭头函数的this看外层的是否有函数，如果有，外层函数的this就是内部箭头函数的this，如果没有，则this是window。

```javascript
// 没有形参，函数体只有一条语句
let fun1 = () => console.log('fun1()');
fun1();

// 一个形参，函数体只有一条语句
let fun2 = x => x; // 括号可以省略
console.log(fun2(5));

// 形参是一个以上，函数体只有一条语句
let fun3 = (x, y) => x + y;
console.log(fun3(25, 39)); // 只有一条语句{}可以省略，省略后自动回表达式

// 函数体有多条语句
let fun4 = (x, y) => {
    console.log(x, y);
    return x + y; // 有{}必须手动返回值
};
console.log(fun4(34, 48));

// 箭头函数应用
let btn = document.getElementById('btn');
btn.onclick = function () {
    console.log(this); // this指btn
};

btn.onclick = () => {
    console.log(this); // this指window
};

let btn2 = document.getElementById('btn2');
let obj = {
    name : 'kobe',
    age : 39,
    getName: () => { // this再次指向上一层
        btn2.onclick = () => { // this指向上一成
            console.log(this); // this指window
        };
    }
  
  	getName2: function() { // 指obj
        btn2.onclick = () => { // this指向上一成
            console.log(this); // this指obj
        };
    }
};
```

#### 形参默认值

```javascript
function Point(x=12, y=12) {
    this.x = x;
    this.y = y;
}
let p = new Point(); // 使用默认值
let point = new Point(25, 36);
```

#### Promise对象

异步编程解决方法，体现在代码中是一个对象，可以通过Promise构造函数来实例化。

promise对象的3个状态：

  * pending：初始化状态
  * fullfilled：成功状态
  * rejected：失败状态

可以从初始化变为成功或失败的一种。通过异步执行结果修改promise的状态，在then调用中处理成功或失败的函数。

```javascript
// 创建一个promise实例对象
let promise = new Promise((resolve, reject) => {
    // 初始化promise的状态为pending --> 初始化状态
    console.log('1111');  // 同步执行
    // 启动异步任务：发送请求或开启定时器等
    setTimeout(() => { // 两秒后执行
        console.log('3333');
      	// 根据异步任务的返回结果修改promise状态
        resolve('atguigu.com'); // 修改promise的状态pending变为fullfilled（成功状态）
        reject('xxxx'); // 修改promise的状态pending变为rejected(失败状态)
    }, 2000)
});

// 相当于把原来setTimeout处理的回调函数拿到出处来处理
promise.then((data) => { // 成功回调函数
    console.log('成功了。。。' + data);
}, (error) => { // 失败回调函数
    console.log('失败了' + error);
});

promise.then((data) => {
   console.log('成功了。。。' + data);
}).catch((err) => { // 调用reject函数，与上面失败回调函数一样
  console.log(data);
})

// 函数中定义一个promise对象，用来处理请求，并将promise返回，
function getNews(url) {
    // 创建一个promise对象
    let promise = new Promise((resolve, reject) => {
        // 初始化promise状态为pending，启动异步任务
        let request = new XMLHttpRequest();
        request.onreadystatechange = function () {
            if(request.readyState === 4){ // 设置了promise状态可以在then中处理
                if(request.status === 200){
                    let news = request.response;
                    resolve(news);
                }else{
                    reject('请求失败了。。。');
                }
            }
        };
        request.responseType = 'json';//设置返回的数据类型
        request.open("GET", url);//规定请求的方法，创建链接
        request.send(); // 发送
    })
    return promise;
}

// 调用请求函数，得到promise对象，并调用promise的then函数处理请求
getNews('http://localhost:3000/news?id=2')
        .then((news) => { // 首先请求新闻
            document.write(JSON.stringify(news));
  					// 请求成功后再去请求评论，调用同一请求函数传递不同地址
            return getNews('http://localhost:3000' + news.commentsUrl); // then的方法可以链式调用，需要返回另一个promise对象
        }, (error) => { // 请求失败
            alert(error); // 如果失败了，没有返回promise对象，调用停止
        }).then((comments) => { // 链式调用，接收第二个promise结果，多次请求变为顺序执行，避免的回调嵌套
            document.write('<br><br><br><br><br>' + JSON.stringify(comments));
        }, (error) => {
            alert(error);
        })
```

#### Symbol

ES6中的添加了一种原始数据类型symbol，主要用于数据私有化问题。在typescript中可以忽略。

* Symbol属性对应的值是唯一的，解决命名冲突问题。
* Symbol值不能与其他数据进行计算，包括同字符串拼串。
* for in, for of遍历时不会遍历symbol属性。
* ES6提供了11个内置的Symbol值，指向语言内部使用的方法。

```javascript
// 用作对象的属性(唯一)
let symbol = Symbol();
let obj = {username: 'kobe', age: 39};
obj[symbol] = 'hello';

// 传参标识
let symbol = Symbol('one');
let symbol2 = Symbol('two');

// 定义常量
const Person_key = Symbol('(Person_key')
```

#### 迭代器与生成器

迭代器（Iterator）主要供`for ... of`进行遍历。

* `for ... in ...` 以原始插入顺序迭代对象的可枚举属性。
* `for ... of ...` 根据可迭代对象的迭代器具体实现迭代对象的数据。

原生具备iterator接口的数据：Array、arguments、set容器、map容、String。使用解构赋值以及三点运算符时会默认调用iterator接口。

```javascript
let arr3 = [1, 2, 'kobe', true];
for(let i of arr3) { // 迭代器遍历
    console.log(i);
}
```

生成器（Generator）函数是一个状态机，内部封装了不同状态的数据，function与函数名之间有一个星号，内部用yield表达式来定义不同的状态。generator函数返回的是指针对象，调用next方法函数内部逻辑开始执行，遇到yield表达式停止，返回`{value: yield/undefined, done: false/true}`。

```javascript
// 定义Generator函数
function* generatorTest() {
    console.log('函数开始执行');
    result = yield 'hello';
    console.log('函数暂停后再次启动');
    yield 'generator';
}
// 生成遍历器对象
let Gt = generatorTest();
let result = Gt.next(); // 函数执行，遇到yield暂停，{value: "hello", done: false}
result = Gt.next('input'); // 函数再次启动，并传入返回值用result接收
result = Gt.next();
console.log(result); // {value: undefined, done: true} 表示函数内部状态已经遍历完毕

// 对象的Symbol.iterator属性;
let myIterable = {};
myIterable[Symbol.iterator] = function* () {
    yield 1;
    yield 2;
    yield 4;
};
for(let i of myIterable) {
    console.log(i);
}
let obj = [...myIterable];
```

#### async

解决异步回调的问题，同步流程表达异步操作。本质是 Generator的语法糖。

* 不需要像Generator去调用next方法，遇到await等待，当前的异步操作完成就往下执行
* 返回的总是Promise对象，可以用then方法进行下一步操作
* async取代Generator函数的星号*，await取代Generator的yield

```javascript
// 语法格式
async function foo(){
  await 异步操作;
  await 异步操作;
}

// 基本使用
async function timeout(ms) {
  return new Promise(resolve => {
    setTimeout(resolve, ms); // 直接返回成功函数
  })
}

async function asyncPrint(value, ms) {
  console.log('函数执行', new Date().toTimeString());
  await timeout(ms);
  console.log('延时时间', new Date().toTimeString()); // 会等到timeout执行完成
  console.log(value);
}
console.log(asyncPrint('hello async', 2000));

// 请求新闻和评论函数
async function sendXml(url) {
  return new Promise((resolve, reject) => {
    $.ajax({
      url,
      type: 'GET',
      success: data =>  resolve(data),
      error: error => reject(error)
    })
  })
}

async function getNews() {
  // 首先请求新闻
  let result = await sendXml('http://localhost:3000/news?id=2')
  // 再次请求评论，会等待第一次请求完成
  result = await sendXml('http://localhost:3000' + result.commentsUrl)
}
```

#### `Class`

```javascript
class Person {
    constructor(name, age){ // 调用类的构造方法
        this.name = name;
        this.age = age;
    }
    
    showName() { // 定义一般的方法
        console.log(this.name, this.age);
    }
}

class StrPerson extends Person { // 定义一个子类
    constructor(name, age, salary) { 
        super(name, age); // 调用父类的构造方法
        this.salary = salary;
    }
  
    showName() { // 在子类自身定义方法
        console.log(this.name, this.age, this.salary);
    }
}
```

### 其他特性

#### 字符串扩展

```javascript
let str = 'abcdefg';
console.log(str.includes('a')); // true，判断是否包含指定的字符串
console.log(str.startsWith('a'));// true，判断是否以指定字符串开头
console.log(str.endsWith('g'));// true，判断是否以指定字符串结尾
console.log(str.repeat(5)); // 将当前字符重复5次
```

#### 数值扩展

```javascript
a = 0b1010; // 二进制
b = 0o56; // 八进制
console.log(Number.isFinite(NaN)); // false，判断是否是有限大的数
console.log(Number.isNaN(NaN)); // true，判断是否是NaN 
console.log(Number.isInteger(5.23)); // false，判断是否是整数 
console.log(Number.parseInt('123abc')); // 123，将字符串转换为对应的数值
console.log(Math.trunc(13.123)); // 13，直接去除小数部分
```

#### 数组扩展

```javascript
// Array.from(v): 将伪数组对象或可遍历对象转换为真数组
let btns = document.getElementsByTagName('button');
Array.from(btns).forEach(function (item, index) {
    console.log(item, index);
});

// Array.of(v1, v2, v3): 将一系列值转换成数组
let arr = Array.of(1, 'abc', true);

// find: 找出第一个满足条件返回true的元素
let arr1 = [1, 3, 5, 2, 6, 7, 3];
let result = arr1.find(function (item, index) {
    return item >3
}); // 返回 5

// findIndex: 找出第一个满足条件返回true的元素下标
let result1 = arr1.findIndex(function (item, index) {
    return item >3
}); // 返回 2

// map方法返回新数组
let one = [1, 2, 3];
let two = one.map((item, index, arr) => item * item); 

// 通过reduce函数生成数组
keys.reduce((prev, key) => {
  return { ...prev, [key]: obj.get(key) || "" };
}, {});
```

#### 对象扩展

```javascript
// 判断2个数据是否完全相等
console.log(Object.is('abc', 'abc')); // true

console.log(NaN == NaN); // false
console.log(Object.is(NaN, NaN)); // true

console.log(0 == -0); // true
console.log(Object.is(0, -0)); // false

// 将源对象的属性复制到目标对象上，源对象可以是多个
let obj = {name : 'kobe', age : 39, c: {d: 2}};
let obj1 = {};
Object.assign(obj1, obj); // obj1目标对象，obj源对象

// 直接操作 __proto__ 属性
let obj3 = {name : 'anverson', age : 41};
let obj4 = {};
obj4.__proto__ = obj3;
console.log(obj4, obj4.name, obj4.age);

// 动态生成对象的key值
const chosenAnimal = 'cat'
const animals = {
  [`animal${chosenAnimal}`]: true,
}

// 与上面写法等价
const chosenAnimal = 'cat'
const animals = { }
animals[`animal${chosenAnimal}`] = true
```

#### set和map数据结构

set和map可以使用`for of`变量

```javascript
// 无序不可重复的多个value的集合体
let set = new Set([1,2,3,4,3,2,1,6]); 

// 相关方法
set.add('abc'); // 添加
set.clear(); // 清空
set.delete(2); // 删除原始2
set.has(4) 
set.size()


// 无序的key不重复的多个key-value的集合体，key可以是任意对象
let map = new Map([
  ['abc', 12], // 键是'abc'值是12
  [25, 'age']  // 键是25值是'age'
]);

// 相关方法
map.set('男', '性别'); // 参数1是key，参数2是值
map.delete('男'); // 删除使用key值
map.clear();
map.forEach((value, key) => {
  console.log(key, value);
})
map.values() // value迭代器
map.keys() // key迭代器
for (let key of map.keys()) {
   console.log(value)
}
...map.valuse() // 对数据进行展开
```

### ES7

```javascript
// 指数运算符(幂)
console.log(3 ** 3);//27

// 判断数组中是否包含指定value
let arr = [1,2,3,4, 'abc'];
console.log(arr.includes(2)); // true
```

#### 修饰符

```javascript
@T
class User { // user会被作为参赛传入T中
    constructor(name, age=20) {
        this.name = name
        this.age = age
    }
}

function T(target) { // 被修饰对象
    target.contry = '中国' // 向对象添加属性
}
```



# 模块化

## 模块化基本形式

### 命名空间模式

模块定义

```javascript
function myModule() {
  var msg = 'My atguigu'

  function doSomething() {
    console.log('doSomething() ' + msg.toUpperCase())
  }
  
  function doOtherthing () {
    console.log('doOtherthing() '+msg.toLowerCase())
  }

  // 向外暴露对象(给外部使用的方法)
  return {
    doSomething: doSomething,
    doOtherthing: doOtherthing
  }
}
```

模块引用

```javascript
var module = myModule()
module.doSomething()
```

### IIFE模式

#### 基本模式

模块定义

```javascript
(function (window) {
  var msg = 'My atguigu'
  
  function doSomething() {
    console.log('doSomething() '+msg.toUpperCase())
  }
  
  function doOtherthing () {
    console.log('doOtherthing() '+msg.toLowerCase())
  }
  
  // 向外暴露对象直接添加给window
  window.myModule = {
    doSomething: doSomething,
    doOtherthing: doOtherthing
  }
})(window)
```

模块引用

```javascript
myModule.doSomething()
```

#### 增强模式

模块定义，增加引入了jQuery，可以操作界面元素。

```javascript
(function (window, $) {
  //数据
  let data = 'atguigu.com'

  // 操作数据的函数
  function foo() { 
    $('body').css('background', 'red')
  }

  function bar() {
    console.log(`bar() ${data}`)
    otherFun() // 调用内部
  }

  function otherFun() { // 内部私有的函数
    console.log('otherFun()')
  }

  window.myModule = {foo, bar} // 导出函数，es6简写
})(window, jQuery)
```

模块引用

```javascript
myModule.foo()
```

## 模块化规范

### CommonJS

特点：

* 每一个文件都可以当做一个模块
* 服务器端：模块加载是运行同步加载
* 浏览器端：模块需要提前编译打包处理

#### 服务器端

原理通过`module.exports`向外暴露属性和方法`module`被添加在全局对象`global`上。

模块定义

```javascript
// module1，整体暴露一个对象
module.exports = {
  msg: 'module1', 
  foo() { 
    console.log(this.msg)
  }
}

// module2，整体暴露一个函数
module.exports = function () {
  console.log('module2()')
}

// module3，以属性方式暴露多个函数
exports.foo = function () {
  console.log('module3 foo()')
}

exports.bar = function () {
  console.log('module3 bar()')
}
```

模块引用

```javascript
let module1 = require('./modules/module1')
let module2 = require('./modules/module2')
let module3 = require('./modules/module3')

module1.foo()
module2()
module3.foo()
module3.bar()
```

#### 浏览器

浏览器端使用CommonJS规范需要安装依赖库，browserify。

使用流程

1. 创建相应模块。
2. 在主文件内引用模块。
3. 使用browserify命令对主文件打包生成相应文件。打包命令`browserify [源文件] -o [目标文件]`。
4. 在html中引用生成文件。`<script type="text/javascript" src="js/dist/bundle.js"></script>`

### AMD

专门针对浏览器端使用，模块加载是异步执行。需要依赖于`require.js`文件

模块定义

```javascript
// dataService.js 定义没有依赖的模块
define(function () { // define是AMD语法
  let msg = 'atguigu.com'
  function getMsg() {
    return msg.toUpperCase()
  }
  return {getMsg}
})

// alerter.js 定义有依赖的模块
define(['dataService', 'jquery'], function (dataService, $) { // AMD语法先声明依赖模块
  let name = 'Tom2'
  function showMsg() {
    $('body').css('background', 'gray')
    alert(dataService.getMsg() + ', ' + name)
  }
  return {showMsg}
})
```

模块引用

主模块

```javascript
(function () {
  // require.js配置
  require.config({
    baseUrl: 'js/', //基本路径
    
    paths: { // 映射: 模块标识名: 路径
      // 自定义模块
      'alerter': 'modules/alerter',
      'dataService': 'modules/dataService',

      // 库模块
      'jquery': 'libs/jquery-1.10.1',
      'angular': 'libs/angular'
    },

    
    shim: { // 配置不兼容AMD的模块
      angular: {
        exports: 'angular' // 暴露模块名称
      }

    }
  })

  // 引入模块使用
  require(['alerter', 'angular'], function (alerter, angular) {
    alerter.showMsg()
    console.log(angular);
  })
})()
```

在html中配置入口文件

```html
<script type="text/javascript" src="js/libs/require.js" data-main="js/main.js"></script>
```

### ES6

依赖模块需要编译打包处理，使用bable编译打包处理。

定义模块

```javascript
// module1，分别暴露
export function foo() {
  console.log('module1 foo()');
}

export let bar = function () {
  console.log('module1 bar()');
}

export const DATA_ARR = [1, 3, 5, 1]

// module2，统一暴露
let data = 'module2 data'

function fun1() {
  console.log('module2 fun1() ' + data);
}

function fun2() {
  console.log('module2 fun2() ' + data);
}

export {fun1, fun2}

// module3，默认暴露
export default {
  name: 'Tom',
  setName: function (name) {
    this.name = name
  }
}
```

引用模块

主模块

```javascript
// 分别暴露和统一暴露 整体暴露一个容器对象，需要结构赋值，取出内容
import {foo, bar} from './module1'
import {DATA_ARR} from './module1'
import {fun1, fun2} from './module2'

// 引用默认暴露，相当于引用一个对象
import person from './module3'
```

使用流程

1. 使用badle对全部模块打包`babel [源文件] -d [目标文件]`，生成es5语法格式文件
2. 使用`browserify [源文件] -o [目标文件]`，生成可调用文件。
3. 在html中使用可调用文件。

```html
<script type="text/javascript" src="js/lib/bundle.js"></script>
```

