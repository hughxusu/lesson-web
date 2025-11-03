# React

## 基本概念和语法

* 操作虚拟(virtual)DOM, 不总是直接操作DOM
* DOM Diff算法, 最小化页面重绘

相关库

* react.js: React的核心库
* react-dom.js: 提供操作DOM的react扩展库
* babel.min.js: 解析JSX语法代码转为纯JS语法代码的库

```html
<script type="text/javascript" src="../js/react.development.js"></script>
<script type="text/javascript" src="../js/react-dom.development.js"></script>
<script type="text/javascript" src="../js/babel.min.js"></script>
<script type="text/babel"> // 必须声明babel
  // 1. 创建虚拟DOM元素
  const vDom = <h1>Hello React</h1> // jsx语法
  // 2. 渲染虚拟DOM到页面真实DOM容器中
  ReactDOM.render(vDom, document.getElementById('test'))
</script>
```

虚拟Dom

* 虚拟DOM对象最终都会被React转换为真实的DOM
* 编码时基本只需要操作react的虚拟DOM相关数据, react会转换为真实DOM变化而更新界面

### jsx

全称:  JavaScript XML，react定义的一种类似于XML的JS扩展语法: XML+JS。用来创建react虚拟DOM(元素)对象。

```react
var ele = <h1 id='myTitle'>{title}</h1>
```

注意：

* 不是字符串, 也不是HTML/XML标签
* 最终产生的就是一个JS对象

jsx语法规则：

