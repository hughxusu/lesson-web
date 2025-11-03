# UmiJS

## 快速上手

```shell
tyarn global add umi # 安装umi

# 创建文件夹，并切换到该路径
tyarn create @umijs/umi-app # 用模板初始化
tyarn # 下载项目包
# 修改package.json错误
tyarn start # 运行项目

umi g page xxx --typescript --less # 创建文件页面
```

### 目录结构

```shell
├── package.json
├── .umirc.ts # umi配置和config文件夹二选一
├── .env # 环境变量
├── config # 配置文件目录
├── dist
├── mock # 模拟数据
├── public # 此目录下所有文件会被 copy 到输出路径。
└── src
    ├── .umi # 临时文件夹，构建后删除，其中router文件是约定式路由生成文件
    ├── layouts/index.tsx # 约定式路由时的全局布局文件
    ├── pages # 路由组件
        ├── index.less
        └── index.tsx
    └── app.ts
```

### 打包发布

```shell
umi build # 打包项目
tyarn global add serve # 安装服务项目

serve ./dist # 启动打包服务
```

### 常见问题

1. 新建项目后果从umi导入connect、Reducer、Effect等报错，创建model文件后，无提示错错误，使用tyarn命令，在.umi文件夹下生产dva插件后即可。
2. vscode插件 ES7 React/Redux/GraphQL/React-Native snippets

## 路由

### 约定式路由

在pages文件夹下的数据会根据文件夹名称自动生成路由页面，同时会在.umi文件中生成一个`router/core/`文件夹下生成一个router文件，用来记录路由。

#### 动态路由

##### 动态路由文件

```shell
umi g page /users/'$'id --typescript --less # 创建命令

# 文件结构
├── users
    ├── $id.less # 动态路由配置
    ├── $id.txs
```

路由配置

```react
const routes = [
  {
    "path": "/users/:id", // 动态路由配置
    "exact": true,
    "component": require('@/pages/users/$id.tsx').default
  }
];
```

读取路由数据

```react
export default (props: any) => {
  const { match } = props; // 通过props可以读取出路由id数据
  return (
    <div>
      <h1 className={styles.title}>Page ID: {match.params.id}</h1>
    </div>
  );
}
```

##### 动态路由文件夹

```shell
umi g page '$'post/index --typescript --less # 创建命令

# 文件结构
├── $post # 动态路由配置
    ├── index.less 
    ├── index.txs
    ├── commit.less 
    ├── commit.txs
```

路由配置

```react
const routes = [
  {
    "path": "/:post/commit", // 可以进入 /1/commit页面，1为传入参数，可以问用户id
    "exact": true,
    "component": require('@/pages/$post/commit.tsx').default
  },
  {
    "path": "/:post",
    "exact": true,
    "component": require('@/pages/$post/index.tsx').default
  }
];
```

#### 嵌套路由

```shell
# 文件结构
├── users
    ├── $id.less # 动态路由配置
    ├── $id.txs
    ├── _layout.less # 嵌套路由页面，固定写法
    ├── _layout.txs
```

路由配置

```react
const routes = [
  {
    "path": "/users",
    "routes": [
      {
        "path": "/users/$id",
        "exact": true,
        "component": require('@/pages/users/$id.tsx').default
      },
      {
        "path": "/users",
        "exact": true,
        "component": require('@/pages/users/index.tsx').default
      }
    ],
    "component": require('@/pages/users/_layout.tsx').default
  }
```

#### 全局布局

```shell
└── src
    ├── layouts/index.tsx # 约定式路由时的全局布局文件，可以包含所有页面
```

过滤布局

```react
export default (props: any) => {
  const { location } = props;
  if (location.pathname === '/login') { // 对于特殊路由取消作用布局文件
    return <>{props.children}</>
  }

  return (
    <div>
      {props.children}
    </div>
  );
}
```

#### 404页面

在pages文件夹下创建 404.tsx 为默认的 404 页面

### 配置式路由

配置式路由和约定式路由是冲突的，使用配置路由约定路由失效。

### 常用API

```react
import { useHistory } from 'umi';
const { location } = useHistory()  // location可以获得当前路由信息
```



## Mock

