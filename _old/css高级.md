# CSS 高级

## Less

Less是一种动态样式语言，是css预处理器，它扩展了 CSS 语言，增加了变量、Mixin、函数等特性，使 CSS 更易维护和扩展。

### 基本使用

```less
/*包裹注释会被编译到css文件中*/
//此类注释不会被编译到css文件中

#wrap{
    width: 300px;
    height: 400px;
    .inner{ // 可以通过嵌套结构实现子元素的选择
        height: 100px;
        width: 100px;
    }
}
```

#### 变量

变量加载时将全部预处理文件读取完成后替换。

```less
@color:deeppink; // 定义变量
@w:#warp
@h:height
// url作为变量使用与属性名一样
@{w}{ // 如果是属性名和选择器作为变量使用时需要{}
    width: 300px;
  	@{height}: 400px;
    .inner{
        background: @color; // 使用变量
        height: 100px;
        width: 100px;
    }
}
```

#### 嵌套规则

```less
#wrap{
    width: 300px;
    height: 400px;
    .inner{
        background: deeppink;
        height: 100px;
        width: 100px;
        &:hover{ // 表示平级选择，跟上面的选择器同级
            background: pink; 
        }
    }
}
```

#### 计算

```less
@rem:100rem;

#wrap .sjx{
   width:(100 + @rem)
}

#wrap .sjx{
   width:(100 + 100px) // 计算双方只有一方有单位即可
}
```

#### 免编译

```less
*{
    margin: 100 *  10px;
    padding: ~"cacl(100px + 100)"; // 当原生字符串输出不用编译
}
```

### 混合

```less
.fix { // 定义混合
    position: absolute;
    top: 0;
    bottom: 0;
    margin: auto;
    background: pink;
    height: 10px;
    width: 10px;
}

.main(@w:10px,@h:10px,@c:pink){ // 带参数混合混合，混合可以声明默认参数
    position: absolute;
    top: 0;
    bottom: 0;
    margin: auto;
    background: @c;
    height: @h;
    width: @w;
}

#wrap {
    width: 300px;
    height: 400px;
    .inner{ // 使用混合
       .main(100px ,100px, pink);
    }
    .sub{
       .fix; // 使用不但参数的混合
    }
  	.foo{
    	.main(@c:black); // 带命名参数的混合，与python指定变量名类型
  	}
}
```

#### 匹配模式

```less
.triangle(@_){
    width: 0px;
    height: 0px;
    overflow: hidden; 
}

.triangle(L,@w,@c){ // 调用该混合是会自动调用上面的混合，这样的混合就是匹配模式
    border-width: @w;
    border-style:dashed solid dashed dashed;
    border-color:transparent @c transparent transparent ;
}

.triangle(R,@w,@c){
    border-width: @w;
    border-style:dashed  dashed dashed solid;
    border-color:transparent transparent transparent @c;
}

@import "./triangle.less"; // 引入其他less文件
#wrap .sjx{
   .triangle(R,40px,yellow) // 调用混合
}
```

#### arguments变量

```less
.border(@w,@style,@c){
    border: @arguments; // 代替输入变量数组
}

#wrap .sjx{
   .border(1px,solid,black)
}
```

### 继承

Less继承不能带参数

```less
// 定义基类样式
.main{
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    top: 0;
    margin: auto;
}

.main:hover{
    background: red!important;
}


// 继承样式
@import "mixin/main.less";
#wrap{
    width: 300px;
    height: 300px;
    .inner:extend(.main) { // 使用样式继承
        &:nth-child(1) {
           width: 100px;
           height: 100px;
           background: pink;
        }
    }
}

#wrap{
    width: 300px;
    height: 300px;
    .inner { 
      	&:extend(.main all); // 继承main全部样式包括hover
        &:nth-child(1) {
           width: 100px;
           height: 100px;
           background: pink;
        }
    }
}

```

## Emotion(css-in-js)

### 使用流程

1. 安装依赖 `yarn add @emotion/react @emotion/styled @emotion/css`，web storm 安装插件 styled-components 

2. 安装 css props 依赖 `yarn add @emotion/babel-preset-css-prop -D`，并 umi 中配置插件依赖

   ```ts
   export default defineConfig({
     extraBabelPresets: [ // 配置插件依赖
       "@emotion/babel-preset-css-prop"
     ]
   });
   ```

3. 配置 tsx css 属性在 tsconfig中

   ```json
   {
     "compilerOptions": {
       "jsxImportSource": "@emotion/react"
     }
   }

在代码中使用

```tsx
import { css } from '@emotion/react';

let color = 1

function showColor() {
  switch (color) {
    default:
      return 'yellow'
    case 1:
      return 'red'
    case 2:
      return 'blue'
  }
}

const indexCss = css`
  background: ${showColor()};
`
export default function IndexPage() {
  return (
    <div >
      <h1 css={indexCss}>Page index</h1>
    </div>
  );
}

// 导出使用过样式的div组件
export const WhiteScreen = styled.div` 
  width: 100%;
  height: 100vh;
  background: white;
`;

// 对react组件使用样式，定义为新的组件
import { Row } from "components/lib";
const Header = styled(Row)`
  justify-content: space-between;
  padding: 3.2rem;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
`;

```