* 标签名任意: html标签或其它标签
* 标签属性任意: html标签属性或其它
* 遇到 <开头的代码, 以标签的语法解析: html同名标签转换为html同名元素, 其它标签需要特别解析。
* 遇到以 { 开头的代码，以js语法解析: 标签中的js代码必须用{ }包含

babel.js 作用：

* 浏览器不能直接解析jsx代码, 需要babel转译为纯js的代码才能运行。

* 只要用了jsx，都要加上type="text/babel"，声明需要babel来处理。

### 组件

#### 模块与组件

模块：向外提供特定功能的 js 程序, 一般就是一个 js 文件。

组件：用来实现特定(局部)功能效果的代码集合，包括：html、css和js三个部分。

组件化：当应用是以多组件的方式实现,，这个应用就是一个组件化的应用。

模块和组件都实现了简化和复用编码，提高运行效率的作用。

#### React中定义组件

每个组件只能有一个根标签

```react
// 1. 定义组件
// 方式1: 工厂函数组件(简单组件)
function MyComponent () {
  return <h2>工厂函数组件(简单组件)</h2>
}

// 方式2: ES6类组件(复杂组件)
class MyComponent2 extends React.Component {
  render () { // 继承父类方法，在渲染时会被回调
    console.log(this) // MyComponent2的实例对象
    return <h2>ES6类组件(复杂组件)</h2>
  }
}

// 2. 渲染组件标签
ReactDOM.render(<MyComponent />, document.getElementById('example1'))
ReactDOM.render(<MyComponent2 />, document.getElementById('example2'))
```

### React组件三大特性

#### state

state是组件对象最重要的属性，是对象类型（可以包含多个数据）。组件被称为“状态机”，通过更新组件的state来更新对应的页面显示（重新渲染组件）。

```react
class Like extends React.Component {
  constructor (props) {
    super(props)
    // 初始化状态
    this.state = {
      isLikeMe: true
    }
    
    this.change = this.change.bind(this) // 将自定义的函数强制绑定为组件对象，绑定this为组件对象。
  }
  change () { 
    this.setState({
      isLikeMe: !this.state.isLikeMe // 更新状态，更新状态必须调用setState函数，否则不会映射到dom对象上。
    })
  }
  render () {
    console.log('render()')
    const text = this.state.isLikeMe ? '你喜欢我' : '我喜欢你'
    return <h2 onClick={this.change}>{text}</h2>
  }
}
ReactDOM.render(<Like />, document.getElementById('example'))
```

#### props

组件标签的属性保存在props中，通过标签属性从组件外向组件内传递数据，注意: ==组件内部不可修改props数据。==props发生变化组件会重新渲染。

```react
// 1. 定义组件类
class Person extends React.Component {
  render() {
    return ( // 渲染属性数据
      <ul>
        <li>姓名: {this.props.name}</li>
        <li>性别: {this.props.sex}</li>
        <li>年龄: {this.props.age}</li>
      </ul>
    )
  }
}

// 对标签属性进行限制
Person.propTypes = {
  name: PropTypes.string.isRequired,
  sex: PropTypes.string,
  age: PropTypes.number
}

// 指定属性的默认值
Person.defaultProps = {
  sex: '男',
  age: 18
}

// 2. 渲染组件标签
const person = {
  name: 'Tom',
  sex: '女',
  age: 18
}
ReactDOM.render(<Person {...person}/>, document.getElementById('example1')) // 将数据传递到组件内部

const person2 = {
  myName: 'JACK',
  age: 17
}
// 标签里定义一个属性名名，如name、age等，props中就对应一个相应的属性。
ReactDOM.render(<Person name={person2.myName} age={person2.age}/>, document.getElementById('example2'))
```

#### refs

组件内的标签都可以定义ref属性来标识自己，通过ref获取组件内容特定标签对象，进行读取其相关数据

```react
// 定义组件
class MyComponent extends React.Component {
  constructor(props) {
    super(props) 
    this.handleClick = this.handleClick.bind(this)
  }
  
  handleClick () {
    alert(this.msgInput.value) // 通过属性获取标签信息
  }
  
  handleBlur (event) {
    alert(event.target.value) // 通过event函数获取输入信息
  }
  
  render () {
    return ( // 将input输入框绑定为this.msgInput属性
      <div>
        <input type="text" ref={input => this.msgInput = input}/>{' '} 
        <button onClick={this.handleClick}>提示输入数据</button>{' '}
        <input type="text" placeholder="失去焦点提示数据" onBlur={this.handleBlur}/>
      </div>
    )
  }
}

// 渲染组件标签
ReactDOM.render(<MyComponent />, document.getElementById('example'))
```

### 组件化编码思想

* 编码流程
  1. 将页面拆分为不同的组件组件。
  2. 使用组件实现静态页面效果（实现静态组件）。
  3. 实现动态组件：
     * 动态显示初始化数据
     * 交互功能（从绑定事件监听开始）

* 数据存储原则
  1. 只有单个组件使用，保存在该组件内。
  2. 多个组件使用，保存在共同的父组件内。
  3. 数据在哪，更新数据的行为（函数）就应该定义在哪。
  4. 不要在子组件中直接修改父组件的数据，通过相应的方法修改数据（即在父组件中定义函数，传递给子组件，在子组件中调用）。

### 组件分类

* 受控组件：表单项输入数据能自动收集成状态，需要对组件进行特殊编程处理。受控组件将dom和状态深度绑定，数据的修改直接反应在状态上。
* 非受控组件：主要是操作组件，需要时才手动读取表单输入框中的数据。通过dom的原生语法获取数据。

```react
class LoginForm extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      username: ''
    }
    this.handleSubmit = this.handleSubmit.bind(this)
    this.handleChange = this.handleChange.bind(this)
  }

  handleChange(event) {
    this.setState({username: event.target.value}) // 受控组件实时更新状态
  }

  handleSubmit(event) {
    alert(`准备提交的用户名为: ${this.state.username}, 密码:${this.pwdInput.value}`)

    // 阻止事件的默认行为: 提交表单
    event.preventDefault()
  }
  render () {
    return ( // 用户名为受控组件，实现了onChange方法，数据可以实时传递到状态中；密码为非受控组件。
      <form onSubmit={this.handleSubmit} action="/test">
        <label>
          用户名: <input type="text" value={this.state.username} onChange={this.handleChange} />
        </label>&nbsp;
        <label>
          密码: <input type="password" ref={(input) => this.pwdInput = input} />
        </label>&nbsp;
        <input type="submit" value="登陆" />
      </form>
    )
  }
}

ReactDOM.render(<LoginForm />, document.getElementById('example'))
```

### 组件的生命周期

<img src="https://yangandmore.github.io/img/ReactLifecycle/0.png" style="zoom:55%;" />

移除组件：`ReactDOM.unmountComponentAtNode(containerDom)`会回调`componentWillUnmount()`方法

#### 监控组件状态改变

```react
class UserList extends React.Component {

  static propTypes = {
    searchName: PropTypes.string.isRequired
  }

  componentWillReceiveProps(nextProps)  { // 当外部传入的props状态发生改变时回调
    let searchName = nextProps.searchName 
    const url = `https://api.github.com/search/users?q=${searchName}`
    this.setState({ firstView: false, loading: true })
    axios.get(url) // 状态改变，使用axios库发送请求
      .then((response) => {
        console.log(response)
      })
      .catch((error)=>{
        // debugger
        console.log('error', error.response.data.message, error.message)
      })
  }

  render () {
		return <h2>Enter name to search</h2>
  }
}
```

### Dom Diff

react用dom diff算法最小化更新dom对象，实现局部页面重绘。

<img src="https://i0.wp.com/programmingwithmosh.com/wp-content/uploads/2018/11/lnrn_0201.png?ssl=1" style="zoom: 40%;" />

虚拟dom数据保存在内存中，为真实dom关系的映射，只有真实dom相应很少的属性。程序中操作的是虚拟dom，react框架自动的会将虚拟dom的数据映射到真实dom中。

## React项目管理和插件

### 开发环境搭建

1. 安装脚手架`cnpm install -g create-react-app `，注意更新脚手架工具的版本。
2. 初始化命令 `create-react-app my-app`，注意使用代理。
3. 在`my-app`目录下执行`npm start`运行项目
4. 是vite初始化react项目

项目目录

```shell
.
├── README.md # 帮助文档
├── node_modules
├── package-lock.json
├── package.json # 项目依赖和命令
├── public # 项目公共文件夹
│   ├── favicon.ico
│   ├── index.html # html主文件 其中<div id="root"></div>是项目主节点
│   ├── manifest.json
│   └── robots.txt
└── src
    ├── App.css
    ├── App.js	# 组件
    ├── App.test.js
    ├── index.css
    ├── index.js	# 入口js，webpack打包程序
    ├── logo.svg
    ├── serviceWorker.js
    └── setupTests.js
