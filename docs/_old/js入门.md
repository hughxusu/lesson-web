# js入门

## 基本语法

## 对象

### `this`

```javascript
/*
* 	根据函数的调用方式的不同，this会指向不同的对象
* 	1.以函数的形式调用时，this永远都是window
* 	2.以方法的形式调用时，this就是调用方法的那个对象
*/

//创建一个name变量
var name = "全局";

//创建一个fun()函数
function fun(){
  console.log(this.name);
}

//创建两个对象
var obj = {
  name:"孙悟空",
  sayName:fun
};

var obj2 = {
  name:"沙和尚",
  sayName:fun
};

obj.sayName();
obj2.sayName();
```

### 工厂方法

```javascript
// 使用工厂方法创建对象通过该方法可以大批量的创建对象
function createPerson(name , age ,gender){
  // 创建一个新的对象 
  var obj = new Object();
  
  // 向对象中添加属性
  obj.name = name;
  obj.age = age;
  obj.gender = gender;
  obj.sayName = function() {
    alert(this.name);
  };
  
  // 将新的对象返回
  return obj;
}

var person = createPerson("猪八戒", 28, "男");
person.sayName()
```

### 构造函数

```javascript
/*
* 构造函数，专门用来创建Person对象，构造函数就是一个普通的函数，
* 构造函数习惯上首字母大写，构造函数需要使用new关键字来调用
* 
* 构造函数的执行流程：
* 1. 立刻创建一个新的对象
* 2. 将新建的对象设置为函数中this，在构造函数中可以使用this来引用新建的对象
* 3. 逐行执行函数中的代码
* 4. 将新建的对象作为返回值返回
* 
* this的情况：
* 1. 当以函数的形式调用时，this是window
* 2. 当以方法的形式调用时，this指向调用对象
* 3. 当以构造函数的形式调用时，this就是新创建对象
*/
function Person(name , age , gender){
  this.name = name;
  this.age = age;
  this.gender = gender;
  this.sayName = function(){
    alert(this.name);
  };
}

var per = new Person("孙悟空", 18, "男");

/*
* 使用instanceof可以检查一个对象是否是一个类的实例
* 对象 instanceof 构造函数，如果是，则返回true，否则返回false
*/
console.log(per instanceof Person);

/*
* 所有的对象都是Object的后代，
* 所以任何对象和Object左instanceof检查时都会返回true
*/
console.log(dog instanceof Object);

// 构造函数优化

/*
* 在构造函数内部创建，构造函数每执行一次就会创建一个新的sayName方法
* 也是所有实例的sayName都是唯一的。
* 这是完全没有必要，完全可以使所有的对象共享同一个方法。
*/
function Person(name , age , gender){
  this.name = name;
  this.age = age;
  this.gender = gender;
}

// 向原型中添加sayName方法
Person.prototype.sayName = function(){
  alert("Hello大家好，我是:"+this.name);
};

//创建一个Person的实例
var per = new Person("孙悟空",18,"男");
per.sayName();
```

### 原型与原型链

<img src="https://clarkdo.js.org/public/img/jsobj_full.jpg" style="zoom:50%;" />

* 所有函数都有一个特别的属性：
  * `prototype` ：显式原型属性，默认指向一个空对象。
  * `prototype`中有`constructor`属性指函数对象。

```javascript
console.log(Date.prototype, typeof Date.prototype)

function Fun () {
}

console.log(Fun.prototype)  // 默认指向一个Object空对象

// 原型对象中有一个属性constructor, 它指向函数对象
console.log(Date.prototype.constructor === Date)
console.log(Fun.prototype.constructor === Fun)

// 给原型对象添加方法，实例对象可以访问
Fun.prototype.test = function () {
    console.log('test()')
}

var fun = new Fun()
fun.test()
```

* 所有实例对象都有一个特别的属性：
  * `__proto__` ：隐式原型属性。

