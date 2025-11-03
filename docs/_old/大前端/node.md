# Node

## nvm

nvm（node version manager）用于管理node的不同版本

nvm安装的node路径为`/Users/xusu/.nvm/versions/node/v10.18.0/bin/node`

```shell
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.36.0/install.sh | bash

# 在.bash_profile中添加，设置环境变量，新版本nvm默认自动添加
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion


# .bash_profile 中添加
export NVM_NODEJS_ORG_MIRROR=http://npm.taobao.org/mirrors/node

# 常用命令
nvm ls-remote # 查看版本
nvm install v4.5.0 # 安装某一版本
nvm ls # 查看所有安装过的版本
nvm alias default v10.10.0 # 修改默认版本
nvm use v10.10.0 # 在当前命令窗口使用v10.10.0，不修改默认版本
nvm uninstall v10.10.0 # 卸载指定版本
nvm node_mirror [url] # 设置nvm镜像
nvm npm_mirror [url] # 设置npm镜像
nvm current # 显示当前版本
nvm deactivate # 解除当前绑定版本
nvm install stable # 安装最新稳定版node

which node # 查看node安装位置

# nvm 卸载
cd ~
rm -rf .nvm # 卸载包
# 移除掉~/.profile, ~/.bash_profile, ~/.zshrc, ~/.bashrc文件中关于nvm的配置


```

## 包管理工具

### npm

Node Package Manager包管理工具

* 通过npm下载的包都放到node_modules文件夹中。
* npm包可以直接通过包名引入。
* node在使用模块名字来引入模块时，会首先在当前目录的node_modules中寻找，如果没有则去上一级目录的node_modules中寻找，直到找到为止或磁盘的根目录，如果没有则报错。

```shell
# 基本命令
npm # 查看帮助
npm -v # npm版本

npm init # 初始化包，生成package.json文件
npm init -y # 直接初始化项目目录

npm search [包名] # 搜索包名
npm install [包名] # 安装包
npm install [包名] --save # 安装包，并添加到依赖中
npm install -g [包名] # 全局安装，一般是一些工具
npm remove [包名] # 删除包
npm root -g # 全局下载根目录
npm list --depth --global # 查看全局安装包
npm list --depth=0 # 查看文件安装包

# 设置淘宝镜像服务器，使用cnpm是淘宝服务器，npm是原始服务器
npm install -g cnpm --registry=https://registry.npm.taobao.org
# 直接修改npm镜像，会在用户目录下生产.npmrc文件可
npm config set registry https://registry.npm.taobao.org
npm config get registry # 检查仓库路径

npm view umi version # 查看某个包的最新版本

npm update # 更新所有包
npm update dayjs # 更新单个包
```

### yarn使用

进阶的npm管理工具

```shell
# 淘宝镜像安装
npm i yarn tyarn -g # tyarn为淘宝镜像
tyarn -v 

# 设置yarn环境变量
yarn config set prefix /Users/xusu/opt/yarn
yarn config set global-folder "/Users/xusu/opt/yarn/global" 
yarn config set cache-folder "/Users/xusu/opt/yarn/cache"
yarn config list # 查看config配置
export PATH="$PATH:`yarn global bin`" # 导出环境变量

tyarn # 淘宝代替yarn工具
tyarn add @ant-design/pro-form

yarn init # 初始化项目

yarn / yarn install # 一键安装jpackage.json所有包
yarn add [package] # 添加包，会自动安装最新版本，注意会覆盖指定版本号！！！
yarn add [package]@[version] #  带版本号安装
yarn remove [package] # 移除某个包
yarn upgrade [package] # 更新一个包
yarn add vue@2.5.0  
yarn upgrade vue 
yarn upgrade vue@3.0.0 
yarn remove vue 

yarn add --dev react-dev-inspector # 安装到devDependencies
yarn global add # 全局安装
```

## 入门

```shell
node -v # 查看node版本

# 执行js文件
node hello.js
```

在node中有一个全局对象global，类似网页中window。变量为global的属性，函数为global的方法。

```javascript
var a = 10;
console.log(global.a); // 全局路径
```

使用webstorm中的run命令可以运行单个node文件

### commonjs

模块化

* 模块类型
  * 核心模块：由node引擎提供的模块。
  * 文件模块：用户自己创建的模块。

* 在Node中，一个js文件就是一个模块。每模块的代码都包装到到一个函数中。
  * `exports`该对象用来将变量或函数暴露到外部。
  * `require`用来引入外部的模块。
  * `module`当前模块本身，exports就是module的属性。
  * `__filename`当前模块的完整路径。
  * `__dirnam`当前模块所在文件夹的完整路径。

```javascript
function (exports, require, module, __filename, __dirname) {} // 包装模块的代码
```

* 导出变量和方法
  * 通过 exports 来向外部暴露变量和方法，将外部变量或方法设置为exports的属性。
  * 也可以使用module.exports导出，这种方式可以直接赋值。

```javascript
exports.x = "x变量";
exports.y = "y变量";
exports.fn = function () {
	console.log("this is math");
};

module.exports.add = function (a , b) {
	return a+b;
};

// 直接赋值导出模块
module.exports = {
	name:"猪八戒",
	age:28,
	sayName:function () {
		console.log("我是猪八戒");
	}
};
```