```

### react-router

单页Web应用（single page web application，SPA），整个应用只有一个完整的页面。点击页面中的链接不会刷新页面，本身也不会向服务器发请求。当点击路由链接时，只会做页面的局部更新，数据都需要通过ajax请求获取，并在前端异步展现。

react的一个插件库，专门用来实现一个SPA应用。

#### 路由

一个路由就是一个映射关系`key:value`，key为路由路径，value可能是function/component（函数或组件）

路由分类

* 后台路由（服务器端路由）：value是function，用来处理客户端提交的请求并返回一个响应数据。
  * 注册路由（node示例）`router.get(path, function(req, res))`
  * 当node接收到一个请求时，根据请求路径找到匹配的路由，调用路由中的函数来处理请求，返回响应数据
* 前端路由（浏览器端路由）：value是component，当请求的是路由path时，浏览器端前没有发送http请求，但界面会更新显示对应的组件。
  * 注册路由： `<Route path="/about" component={About}>`
  * 当浏览器的hash变为#about时，当前路由组件就会变为About组件。

前端路由是通过封装history api实现路由功能。

#### 功能模块

##### 路由组件

* 路由器组件
  * `<BrowserRouter>`设置根标签为路由app
  * `<HashRouter>`

* `<Route>` 路由组件

* `<Redirect>`重定向组件

* `<Link>`路由连接，路由页面数据变化。

* `<NavLink>`导航连接，跳转路由页面的按钮。

* `<Switch>`切换组件，表示显示部分，切换的内容。

##### 对象和函数

* history对象

* match对象

* withRouter函数

#### 使用路由

安装路由插件`npm install --save react-router`

```shell
# 文件结构
├── src
    ├── components # 非路由组件
    └── views # 路由组件，或称pages