* 显式原型与隐式原型的关系
  * 函数的prototype：定义函数时被自动赋值，值默认为{}，即用为原型对象。
  * 实例对象的`__proto__`：在创建实例对象时被自动添加，并赋值为构造函数的prototype值。
  * 原型对象即为当前实例对象的父对象。

```javascript
function Fn() { }
console.log(Fn.prototype) // 显式原型属性
var fn = new Fn()
console.log(fn.__proto__) // 隐式原型
// 对象的隐式原型的值为其对应构造函数的显式原型的值
console.log(Fn.prototype === fn.__proto__) // true
```

* 原型链（隐式原型链）
  * 所有的实例对象都有`__proto__`属性，它指向的就是原型对象
  * 这样通过`__proto__`属性就形成了一个链的结构，原型链
  * 当查找对象内部的属性/方法时，js引擎自动沿着这个原型链查找
  * 当给对象属性赋值时不会使用原型链, 而只是在当前对象中进行操作

```javascript
function Fn() {
    this.test1 = function () {
        console.log('test1()')
    }
}

console.log(Fn.prototype)

Fn.prototype.test2 = function () {
    console.log('test2()')
}

var fn = new Fn()

fn.test1()
fn.test2()
console.log(fn.toString())

// 函数显示原型默认指向空Object实例对象(但Object不满足)
console.log(Fn.prototype instanceof Object) // true
console.log(Object.prototype instanceof Object) // false
console.log(Function.prototype instanceof Object) // true

// 所有函数都是Function的实例(包含Function)
console.log(Function.__proto__ === Function.prototype)

// Object的原型对象是原型链尽头
console.log(Object.prototype.__proto__) // null
```

* 原型链属性问题
  * 读取对象的属性值时：会自动到原型链中查找。
  * 设置对象的属性值时：不会查找原型链, 如果当前对象中没有此属性, 直接添加此属性并设置其值。
  * 方法一般定义在原型中，属性一般通过构造函数定义在对象本身上

```javascript
function Fn() {
}
Fn.prototype.a = 'xxx'
var fn1 = new Fn()

var fn2 = new Fn()
fn2.a = 'yyy' // 设置属性值不会查找原型链

function Person(name, age) {
    this.name = name
    this.age = age
}

Person.prototype.setName = function (name) {
    this.name = name
}

var p1 = new Person('Tom', 12)
p1.setName('Bob')
console.log(p1)

var p2 = new Person('Jack', 12)
p2.setName('Cat')
console.log(p2)
console.log(p1.__proto__ === p2.__proto__) // true
```

#### `toString`

```javascript
function Person(name , age , gender){
  this.name = name;
  this.age = age;
  this.gender = gender;
}

//修改Person原型的toString
Person.prototype.toString = function(){
  return "Person[name="+this.name+",age="+this.age+",gender="+this.gender+"]";
};


//创建一个Person实例
var per = new Person("孙悟空",18,"男");

var result = per.toString();
// console.log("result = " + result);
// console.log(per.__proto__.__proto__.hasOwnProperty("toString"));
console.log(per);
```

#### `instanceof`

表达式: A instanceof B，如果B函数的显式原型对象在A对象的原型链上，返回true，否则返回false。

Function是通过new自己产生的实例。

```javascript
function Foo() {  }
var f1 = new Foo()
console.log(f1 instanceof Foo) // true
console.log(f1 instanceof Object) // true
console.log(Object instanceof Foo) // false

console.log(Object instanceof Function) // true
console.log(Object instanceof Object) // true
console.log(Function instanceof Function) // true
console.log(Function instanceof Object) // true
```

### 继承

#### 原型链继承

* 创建父类型的对象赋值给子类型的原型
* 将子类型原型的构造属性设置为子类型

```javascript
function Supper() {
    this.supProp = 'Supper property'
}

Supper.prototype.showSupperProp = function () {
    console.log(this.supProp)
}

function Sub() {
    this.subProp = 'Sub property'
}

Sub.prototype = new Supper() // 子类型的原型为父类型的一个实例对象
Sub.prototype.constructor = Sub // 让子类型的原型的constructor指向子类型

Sub.prototype.showSubProp = function () {
    console.log(this.subProp)
}

var sub = new Sub()
```

