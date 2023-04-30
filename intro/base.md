# 前端开发基础知识

```mermaid
flowchart LR
    a(网页文件) --> b(浏览器) --> c(网站)
```

* 网页文件：按照一定规范编写的文本文件，包括——文字、图片、音频、视频、超链接。
* 浏览器：解析网页文件并渲染。
* 网站：满足用户交互需要或收集用户数据。

前端开发的工作就是编写网页文件。

## 浏览器

常见浏览器：谷歌浏览器(Chrome)、火狐浏览器(Firefox)、欧朋浏览器(Opera)、Safari浏览器、IE浏览器

![](https://s1.ax1x.com/2023/02/08/pSRrOYV.md.png)

渲染引擎(浏览器内核)：浏览器中专门对代码进行解析渲染的部分。

| 浏览器       | 内核    | 备注                                    |
| ------------ | ------- | --------------------------------------- |
| IE           | Trident | IE、猎豹安全、360极速浏览器、百度浏览器 |
| FireFox      | Gecko   | 火狐浏览器内核                          |
| Safari       | Webkit  | 苹果浏览器内核                          |
| Chrome/Opera | Blink   | Blink 其实是 Webkit 的分支              |

* 渲染引擎不同，导致解析相同代码时的 速度、性能、效果也不同。
* 前端开发基本使用 Chrome。

## Web标准

Web 标准：定义了浏览器的输入（用于浏览器渲染的网页规范）和输出（浏览器的渲染结果）。

Web 标准由三部分构成

| 功能 | 语言       | 说明                                     |
| ---- | ---------- | ---------------------------------------- |
| 结构 | HTML       | 页面元素和内容                           |
| 表现 | CSS        | 页面元素的样式（如：大小、位置和颜色等） |
| 行为 | JavaScript | 页面与用户的交互                         |

Web 标准要求页面实现:结构、表现、行为三层分离。

![](https://s1.ax1x.com/2023/02/08/pSRy1b9.jpg)

## 前端开发工具

<img src="https://s1.ax1x.com/2023/03/04/ppEn7w9.jpg" style="zoom: 20%;" />

Chrome 浏览器，设置为默认浏览器。

Vs Code 代码编辑器。

### VS Code 使用

安装 VS Code 插件

<img src="https://s1.ax1x.com/2023/02/08/pSRclkR.png" style="zoom:80%;" />

常用插件

| 插件                                                      | 作用               |
| --------------------------------------------------------- | ------------------ |
| Chinese (Simplified) Language Pack for Visual Studio Code | 简体中文界面       |
| open in browser                                           | 选择浏览器打开页面 |
| Auto Rename Tag                                           | html 重命名        |
| CSS Peek                                                  | 样式追踪           |