```

1. 设置入口标签，在index.js文件中修改

```react
ReactDOM.render(( // 设置路由app标签
    <BrowserRouter>
      <App/>
    </BrowserRouter>
    /*<HashRouter>
      <App />
    </HashRouter>*/
  ), document.getElementById('root')
)
```

2. 设置显示部分

```react
export default class App extends React.Component {
  render() {
    return (
        <div className="row"> {/*导航路由链接*/}
          <div className="list-group"> {/*react中设置class使用className*/}
            <NavLink className="list-group-item" to='/about'>About</MyNavLink> {/*to 表示要跳转的链接*/}
            <NavLink className="list-group-item" to='/home'>Home</MyNavLink>
          </div>
          <div className="panel-body">
            <Switch> {/*可切换的路由组件*/}
              <Route path='/about' component={About}/> {/*path组件映射的路径，component显示的组件*/}
              <Route path='/home' component={Home}/>
              <Redirect to='/about'/> {/*初始化为about组件*/}
            </Switch>
          </div>
        </div>
    )
  }
}
```

3. 定义路由子组件：1. 子组件中没有路由页面，即为一般的react组件。2.子组件中有路由页面。

```react
export default function Home() {
  return (
    <div>
      <ul className="nav nav-tabs">
        <li> <NavLink to='/home/news'>News</NavLink> </li> {/*路由组件中嵌套路由，需要填写父组件的路径*/}
        <li> <NavLink to="/home/message">Message</NavLink> </li>
      </ul>
      <Switch>
        <Route path='/home/news' component={News} />
        <Route path='/home/message' component={Message} />
        <Redirect to='/home/news'/>
      </Switch>
    </div>
  )
}
```

#### 路由间数据传递

发送数据

```react
export default class Message extends React.Component {
  state = {
    messages: []
  }

  componentDidMount () {
    setTimeout(() => { // 模拟发送ajax请求
      const data = [
        {id: 1, title: 'Message001'},
        {id: 3, title: 'Message003'},
        {id: 6, title: 'Message006'},
      ]
      this.setState({ messages: data }) // 设置状态
    }, 1000)
  }

  ShowDetail = (id) => {
    this.props.history.push(`/home/message/${id}`) // props中自带history对象
    // this.props.history.replace(`/home/message/${id}`)
  }

  back = () => {
    this.props.history.goBack()
  }

  forward = () => {
    this.props.history.goForward()
  }

  render () {
    const path = this.props.match.path // 获得组件路径

    return (
      <div>
        <ul>
          {
            this.state.messages.map((m, index) => {
              return (
                <li key={index}>
                  <Link to={`${path}/${m.id}`}>{m.title}</Link> {/*路由连接最后传入参数*/}
                  <button onClick={() => this.ShowDetail(m.id)}>查看详情</button>
                </li>
              )
            })
          }
        </ul>
        <p>
          <button onClick={this.back}>返回</button>
          <button onClick={this.forward}>前进</button>
        </p>
        <Route path={`${path}/:id`} component={MessageDetail}></Route> {/*组件接收数据，:id表示占位符*/}
      </div>
    )
  }
}
```

接收信息组件

```react
export default function MessageDetail(props) {
  const id = props.match.params.id // 获得路径参数
  return (
    <ul> <li>ID: {md.id}</li> </ul>
  )
}
```

### 组件间通信

#### 通过props传递数据

常用于父子组件之间的通信。

1. 共同的数据放在父组件states中。
2. 通过props可以传递一般数据和函数数据，只能一层一层传递。
   * 一般数据：父组件通过props传递给子组件，子组件读取数据。
   * 函数数据：子组件调用函数，修改父组件的数据，即修改states

#### 使用消息订阅、发布机制

常用于兄弟组件之间的通信。订阅消息等价于绑定事件监听，发布消息相当于触发回调函数。

使用工具库：PubSubJS，安装`npm install pubsub-js --save`，使用流程：

1. 引入`import PubSub from 'pubsub-js'`
2. 订阅消息`PubSub.subscribe('delete', function(msg, data){ });` 接收data数据。
3. 发布消息`PubSub.publish('delete', data)` ，data是需要传递的数据。

### redux

redux是状态管理的JS库，它可以用在react、angular和vue等项目中。但基本与react配合使用，集中管理多个组件共享的状态，从而实现组件间通信。

<img src="http://www.ruanyifeng.com/blogimg/asset/2016/bg2016091802.jpg" style="zoom:75%;" />

redux的使用

* 总体原则，尽量不使用redux。
* 状态需要共享，且在多处使用。
* 组件需要改变全局状态。
* 一个组件需要改变另一个组件的状态。

库安装`npm install --save redux`

#### 核心概念

action，标识要执行行为的对象，包含2个方面的属性。

* type：标识属性，值为字符串，唯一必要属性。
* data：数据属性，值类型任意，可选属性。

reducer，根据老的state和action，产生新的state的纯函数。

将state，action与reducer联系在一起的对象。

#### redux基本使用

目录结构

```shell
.
├── components # 组件
├── index.js
└── redux # redux 使用文件
    ├── action-types.js # action类型常量文件
    ├── actions.js # 修改状态函数
    └── reducers.js # reducer函数文件