mock数据保存在项目mock文件夹下，处理流程：

1. 创建mock文件。
2. 文件中定义mock接口。

```react
export default {
  // 请求路径: 返回数据
  'GET /api/getGeneralClueList': { clues: [1, 2] }, // 支持值为 Object 和 Array 
  '/api/users/1': { id: 1 }, // GET 可忽略
  'POST /api/users/create': (req, res) => { // 支持自定义函数，API 参考 express@4
    res.setHeader('Access-Control-Allow-Origin', '*'); // 添加跨域请求头
    res.end('ok');
  }
}

// 使用faker模拟数据
import Faker from 'faker';
const tikItems: TikItem[] = Array.from({ length: 20 }, () => {
  return {
    pk: Faker.random.number({min: 1}),
    time: Faker.date.recent().toISOString(),
    media: {
      pk: 1,
      name: Faker.name.findName(),
      code: Faker.internet.domainName(),
      avatar: Faker.internet.avatar(),
    },
  };
});

let tikList: TikList = {
  items: tikItems,
  total: 50,
  status: 200,
  meta: {
    page: 1,
    page_size: 20,
  },
};

export default {
  '/mock/tikItems': tikList,
};
```

3. 在请求中使用mock数据。

```react
export async function getGeneralClueList() {
  return read('/api/getGeneralClueList'); // 请求路由必须与mock路径一致
}
```

## dva

安装dva`yarn add @umijs/preset-react`，在umi中约定在src/models文件夹中定义model。

dva数据流图

<img src="https://zos.alipayobjects.com/rmsportal/PPrerEAKbIoDZYr.png" style="zoom:60%;" />

### 定义model层

1. namespace是model数据的唯一标识。
2. state是模型中的数据存储，是对象数据类型，每个键对应一种数据，可以采用预定义数据类型或typescript数据类型。
3. reducers界面更新函数
4. effects异步请求函数
5. subscriptions指定挂在界面函数，该函数会在界面挂在完整后自动调用。

```typescript
import { Effect, Reducer, Subscription } from 'umi';

export type ResponseState = { // 定义state的接口类型
  login: string;
}

type ResponseType = { // 定义模型类型
  namespace: 'response';
  state: ResponseState, // 数据仓库用于保存数据，state类型可以是基本数据类型或预定义类型
  reducers: { // 更新界面状态，封装了不同的函数
    getLogin: Reducer<ResponseState> // 定义一个更新函数，该函数为Reducer类型，泛型为state数据类型
  },
  effects: { // 异步请求，封装的Generator函数
    getRemote: Effect, // 定义一个异步函数，该函数类型为Effect，Effect函数没有泛型
  },
  subscriptions: { // 用于绑定指定的界面，当页面挂载完成时，会自动调用，用于自动请求数据
    setup: Subscription; // 定义了一个绑定函数，该函数类型为Subscription，Subscription函数没有泛型
  }
}

const ResponseModel: ResponseType = { // 实现了model该类型为预定义的类型ResponseType
  namespace: 'response', // model唯一标识
  state: { // 数据初始值，数据类型为预定义的类型
    login: '初始化数据'
  },
  reducers: {
    getLogin(state, { payload }) { // state更新前的状态数据，payload是调用者传递过来的数据
      return payload; // 返回处理后更新的状态，该状态类型为state的类型
    }
  },
  effects: { 
    *getRemote(anyAction, effectsCommandMap) { // 定义一个请求函数，每个函数都包括AnyAction和EffectsCommandMap两个参数
     	// AnyAction封装了请求参数通过调用方法发送给AnyAction
      let {call, put} = effectsCommandMap // EffectsCommandMap封装了一些方法用于调用请求和reducers中的函数
      const data = yield call(login, {username:'13146395839', password:'123456'}) // 调用请求
      if (data) {
        yield put({ // 调用reducers函数
          type: 'getLogin',
          payload: {
            login: JSON.stringify(data)
          }
        })
      }
    }
  },
  subscriptions: {
    setup(subscriptionAPI) { // subscriptionAPI中封装了dispatch和history两个变量
      let {dispatch, history} = subscriptionAPI
      history.listen((location) => { // history用于监听路由变化
        let {pathname} = location // 封装了pathname，hash，state等参数，pathname为当前的路由页面
        if (pathname === '/') {
          dispatch({ // dispatch用于调用model中的方法
            type: 'getRemote',
          })
        }
      });
    }
  }
}

export default ResponseModel

export const getLogin = (str: string) => ({ type: 'response/getLogin', payload: { login: str }}) // 将dispatch参数作为函数导出，方便调用
```

