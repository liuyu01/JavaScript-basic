##### Ant Design

###### 如何正确的拼写 Ant Design

- ✅ **Ant Design**：用空格分隔的首字母大写单词，指代设计语言。
- ✅ **antd**：全小写，指代 React UI 组件库。
- ✅ **ant.design**：特指 ant.design 网站网址。

###### Ant Design of React

antd 是基于Ant Design 设计体系的React UI组件库，主要用于研发企业级中后台产品。

###### 特性

- 提炼自企业级中后台产品的交互语言和视觉风格
- 开箱即用的高质量React组件
- 使用TypeScript开发，提供完整的类型定义文件
- 全链路开发和设计工具体系
- 数十个国际化语言支持
- 深入每个细节的主题定制能力

###### 兼容环境

- 现代浏览器和IE11(需要polyfills)
- 支持服务端渲染
- Electron

对于 IE 系列浏览器，需要提供相应的 Polyfill 支持，建议使用 [@babel/preset-env](https://babeljs.io/docs/en/babel-preset-env) 来解决浏览器兼容问题。如果你在使用 [umi](http://umijs.org/)，可以直接使用 [targets](https://umijs.org/zh/config/#targets) 配置。

`antd@3.x` 对 React 15/16 两个版本提供支持，但是我们强烈建议你升级到 React 16，以便获得更好的性能和遇到更少的问题。

> `antd@2.0` 之后不再支持 IE8，`antd@4.0` 之后不再支持 IE9/10。

##### 组件：

###### 通用

Button 按钮 、Icon 图标 、Typography 排版

###### 布局

Divider 分割线 、Grid 栅格 、Layout 布局 、Space 间隔

###### 导航

Affix 固钉 、Breadcrumb 面包屑  、Dropdown 下拉菜单 、Menu 导航菜单 、Pagination 分页 、

PageHeader页头 、Steps 步骤条

###### 数据录入

AutoComplete 自动完成 、Checkbox 多选框 、Cascader 级联选择 、DataPicker 日期选择框 、Form 表单

、InputNumber 数字输入框 、Input 输入框 、Mentions 提及 、Rate 评分 、Radio 单选框 、Switch 开关 、

Slider 滑动输入条 、Select 选择器 、TreeSelect 树选择 、Transfer 穿梭框 、TimePicker 时间选择框 、

Upload 上传

###### 数据展示

Avatar 头像 、Badge 徽标数 、Comment 评论 、Collapse 折叠面板 、Carousel 走马灯 、Card 卡片 、Calendar 日历 、Descriptions 描述列表 、Empty 空状态 、List 列表 、Popover 气泡卡片 、 Statistic 统计数值 、Tree 树形控件 、Tooltip 文字提示 、Timeline 时间轴 、Tag 标签 、Tabs 标签页 、Table 表格、

###### 反馈

Alert 警告提示 、Drawer 抽屉 、Modal 对话框 、Message 全局提示 、Notification 通知提醒框 、Progress 进度条 、Popconfirm 气泡确认框 、Result 结果 、Spin 加载 、Skeleton 骨架屏

###### 其他

Anchor 锚点 、BackTop 回到顶部 、ConfigProvider 全局化配置

##### Ant Design Pro

Ant Design Pro是一个企业级中后台前端/设计解决方案，秉承Ant Design 的设计价值观，致力于在设计规范和基础组件的基础上，继续向上构建，提炼出典型模板/业务组件/配套设计资源，进一步提升企业级中后台产品设计研发过程中的用户和设计者的体验。

##### **可以理解为 Ant Design 是一套 组件库，而 Pro 是使用了这套组件库的完整前端脚手架。** 

**Ant Design**更多是着重致力于设计规范和基础组件

**Ant Design Pro** 直接提炼出典型模板/业务组件/配套设计资源。你不需要重复设计那些复用性高的业务组件，直接引用集合好的业务组件，省事又省力。

**Ant Design Pro** 面向的人群更多企业的中后台开发，企业的后台产品对界面的设计要求并没有那么严格，因为不是面向大众使用的，一般也不过于追求 UI视觉效果，更多的是需要一些基础性功能，它的对于数据的显示和操作交互要求更高。而**Ant Design Pro** 直接把**典型模板/业务组件/配套的设计资源全部给你做齐了**，等于说组件全部都封装好了，你只需要按照**Ant Design Pro** 中给到的配套资源和一套基于 Vue 的中后台管理控制台的脚手架，直接进行搭建就好了，这样可以极大的缩减开发人员的时间成本。

##### Ant Design React 和 Ant Design Pro 有什么区别？

可以理解为 Ant Design React 是一套 React 组件库，而 Pro 是使用了这套组件库的完整前端脚手架。

##### 脚手架

前端开发中提到的“脚手架”是一个形象的比喻，比喻各类语言的前期工作环境。

在软件开发上（当然也包括前端开发）的脚手架指的就是：有人帮你把这个开发过程中要用到的工具、环境都配置好了，你就可以方便地直接开始做开发，专注你的业务，而不用再花时间去配置这个开发环境，这个开发环境就是脚手架。
比如vue.js就有个vue-cli脚手架，基于node.js的开发环境，作者帮你把开发环境大部分东西都配置好了，你把脚手架下载下来就可以直接开发了，不用再考虑搭建这些工具环境。

脚手架就可以帮你减少这些为**减少重复性工作而做的重复性工作**. 脚手架一个命令，目录结构、gulp脚本、babel配置、空的测试文件都帮你搞好了. 直接写核心业务代码，不做重复性工作，这就是脚手架的作用.

脚手架在[前端](http://www.fly63.com/)工作流中负责项目起始阶段创建初始文件。与其他功能模块不同的是，脚手架是一个完全“启下”的模块，它没有任何前置依赖。创建完成项目初始文件之后，脚手架就再无用武之地了。

在实际的开发过程中，从零开始建立项目的结构是一件让人头疼的事情，所以各种各样的脚手架[工具](http://www.fly63.com/tool)应运而生。它们功能丰富，但**最核心的功能都是能够快速搭建一个完整的项目的结构，开发者只需要在生成的项目结构的基础上进行开发即可，**非常简单高效。

**脚手架的作用：**

1、减少时间，不必从零开始搭建初始项目，提高开发效率。

2、便于多人协作。

3、项目更新同步方便，只需要更新[代码](http://www.fly63.com/tag/代码)库中项目模板，即可下载最新的项目。

脚手架本质上就是一套工具，由于在web2.0时代，应用变复杂后，出现了很多可以让前端开发效率提升的框架和标准及工具等等，可能这些新的代码方式远行环境还不支持，也许我们需要一个本地测试环境和运行环境及调试环境等，所以需要一套完整的工具帮我们处理问题及项目构建。

###### 介绍

Ant Design Pro 最新技术栈 : 使用 React/dva/antd 等前端前沿技术开发

Ant Design Pro 使用 [Umi](https://umijs.org/) 作为开发工具

###### 同源策略

在MDN中我们可以看到关于同源策略是一个安全机制。详细的说明如下：

同源策略限制了从同一个源加载的文档或脚本如何与来自另一个源的资源进行交互。这是一个用于隔离潜在恶意文件的重要安全机制。

这个机制本身出发点是很好的，但是同源的限制非常严格，url，端口任一不同都会造成跨域错误。

同源策略全称叫《浏览器的同源策略》，它是浏览器内建的一种安全机制。那么我们不要使用浏览器请求就能完美解决问题了。对于前端来说最方便的自然就是node.js了。

###### CORS 

可以使用CORS的方式来允许跨域调用。

###### 如何使用 Ant Design Pro 布局

通常布局是和路由系统紧密结合的，Ant Design Pro 的路由使用了 Umi 的路由方案，我们将配置信息统一抽离到 `config/config.ts` 下

###### less

Ant Design Pro 默认使用 less 作为样式语言

###### 和服务端进行交互

Ant Design Pro 是一套基于 React 技术栈的单页面应用，我们提供的是前端代码和本地模拟数据的开发模式，通过 API 的形式和任何技术栈的服务端应用一起工作。下面将简单介绍和服务端交互的基本写法。

###### 前端请求流程

在 Ant Design Pro 中，一个完整的前端 UI 交互到服务端处理流程是这样的：

1. UI 组件交互操作；
2. 调用 model 的 effect； 
3. 调用统一管理的 service 请求函数；
4. 使用封装的 request.ts 发送请求； 
5. 获取服务端返回；
6. 然后调用 reducer 改变 state；
7. 更新 model。

其中，`utils/request.ts` 是基于 [fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) 的封装，便于统一处理 POST，GET 等请求参数，请求头，以及错误提示信息等。

###### Mock 和联调

Mock 数据是前端开发过程中必不可少的一环，是分离前后端开发的关键链路。通过预先跟服务器端约定好的接口，模拟请求数据甚至逻辑，能够让前端开发独立自主，不会被服务端的开发所阻塞。

在 Ant Design Pro 中，因为我们的底层框架是 umi，而它自带了代理请求功能，通过代理请求就能够轻松处理数据模拟的功能。

###### 什么是区块

区块是研发资产的一种，它是一系列快速搭建页面的代码片段，它可以帮助你快速的在项目中初始化好一个页面，帮助你更快速的开发代码。当前的区块都是页面级别的区块，你可以理解为它是一些项目中经常会用到的典型页面的模板，使用区块其实相当于从已有的项目中复制一些页面的代码到你当前的项目中。

- 以前开发一个页面：创建 JS -> 创建 CSS -> 创建 Model -> 创建 service -> 写页面组件。
- 现在开发一个页面：下载区块 -> 基于区块初始化好的页面组件修改代码。

###### 区块和模板

在 Pro 中资产被分为了两种，区块和模板。区块可以类比为一个组件，而模板代表一个页面。区块现在支持所有 antd 中的 demo，可以更加快速的将 demo 导入到项目中去。

###### 前端路由

Ant Design Pro 使用的 Umi 支持两种路由方式：`browserHistory` 和 `hashHistory`。

`hashHistory` 使用如 `https://cdn.com/#/users/123` 这样的 URL，取井号后面的字符作为路径。`browserHistory` 则直接使用 `https://cdn.com/users/123` 这样的 URL。使用 `hashHistory` 时浏览器访问到的始终都是根目录下 `index.html`。使用 `browserHistory` 则需要服务器做好处理 URL 的准备，处理应用启动最初的 `/` 这样的请求应该没问题，但当用户来回跳转并在 `/users/123` 刷新时，服务器就会收到来自 `/users/123` 的请求，这时你需要配置服务器能处理这个 URL 返回正确的 `index.html`。强烈推荐使用默认的 `browserHistory`。

###### Umi Ant Design Pro

使用 [umi 约定式路由](https://umijs.org/zh/guide/router.html#约定式路由)的 Ant Design Pro。

###### React Easy Start

基于 create-react-app 搭建的轻量级 Ant Design Pro 脚手架。

DVA是基于redux,redux-saga和react-router的轻量级前端框架。