#### 借用构造函数继承

* 并非真继承
* 在子类型构造函数中通用call()调用父类型构造函数

```javascript
function Person(name, age) {
    this.name = name
    this.age = age
}
function Student(name, age, price) {
    Person.call(this, name, age)  // this.Person(name, age)
    this.price = price
}

var s = new Student('Tom', 20, 14000)
```

#### 组合继承

```javascript
function Person(name, age) {
    this.name = name
    this.age = age
}
Person.prototype.setName = function (name) {
    this.name = name
}

function Student(name, age, price) {
    Person.call(this, name, age)  // 设置属性
    this.price = price
}
Student.prototype = new Person() // 得到父类方法
Student.prototype.constructor = Student // 修正constructor属性
Student.prototype.setPrice = function (price) {
    this.price = price
}

var s = new Student('Tom', 24, 15000)
```

## 常用函数和类

### `Date`对象

```javascript
// 创建一个Date对象，封装为当前代码执行的时间
var d = new Date();

//日期的格式  月份/日/年 时:分:秒
var d2 = new Date("2/18/2011 11:10:30");

// getDate() 获取当前日期对象是几日
var date = d2.getDate();

// getDay() 返回0-6的值，0表示周日，1表示周一
var day = d2.getDay();

// 获取当前时间对象的月份
// 会返回一个0-11的值：0表示1月，1表示2月
var month = d2.getMonth();

// 获取当前日期对象的年份
var year = d2.getFullYear();

/*
* getTime()
* 获取当前日期对象的时间戳
* 时间戳是从格林威治标准时间的1970年1月1日，0时0分0秒，到当前日期所花费的毫秒数
*/
var time = d2.getTime();

//获取当前的时间戳，利用时间戳来测试代码的执行的性能
var start = Date.now();

for(var i=0 ; i<100 ; i++){
  console.log(i);
}

var end = Date.now();
console.log("执行了："+(end - start)+"毫秒");
```

### 包装类

```javascript
/*
* 
* 在JS中为我们提供了三个包装类，通过这三个包装类可以将基本数据类型的数据转换为对象
* String() 可以将基本数据类型字符串转换为String对象
* Number() 可以将基本数据类型的数字转换为Number对象
* Boolean() 可以将基本数据类型的布尔值转换为Boolean对象
*/

var num = new Number(3);
var num2 = new Number(3);
var str = new String("hello");
var str2 = new String("hello");
var bool = new Boolean(true);
var bool2 = true;

// 向num中添加一个属性
num.hello = "abcdefg";
var b = new Boolean(false);


/*
* 方法和属性之能添加给对象，不能添加给基本数据类型
* 当我们对一些基本数据类型的值去调用属性和方法时，
* 浏览器会临时使用包装类将其转换为对象，然后在调用对象的属性和方法
* 调用完以后，在将其转换为基本数据类型
*/
var s = 123;
s = s.toString();
s.hello = "你好";
```

### 字符串对象