```

1. 定义action类型常量，在action-types.js中

```react
export const INCREMENT = 'increment'
export const DECREMENT = 'decrement'
```

2. 创建reducer函数，在reducers.js中

```react
import {INCREMENT, DECREMENT} from './action-types'

export function counter(state = 0, action) {
  switch (action.type) {
    case INCREMENT:
      return state + action.data
    case DECREMENT:
      return state - action.data
    default:
      return state
  }
}
```

3. 在index.js中创建store对象

```react
import {createStore} from 'redux'

import {counter} from './redux/reducers'

const store = createStore(counter) // 使用reducer函数注册状态，内部首次调用reducer函数得到初始state
const render = () => {
  ReactDOM.render(<App store={store}/>, document.getElementById('root')) // 将store传入app内部
}
render()
store.subscribe(render) // 注册(订阅)监听, 一旦状态发生改变, 自动重新渲染
```

4. 在程序中显示状态

```react
this.props.store.getState()
```

5. 定义action函数，在actions.js文件中

```react
import {INCREMENT, DECREMENT} from './action-types'

export const increment = number => ({type: INCREMENT, data: number})
export const decrement = number => ({type: DECREMENT, data: number})
```

6. 修改状态，调用action函数

```react
this.props.store.dispatch(actions.decrement(number))
```

#### react-redux使用

一个react插件库，专门用来简化react应用中使用redux。安装`npm install --save react-redux`

1. 使用react-redux包装app界面，在index.js文件中修改。

```react

import {createStore} from 'redux'
import {Provider} from 'react-redux'
import {counter} from './redux/reducers'

// 根据counter函数创建store对象
const store = createStore(counter)

// 定义渲染根组件标签的函数
ReactDOM.render(
  (<Provider store={store}> <App/> </Provider>), // 使用Provider来包装app，并将store传递给Provider
  document.getElementById('root')
)
```

2. 使用连接将app与store和action连接

```react
// 包含Counter组件的容器组件
import React from 'react'
// 引入连接函数
import {connect} from 'react-redux'
// 引入action函数
import {increment, decrement} from '../redux/actions'
import Counter from '../components/counter'

// 向外暴露连接App组件的包装组件
export default connect(
  state => ({count: state}), {increment, decrement}
)(Counter) // Counter为真正app
```

3. 在项目中直接使用

```react
// 在props中直接声明store和action
static propTypes = {
  count: PropTypes.number.isRequired,
  increment: PropTypes.func.isRequired,
  decrement: PropTypes.func.isRequired
}

// 直接调用props中的变量
this.props.increment(number) 
this.props.decrement(number)
let count = this.props.count
```

React-Redux将所有组件分成两大类UI组件和容器组件

* UI组件：只负责 UI 的呈现，不使用任何 Redux 的 API，通过props接收数据（一般数据和函数），一般保存在components文件夹下。
* 容器组件：不负责UI的呈现，使用 Redux 的 API，一般保存在containers文件夹下。

#### redux异步编程

使用中间件`npm install --save redux-thunk`

1. 创建store时使用异步中间间。

```react
export default createStore(
  reducers,
  applyMiddleware(thunk) // 应用上异步中间件
)
```

2. 创建异步action

```react
export const incrementAsync = number => {
  return dispatch => {
    setTimeout(() => {
      dispatch(increment(number))
    }, 1000)
  }
}
```

3. 连接方法和app，并在组件中使用，流程同上。

#### 使用redux插件

chrome安装插件，并安装依赖包`npm install --save-dev redux-devtools-extension`

```react
// 使用开发依赖

