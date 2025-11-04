# Koa

https://koa.bootcss.com/ 说明文件

## 环境搭建

### 通过koa命令创建项目

```shell
# 1. 全局安装脚手架工具
cnpm install -g koa-generator 

yarn global add koa-generator
tyarn global add koa-generator

# 2. 创建manager-server项目
koa2 manager-server # 如果无法使用koa2命令，说明需要配置环境变量，koa2命令与koa命令生成的模板不同

# 3. 安装依赖，进入项目文件夹
cnpm install
yarn

# 4. 启动项目
yarn start
node .bin/www
```

#### 目录结构

```shell
 |-- koa-server
   |-- app.js 						# 根入口
   |-- package-lock.json
   |-- package.json 			# 项目依赖包文件
   |-- bin
   |   |-- www						# 运行启动文件
|-- public								# 公共资源
| |-- images
   |   |-- javascripts
   |   |-- stylesheets
   |       |-- style.css
   |-- routes
   |   |-- index.js				# 定义了localhost:3000/之下的路由
   |   |-- users.js				# 定义了localhost:3000/users/之下的路由c
|-- views									# 视图Pug是一款HTML模板引擎，专门为 Node.js 平台开发
```

### 手动创建项目

1. 安装依赖包

```shell
tyarn init -y 
tyarn add koa
tyarn add nodemon # 文件监视工具, 文件修改重庆服务器
```

2. 添加app.js文件

```js
const Koa = require('koa');

const app = new Koa(); // 初始化服务器

app.use((ctx, next) => {
    ctx.body = 'hello koa!'; // 相应请求
})

app.listen(80); // 设置端口
```

3. 配置package.json启动脚本

```json
{
	...
  "scripts": {
    "dev": "export NODE_ENV=development; nodemon app.js" // 使用nodemon监视文件
  },
}
```

4. 启动服务器`tyarn run dev`或者使用webstorm启动服务。

## Koa基本使用

### koa核心架构

```shell
koa = Application # 当前应用程序对象 new Koa() 实例，一个实例可以处理多个请求;
				- Context # 实例中处理一个请求包含的上下文环境;
						- Reqeust		# 上下文环境中的的请求头
						- Response	# 上下文环境中的的请求相应
```

Koa是通过中间件的来处理响应流程

```js
const Koa = require('koa'); // 

const app = new Koa(); // 创建 Application

app.use(async (ctx, next) => { //ctx 处理请求的上下文环境
  
    console.log('登录处理');
  	throw new Error(); // 抛出异常

    next(); // 调用下一个, 处理中间件
});

// 中间件按照排列的前后顺序处理
app.use(async (ctx, next) => { // 可以调用异步函数
  	await someReqeust() 
    ctx.response.body = {a: 1, b: 2};
});

app.on('error', (err, ctx) => { // 处理异常中间件
    console.log('错了', err);
});

app.listen(80); // 监听地址和端口
```

### context对象

```js
ctx.req						// Node的request对象
ctx.res						// Node的response对象
ctx.request				// Koa的request对象
ctx.response			// Koa的response对象
ctx.state					// 用户数据存储空间
ctx.app						// 当前应用程序实例
ctx.cookies				// cookie处理
ctx.throw					// 抛出异常 ctx.throw(400) 可以指定相应的状态码
```

#### Reqeust对象

```js
request.header					// 头信息对象
request.method					// 请求方法
request.length					// 请求正文内容长度
request.url							// 请求URL
request.orginalURL			// 原始URL, 不包含协议与主机部分
request.href						// 原始完整URL，包含协议、主机、请求串
request.path						// URL路径部分
request.querystring			// URL中的querystring
request.search					// URL中的search, 带?的querystring
request.host						// 请求头中的host
request.URL							// 解析过的URL对象
request.type						// 请求头中 content-type
request.charset					// 请求头中的charset
request.query						// 解析过的querystring对象
... 										// 其他相关方法

ctx.header			// ctx中有相应字段指向request.header
... 						// ctx包含其他快捷字段
```

#### **Response**

```js
response.header			// 响应头对象
response.headers		// header的别名
response.socket			// socket对象
response.status			// 响应状态码
response.message		// 响应状态码描述文本
response.body				// 响应内容
response.length			// 响应内容长度
response.append			// 追加头信息
response.type				// content-type
...									// 其他相关方法

ctx.body				// ctx中有相应字段指向response.body
...							// ctx包含其他快捷字段
```