```javascript
/*
* 在底层字符串是以字符数组的形式保存的
* ["H","e","l"]
*/
var str = new String("hello");

// 可以用来获取字符串的长度
console.log(str.length);

str = "中Hello Atguigu";
// 返回字符串中指定位置的字符
var result = str.charAt(6);

// 获取指定位置字符的字符Unicode编码
result = str.charCodeAt(0);

// 根据字符编码去获取字符
result = String.fromCharCode(0x2692);

// 连接两个或多个字符串，作用和+一样
result = str.concat("你好", "再见");

/*
* indexof()
* 	- 该方法可以检索一个字符串中是否含有指定内容，从前往后找
* 	- 如果字符串中含有该内容，则会返回其第一次出现的索引
* 		如果没有找到指定的内容，则返回-1
* 	- 可以指定一个第二个参数，指定开始查找的位置
* 
* lastIndexOf();
* 	- 该方法的用法和indexOf()类似，从后往前找
* 	- 也可以指定开始查找的位置
*/

str = "hello hatguigu";
result = str.indexOf("h",1);
result = str.lastIndexOf("h",5);

// 从字符串中截取指定的内容，不影响原字符串，负数的话将会从后边计算
str = "abcdefghijk";
result = str.slice(1, 4);
result = str.slice(1, -1);

// 用来截取一个字符串，类似slice()，不能接受负值
result = str.substring(0, 1);

/*
* 用来截取字符串
* 参数：
* 1.截取开始位置的索引
* 2.截取的长度
*/
str = "abcdefg";
result = str.substr(3, 2);

/*
* split() 可以将一个字符串拆分为一个数组
* 参数：
* 需要一个字符串作为参数，将会根据该字符串去拆分数组
*/
str = "abcbcdefghij";
result = str.split("d");

// 参数为空串作，会将每个字符都拆分为数组中的一个元素
result = str.split("");

str = "abcdefg";

// 将一个字符串转换为大写并返回z
result = str.toUpperCase();

str = "ABCDEFG";

// 将一个字符串转换为小写并返回
result = str.toLowerCase();
```

### 正则表达式

```javascript
/*
* 语法：
* var 变量 = new RegExp("正则表达式", "匹配模式");
* 在构造函数中可以传递一个匹配模式作为第二个参数，
* i 忽略大小写 
* g 全局匹配模式
*/
var reg = new RegExp("ab", "i");
var str = "a";

// 检查一个字符串是否符合正则表达式的规则，符合则返回true，否则返回false
var result = reg.test(str);
reg.test("Ac");

/*
* 使用字面量来创建正则表达式
* 语法：var 变量=/正则表达式/匹配模式
*/
reg = /a/i; // 	var reg = new RegExp("a","i");
```

#### 正则函数

```javascript
var str = "1a2b3c4d5e6f7";
/*
* split()
* 方法中可以传递一个正则表达式作为参数，这样方法将会根据正则表达式去拆分字符串
*/

// 根据任意字母来将字符串拆分
var result = str.split(/[A-z]/);

/*
* search() 搜索字符串中是否含有指定内容
* - 如果搜索到指定内容，则会返回第一次出现的索引，如果没有搜索到返回-1
* - 它可以接受一个正则表达式作为参数，然后会根据正则表达式去检索字符串
* - serach()只会查找第一个，即使设置全局匹配也没用
*/

str = "hello abc hello aec afc";
// 搜索字符串中是否含有abc 或 aec 或 afc
result = str.search(/a[bef]c/);

/*
* match() 根据正则表达式，从一个字符串中将符合条件的内容提取出来
* - 默认情况下match只会找到第一个符合要求的内容，找到以后就停止检索
* 	可以设置正则表达式为全局匹配模式，这样就会匹配到所有的内容
* 	可以为一个正则表达式设置多个匹配模式，且顺序无所谓
* - match()会将匹配到的内容封装到一个数组中返回，即使只查询到一个结果 	
*/
str = "1a2a3a4a5e6f7A8B9C";
result = str.match(/[a-z]/ig);

/*
* replace() 可以将字符串中指定内容替换为新的内容
* - 参数：
* 	1.被替换的内容，可以接受一个正则表达式作为参数
* 	2.新的内容
* - 默认只会替换第一个
*/
result = str.replace(/[a-z]/gi , "");
```

#### 正则邮件

```javascript
/*
* 电子邮件
* 	hello  .nihao          @     abc  .com.cn
* 
* 任意字母数字下划线    .任意字母数字下划线  @   任意字母数字     .任意字母（2-5位）   .任意字母（2-5位）
* 
* \w{3,}  (\.\w+)*  @  [A-z0-9]+  (\.[A-z]{2,5}){1,2}
*/

var emailReg = /^\w{3,}(\.\w+)*@[A-z0-9]+(\.[A-z]{2,5}){1,2}$/;
var email = "abc.hello@163.com";
console.log(emailReg.test(email));
```