import {composeWithDevTools} from 'redux-devtools-extension'

export default createStore(
  reducers,
  composeWithDevTools(applyMiddleware(thunk)) // 使用开发依赖包
)
```

## React Hooks

Hook可以在不编写 class 的情况下使用 state 以及其他的 React 特性。

[官方文档](https://zh-hans.reactjs.org/docs/hooks-intro.html)

###  `useState`

```react
const [modals, setModals] = useState({
  isCompanySearchShow: false,
  isTortEntityShow: false,
});
// modals 状态值，状态值可以为任何类型
// setModals 设置modals状态值

setModals({ ...modals, isCompanySearchShow: true }); // 设置modals部分值
// useState 传入函数是惰性初始化，函数在页面加载完成后会背自动调用
```

### `useEffect`

useEffect第二个参数表示在组件挂载和卸载时调用。

```react
// 监视状态值的effect
useEffect(() => {
  setSelectItem([]);
  if (data.meta.law_status === -1)
    setOperationDisables([false, false, false, true]);
  else setOperationDisables([]);
}, [data.meta.law_status]); 

// 组件挂载成功后调用请求
useEffect(() => {
  dispatch({
    type: 'RightGuardClueModel/fetchRightGuardClue',
    payload: pk,
  });
}, []);
```

### `useContext`

```react
// 1. 定义context类型
type MenuContextType = {
  setMenu?: any;
};

// 2. 创建上下文context
export const MenuContext = React.createContext<MenuContextType>({});

// 3. 调用上下文环境并传入相关数据值
const [menu, setMenu] = useState<[]>([]);

<MenuContext.Provider value={{ setMenu }}>
  <Content className={styles.content}>{children}</Content>
</MenuContext.Provider>

// 4. 在子组件中获取上下文中的参数和变量
const { setMenu } = useContext(MenuContext);
```

### `useMemo`

```tsx
// 函数和依赖项数组作为参数传入 useMemo，某个依赖项改变时重新计算memo的值
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]); 
// a, b 发生变化，函数会被调用，重新计算得到 memoizedValue，如果 memoizedValue 是组件中的状态，组件将被重新渲染
```

### 自定义hook

自定义hook必须使用use开始，在自定义hook中可以调用其他hook。

```react
import { useEffect, useState } from 'react';

export const useFillHeight = (known = 0) => {
  const [fillHeight, setFillHeight] = useState(0); // 调用useState

  function adjustHeight() {
    setFillHeight(document.documentElement.clientHeight - known);
  }

  useEffect(() => { // 调用useEffect
    adjustHeight();
    window.onresize = () => {
      adjustHeight();
    };
  }, []);

  return fillHeight;
};