注意：模型中每个部分的关键字必须与上述内容一致，否则无法正常运行

### 连接model

通过umi的connect方法将组件与model相连接：

* connect 方法返回的也是一个React组件，通常称为容器组件。因为它是原始UI组件的容器，即在外面包了一层State。
* connect 方法传入的第一个参数是mapStateToProps函数，mapStateToProps函数会返回一个对象，用于建立State到Props的映射关系。

```typescript
import React, {FC} from 'react';
import { connect } from 'umi';

const Index:FC<any> = (props) => { // 包括mapStateToProps中映射的数据和其他数据
  return(<div>组件</div>);
}

const mapStateToProps = (params: any) => { // 将model中的state映射到组件中的函数
  return {
    response: params['response'] // 返回值是对象类型，键为数据名称，会被映射到组件的props中
  }
};

export default connect(mapStateToProps)(Index); // 通过connect将组件包装后暴露给外部使用，该组件是原始组件包装了model中的state
```

* params参数主要包括：1. loading状态参数；2.models中的全部state；3.router路由信息。只需要将页面需要的state映射到props中。
* props参数主要包括：1.mapStateToProps中映射的数据；2.children子元素；3.dispatch方法；4.路由信息和位置信息等。

### 使用model

```tsx
const index: React.FC = (props: any) => {
  const { response, dispatch } = props; // 使用model属性
  return ( // 调用model方法，dispatch 参数可以通过调用函数生成
    <>
    	{response.login}
			<Button onClick={() => {dispatch(getLogin('输入内容')}}>添加</Button> 
    </>
  )
}
```

## ant-design

### 入门

基本安装`npm install antd --save`

#### 修改组件样式

修改组件样式步骤：

1. 定义样式文件 `.less`文件。
2. 在组件中引入样式文件，通过`className={code}`使用代码使用样式。
3. 通过组件的`style={{}}`修改样式，第一个包括表示react代码，第二个括号表示数据数据是一个结构体。

样式文件

```less
.content {
  background: #fff;
  margin: 80px 16px 16px;
  min-height: 700px;
}
```

组件文件

```react
import styles from './welcome.less' // 引入样式文件

const Welcome: React.FC<any> = ({ children }) => {
  const { Content } = Layout;
  return (
    <Layout>
      <AdminHeader />
      <Content
        className={styles.content} // 使用样式文件
        style={{height: 1000, margin: '100px'}} // 直接使用样式，会覆盖样式文件中样式
      >
        <h1>content</h1>
      </Content>
      <AdminFooter />
    </Layout>
  );

export default Welcome;
```

### 组件

#### Menu

```react
import { Menu } from 'antd';

const { ItemGroup, Divider, Item } = Menu // menu常用子组件
const { location } = useHistory()

<Menu mode='inline' // 设置menu样式
selectedKeys={[location.pathname]}> // 设置默认选中，不用defaultSelectedKeys，默认选值使用当前路由
  {
  CourtList.map((court) => {
    const items = court.items;
    return (
      <ItemGroup
        key={court.group} // key 值使用动态值
        title={court.group}
        >
        <Divider/>
        {items.map((item) => {
          counter++;
          return (
            <Item // item的key使用路由路径
              key={item.path}> 
              <Link to={item.path}>{item.title}</Link>
            </Item>
          );
        })}
      </ItemGroup>
    );
  })
}
</Menu>
```

界面路由需要重定向到选中菜单位置

```typescript
{
  path: '/works/court',
  redirect: '/works/court/ImgUsing',
}
```

## ant-design-mobile

### 入门

基本安装`npm install antd-mobile --save`



## Ant Design Pro

### 安装

```shell
tyarn create umi myapp # 初始化项目
tyarn # 安装依赖包
tyarn start # 启动项目
```