* 通过`require()`函数来引入外部的模块，相对路径以`.`开头。该函数会返回一个对象，这个对象代表引入的模块。

```javascript
var math = require("./math");
console.log(math.add(123,456));
```

### Package（包）

commonjs包规范由包结构和包描述文件两部分组成

* 包结构，用于组织包中的各种文件。
  * `package.json`描述文件（必须文件）
  * `bin`可执行二进制文件
  * `lib`js代码
  * `doc`文档
  * `test`单元测试
* 包描述文件，描述包的相关信息，以供外部读取分析，保存在包的根目录下。

```json
{
  "dependencies":{}, // 依赖
  "descriotion": "", // 包描述
  "devDependencies": {}, // 开发依赖
  "main": "./index", // 主文件
  "name": "node_test",  // 包名npm规范包名全部使用小写，require所需要的包名。
}
```

## 文件系统

文件系统就是通过Node来操作系统中的文件。

### Buffer

Buffer：专门用来存储二进制数据， 结构和数组很像，操作的方法也和数组类似。Buffer中的一个元素，占用内存的一个字节，大小一旦确定，则不能修改。Buffer实际上是对底层内存的直接操作。

```javascript
var str = "Hello 尚硅谷";
var buf = Buffer.from(str); // 将一个字符串保存到buffer中
console.log(buf.length); // 占用内存的大小

// 创建一个指定大小的buffer，buffer构造函数都不推荐使用
var buf2 = new Buffer(10);
var buf2 = Buffer.alloc(10); // 通常创建buffer的方法

//通过索引，来操作buf中的元素
buf2[0] = 88;
buf2[1] = 255;
buf2[2] = 0xaa;

// 转换为16进制字符串
console.log(buf2[2].toString(16));

// 遍历buffer，只要数字在控制台输出一定是10进制
for(var i=0 ; i<buf2.length ; i++){
	console.log(buf2[i]);
}

// 创建一个指定大小的buffer，不清空内存
buf3 = Buffer.allocUnsafe(10);

var buf4 = Buffer.from("我是一段文本数据");
console.log(buf4.toString()); // 将缓冲区中的数据转换为字符串
```

### 文件

使用文件，需要引入fs是核心模块，可以直接引入。fs中所有操作都分为同步操作和异步操作，同步会阻塞程序执行，异步操作不会阻塞程序，会在操作完成后通过回调函数将结果返回。

#### 文件写入

操作步骤：1. 打开文件；2. 向文件中写入内容；3. 保存并关闭文件。

```javascript
// 同步文件写入
var fs = require('fs')
fd = fs.openSync('demo.txt', 'w') // 返回一个文件的描述符
fs.writeSync(fd, 'hello, world') // 向文件描述符中写入内容
fs.closeSync(fd) // 关闭文件传入文件描述符

// 异步文件写入
// 回调函数两个参数: err 错误对象，如果没有错误则为null，fd文件的描述符
fs.open('demo.txt', 'w', function (err, f) {
    if (!err) {
        fs.write(f, '异步写入的内容', function (err, f) {
            if (!err) {
                fs.close(f, function (err) {
                    console.log(err)
                })
            }
        })
    } else {
        console.log('打开文件错误')
    }
})

// 简单文件写入，flag 1.r 只读 2.w 可写 3.a 追加
var txt = '简单写入内'
fs.writeFile("demo.txt", txt,{flag:"w"} , function (err) {
    if(!err){
        console.log("写入成功~~~");
    }else{
        console.log(err);
    }
});

// 流文件写入
// 创建一个可写流
var ws = fs.createWriteStream("demo.txt");

/*
  on: 可以为对象绑定一个事件
  once: 可以为对象绑定一个一次性的事件，该事件将会在触发一次以后自动失效
*/

// 绑定open回调函数
ws.once("open",function () {
    console.log("流打开了~~~");
});

// 绑定close回调函数
ws.once("close",function () {
    console.log("流关闭了~~~");
});

ws.write("通过可写流写入文件的内容\n");
ws.write("今天天气真不错\n");

// 关闭流
ws.end();
```

#### 文件读取

```javascript
var fs = require('fs')

// 文件读取
fs.readFile('demo.txt', function (err, data) {
    if (!err) {
        console.log(data.toString())
    }
})

// 流文件读取
var rs = fs.createReadStream('demo.txt');
rs.once("open",function () {
    console.log("可读流打开了~~");
});
rs.once("close",function () {
    console.log("可读流关闭了~~");
});

// 读取数据，需要绑定一个data事件，通过回调函数读取数据
rs.on("data", function (data) {
    console.log(data.length); // 默认读取65536字节
});

var rs = fs.createReadStream('demo.txt'); // 创建一个可读流
var ws = fs.createWriteStream("out.txt"); // 创建一个可写流
rs.pipe(ws); // pipe()可以将可读流中的内容，直接输出到可写流中
```

# 常用包

## docsify

安装

```shell
yarn global add docsify-cli # 安装工具

docsify init ./docs # 初始化项目
docsify init .

docsify serve # 本地预览
docsify -v # 查看版本
```