```

### React夸组件状态管理

简易处理

* 状态提升
* 组合组件

缓存状态

* react-query

客户端状态

* 使用url
* redux
* context

组件间通讯

* pubsub-js

## React UI

常用react-ui

* web端ui：1. material-ui；2. ant-design
* 移动端ui：ant-design-mobile

## React 项目实践

### 初始化项目配置

使用 react-app 创建项目，使用typescript模板，命令：`tyarn create react-app jira --template typescript`

项目目录

```shell
├── README.md
├── node_modules
├── package.json
├── public # 不参与打包的静态文件
│   ├── favicon.ico
│   ├── index.html
│   ├── logo192.png
│   ├── logo512.png
│   ├── manifest.json
│   └── robots.txt # 搜索引擎设置
├── src
│   ├── App.css
│   ├── App.test.tsx
│   ├── App.tsx
│   ├── index.css
│   ├── index.tsx
│   ├── logo.svg
│   ├── react-app-env.d.ts # 类型声明定义文件
│   ├── reportWebVitals.ts # 埋点测试
│   └── setupTests.ts
├── tsconfig.json # typescript 配置
└── yarn.lock
```

#### 项目配置优化

* 配置文件查找路径，在tsconfig中

```json
{
  "compilerOptions": {
    "baseUrl": "./src",
  }
}
```

* 使用prettier配置文件提交后格式化的样式

  1. 安装文件 `tyarn add --dev --exact prettier`
  2. 创建文件`echo {}> .prettierrc.json`
  3. 添加需要忽略的文件，创建`.prettierignore `文件添加要忽略的文件夹 `\build \coverage`
  3. 添加相关包 `tyarn add husky lint-staged -D`
  4. 自动格式化命令`npx mrm@2 lint-staged` 添加 lint-staged，在package.json中添加 如下配置

  ```json
  {
    "husky": {
      "hooks": {
        "pre-commit": "lint-staged"
      }
    },
    "lint-staged": {
      "*.{js,css,md,tsx,ts}": "prettier --write" // 需要增加tsx,ts配置
    }
  }
  ```

  5. 解决 prettier 与 eslint 冲突 `tyarn add -D eslint-config-prettier `

  ```json
  {
    "eslintConfig": {
      "extends": [
        "react-app",
        "react-app/jest",
        "prettier" // 在eslint中增加prettier配置
      ]
    },
  }
  ```

  6. 在 pre-commit 文件中添加路径信息

  ```shell
  #!/bin/sh
  . "$(dirname "$0")/_/husky.sh"
  export PATH="/Users/xusu/.nvm/versions/node/v16.13.1/bin:$PATH" # nvm 下 npm 路径配置
  
  npx lint-staged
  ```

* 选择安装 commitlint 规范git提交信息

#### Mock配置

使用 json-server mock 数据，安装 `yarn add -D json-server`

创建文件夹 `__json_server_mock__`，添加 db.json 文件

在 package.json 添加命令

```json
{
  "scripts": {
    "json-server": "json-server __json_server_mock__/db.json --watch --port 3001"
  },
}
```

### 使用技巧

```typescript
import { Select } from 'antd';

type SelectProps = React.ComponentProps<typeof Select> // 提取组件属性
```

### 常用库

* dayjs可以代替momentjs
* react-helmet 可以用来处理页面标签的显示
* why-did-you-render 可以查看组件渲染动作
* code send box 代码测试环境

### 组件

#### 行对齐组件

```tsx
import styled from "@emotion/styled";

// 所有子元素都有右边距，对齐方式可以选择平均分配，自定义底边边距
export const Row = styled.div<{
  gap?: number | boolean;
  between?: boolean;
  marginBottom?: number;
}>`
  display: flex;
  align-items: center;
  justify-content: ${(props) => (props.between ? "space-between" : undefined)};
  > * {
    margin-top: 0 !important;
    margin-bottom: 0 !important;
    margin-right: ${(props) =>
      typeof props.gap === "number"
        ? props.gap + "rem"
        : props.gap
        ? "2rem"
        : undefined};
  }
`;
```

#### SVG组件

```tsx
import { ReactComponent as SoftWareLogo } from "assets/software-logo.svg";

<SoftWareLogo width={"18rem"} color={"rgb(38, 132, 255)"} /> // 将svg当组件使用，可以添加样式
```

#### 错误边界

自定义错误边界处理

```tsx
import React from 'react'

export type ErrorProps = { error:  Error | null}
type Props = React.PropsWithChildren<{ fallbackRender: FallbackRender }> // 在children属性上增加属性，React提供方法
type FallbackRender = (props: ErrorProps) => React.ReactElement

export class ErrorBoundary extends React.Component<Props, ErrorProps> {
    state = { error: null }

    static getDerivedStateFromError(error: Error) {
        return {error}
    }

    render() {
        const { error } = this.state
        const { fallbackRender, children } = this.props
        if (error) {
            return fallbackRender({error})
        }
        return children
    }
}
```

可以使用`react-error-boundary`库来处理错误边界
