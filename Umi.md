##### G2

G2一套面向常规统计图表，以数据驱动的高交互可视化图形语法，具有高度的易用性和扩展性。使用G2，你可以无需关注图表各种繁琐的实现细节，一条语句即可使用Canvas或SVG构建出各种各样的可交互的统计图表。

##### Umi是什么？

Umi，中文可发音为乌米，是可扩展的企业级前端应用框架。Umi以路由为基础的，同时支持配置式路由和约定式路由，保证路由的功能完备，并以此进行功能扩展。然后配以生命周期完善的插件体系，覆盖从源码到构建产物的每个生命周期，支持各种功能扩展和业务需求。

Umi是蚂蚁金服的底层前端框架，已直接或间接地服务了3000+应用，包括java、node、H5无线、离线（Hybrid）应用、纯前端assets应用、CMS应用等。他已经很好地服务了我们的内部用户，同时希望他也能服务好外部用户。

它主要具备一下功能：

- 可扩展：Umi实现了完整的生命周期，并使其插件化，Umi内部功能也全由插件完成。此外还支持插件和插件集，以满足功能和垂直域的分层需求。
- 开箱即用：Umi内置了路由、构建、部署、测试等，仅需一个依赖即可上手开发。并且还提供针对React的集成插件集，内涵丰富的功能，可满足日常80%的开发需求。
- 企业级：经蚂蚁内部3000+项目以及阿里、优酷、网易、飞猪、口碑等公司项目的验证，值得信赖。
- 大量自研：包含微前端、组件打包、文档工具、请求库、hooks库、数据流等，满足日常项目的周边需求。
- 完备路由：同时支持配置式路由和约定式路由，同时保持功能的完备性，比如动态路由、嵌套路由、权限路由等等。
- 面向未来：在满足需求的同时，我们也不会停止对新技术的探索。比如dll提速、modern mode、webpack@5、自动化external、bundler less等等。

##### 为什么不是 ？

###### create-react-app

create-react-app 是基于webpack的打包层方案，包含build、dev、lint等，他在打包层把体验做到了极致，但是不包含路由、不是框架、也不支持配置。所以，如果大家想基于他修改部分配置，或者希望在打包层之外也做技术收敛时，就会遇到困难。

###### next.js

next.js是个很好的选择，Umi很多功能是参考next.js做的。要说有哪些地方不如Umi，我觉得可能是不够贴近业务、不够接地气。比如antd、dva的深度整合，比如国际化、权限、数据流、配置式路由、不定方案、自动化external方面等等一线开发者才会遇到的问题。

##### 配置式路由和约定式路由

Umi的路由既支持配置式，又支持约定式。配置式是对于现实的低头，也是大部分用户在用的，因为他功能强大；约定式是我们希望走去的方向，因为他简洁优雅。

##### .umi临时文件

.umi临时目录是整个Umi项目的发动机，你的入口文件、路由等等都在这里，这些是由umi内部插件及三方插件生成的。

临时文件是Umi框架中非常重要的一部分，框架或插件会根据你的代码生成临时文件，这些原来需要放在项目里的脏乱差的部分都被藏在了这里。

不要提交.umi目录到git仓库，他们会在umi dev 和umi build时被删除并重新生成。

##### pages目录

所有路由组件存放在这里。

##### app.ts

运行时配置文件，可以在这里扩展运行时的能力，比如修改路由、修改render方法等。

##### 运行时配置

运行时配置和配置的区别是他跑在浏览器端，基于此，我们可以在这里写函数、jsx、import浏览器依赖等等，注意不要引入node依赖。

##### 配置方式

约定src/app.tsx为运行时配置。

#### 路由

在Umi中，应用都是单页应用，页面地址的跳转都是在浏览器端完成的，不会重新请求服务端获取html，html只在应用初始化时加载一次。所有页面由不同的组件构成，页面的切换其实就是不同组件的切换，你只需要在配置中把不同的路由路径和对应的组件关联上。

##### 配置路由

在配置文件中通过routes进行配置，格式为路由信息的数组。

比如：

```
export default {
	routes: [
		{ exact: true, path: '/', component: 'index'},
		{ exact: true, path: '/user', component: 'user'}
	]
}
```

###### path

 type: string

###### component

type:string

配置location和path匹配后用于渲染的React组件路径。可以是绝对路径，也可以是相对路径，如果是相对路径，会从src/pages开始找起。

如果指向src目录的文件，可以用@,也可以用../。比如component:'@/layout.basic'，或者component:'../layouts/basic',推荐用前者。

###### exact

type:boolean  default:true

表示是否严格匹配，即location是否和path完全对应上。

###### routes

配置子路由，通常在需要为多个路径增加layout组件时使用。

比如：

```
export default {
	routes: [
		{ path: '/login', component: 'login'},
		{
			path: '/',
			component: '@/layouts/index',
			routes: [
				{ path: '/list', component: 'list'},
				{ path: '/admin', component: 'admin'},
			]
		}
	]
}
```

然后在src/layouts/index中通过props.children渲染子路由，

```
export default (props) => {
	return <div style={{ padding: 20}}>{props.children}</div>;
}
```

这样，访问/list和/admin就会带上src/layouts/index这个layout组件。

###### redirect

type: string

配置路由跳转

```
export default {
	routes：[
		{ exact: true, path: '/', redirect: '/list'},
		{ exact: true, path: '/list', component: 'list'},
	]
}
```

访问/会跳转到/list，并由src/pages/list文件进行渲染。

###### wrappers

type: string[]

配置路由的高阶组件封装。

比如，可以用于路由级别的权限校验

```
export default {
	routes: [
		{ path: '/user', component: 'user',
		  wrappers: [
		  	'@/wrappers/auth'
		  ],
		 },
		 { path: '/login', component: 'login'},
	]
}
```

然后在src/wrappers/auth中，

```
import {Redirect} from 'umi';

export default (props) => {
	const {isLogin} = useAuth();
	if(isLogin){
		return <div>{props.children}</div>;
	}else{
		return <Redirect to='/login'>;
	}
}
```

这样，访问/user，就通过useAuth做权限校验，如果通过，渲染src/pages/user，否则跳转到/login，由src/pages/login进行渲染。

###### title

type: string

配置路由的标题

##### 页面跳转

```
import { history } from ‘umi';

//跳转到指定路由
history.push('/list');

//带参数跳转到指定路由
history.push('/list?a=b');
history.push({
	pathname: '/list',
	query: {
		a: 'b',
	},
});

//跳转到上一个路由
history.goBack();
```

##### Link组件

```
import { Link } from 'umi';

export default () => (
  <div>
    <Link to="/users">Users Page</Link>
  </div>
);
```

然后点击Users Page就会跳转到/users地址。

注意：Link只用于单页应用的内部跳转，如果是外部地址跳转请使用a标签。

##### 路由组件参数

路由组件可通过props获取到以下属性，

- match，当前路由和url match后的对象，包含params、path、url和 isExact属性
- location，表示应用当前处于哪个位置，包含pathname、search、query等属性
- history
- route，当前路由配置，包含path、exact、component、routes等
- routes，全部路由信息

##### 传递参数给子路由

通过cloneElement,一次就好（Umi 2时需要两次）。

```
import React from 'react';

export default function Layout(props) {
  return React.Children.map(props.children, child => {
    return React.cloneElement(child, { foo: 'bar' });
  });
}
```

##### 约定式路由

除配置式路由外，Umi也支持约定式路由。约定式路由也叫文件路由，就是不需要手写配置，文件系统即路由，通过目录和文件及其命名分析出路由配置。

**如果没有routes配置，Umi会进入约定式路由模式**，然后分析src/pages目录拿到路由配置。

比如以下文件结构：

```
└── pages
    ├── index.tsx
    └── users.tsx
```

会得到以下路由配置，

```
[
	{ exact: true, path: '/', component: '@/pages/index'},
	{ exact: true, path:'/users', component: '@/pages/users'},
]
```

需要注意的是，满足以下任意规则的文件不会被注册为路由，

- 以 .  或  _开头的文件或目录
- 以d.ts结尾的类型定义文件
- 以test.ts、spec.ts、e2e.ts结尾的测试文件（适用于.js、.jsx和.tsx文件）
- components和component目录
- utils和util目录
- 不是.js、.jsx、.ts或.tsx文件
- 文件内容不包括JSX元素

###### 动态路由

约定[]包裹的文件或文件夹为动态路由。

比如：

- src/pages/users/[id].tsx 会成为 /users/:id
- src/pages/users/[id]/settings.tsx 会成为 /users/:id/settings

举个完整的例子，比如以下文件结构，

```bash
.
  └── pages
    └── [post]
      ├── index.tsx
      └── comments.tsx
    └── users
      └── [id].tsx
    └── index.tsx
```

会生成路由配置，

```js
[
  { exact: true, path: '/', component: '@/pages/index' },
  { exact: true, path: '/users/:id', component: '@/pages/users/[id]' },
  { exact: true, path: '/:post/', component: '@/pages/[post]/index' },
  {
    exact: true,
    path: '/:post/comments',
    component: '@/pages/[post]/comments',
  },
];
```

###### 动态可选路由

约定[ $]包裹的文件或文件夹为动态可选路由。

比如：

- src/pages/users/[id$].tsx 会成为 /users/:id?
- src/pages/users/[id$]/settings.tsx 会成为 /users/:id?/settings

举个完整的例子，比如以下文件结构，

```bash
.
  └── pages
    └── [post$]
      └── comments.tsx
    └── users
      └── [id$].tsx
    └── index.tsx
```

会生成路由配置，

```js
[
  { exact: true, path: '/', component: '@/pages/index' },
  { exact: true, path: '/users/:id?', component: '@/pages/users/[id$]' },
  {
    exact: true,
    path: '/:post?/comments',
    component: '@/pages/[post$]/comments',
  },
];
```

###### 嵌套路由

Umi里约定目录下有_layout.tsx时会生成嵌套路由，以 _layout.tsx为该目录的layout。layout文件需要返回一个React组件，并通过props.children渲染子组件。

比如以下目录结构，

```bash
.
└── pages
    └── users
        ├── _layout.tsx
        ├── index.tsx
        └── list.tsx
```

会生成路由，

```js
[
  { exact: false, path: '/users', component: '@/pages/users/_layout',
    routes: [
      { exact: true, path: '/users', component: '@/pages/users/index' },
      { exact: true, path: '/users/list', component: '@/pages/users/list' },
    ]
  }
]
```

###### 全局layout

约定 src/layouts/index.tsx为全局路由。返回一个React组件，并通过props.children渲染子组件。

比如以下目录结构，

```bash
.
└── src
    ├── layouts
    │   └── index.tsx
    └── pages
        ├── index.tsx
        └── users.tsx
```

会生成路由，

```js
[
  { exact: false, path: '/', component: '@/layouts/index',
    routes: [
      { exact: true, path: '/', component: '@/pages/index' },
      { exact: true, path: '/users', component: '@/pages/users' },
    ],
  },
]
```

一个自定义的全局 `layout` 如下：

```tsx
import { IRouteComponentProps } from 'umi'
export default function Layout({ children, location, route, history, match }: IRouteComponentProps) {  return children}
```

###### 不同的全局layout

你可能需要针对不同路由输出不同的全局layout，Umi不支持这样的配置，但你仍可以在src/layouts/index.tsx中对location.path做区分，渲染不同的layout。

比如想要针对 `/login` 输出简单布局，

```js
export default function(props) {
  if (props.location.pathname === '/login') {
    return <SimpleLayout>{ props.children }</SimpleLayout>
  }

  return (
    <>
      <Header />
      { props.children }
      <Footer />
    </>
  );
}
```

###### 404路由

约定src/pages/404.tsx为404页面，需返回React组件。

比如以下目录结构，

```bash
.
└── pages
    ├── 404.tsx
    ├── index.tsx
    └── users.tsx
```

会生成路由，

```js
[
  { exact: true, path: '/', component: '@/pages/index' },
  { exact: true, path: '/users', component: '@/pages/users' },
  { component: '@/pages/404' },
]
```

这样，如果访问 `/foo`，`/` 和 `/users` 都不能匹配，会 fallback 到 404 路由，通过 `src/pages/404.tsx` 进行渲染。

###### 权限路由

通过指定高阶组件wrappers达成效果。

如下，`src/pages/user`：

```js
import React from 'react'
function User() {  
    return <>user profile</>
}
User.wrappers = ['@/wrappers/auth']
export default User
```

然后在 `src/wrappers/auth` 中，

```jsx
import { Redirect } from 'umi'

export default (props) => {
  const { isLogin } = useAuth();
  if (isLogin) {
    return <div>{ props.children }</div>;
  } else {
    return <Redirect to="/login" />;
  }
}
```

这样，访问 `/user`，就通过 `useAuth` 做权限校验，如果通过，渲染 `src/pages/user`，否则跳转到 `/login`，由 `src/pages/login` 进行渲染。

###### 扩展路由属性

支持在代码层通过导出静态属性的方式扩展路由。

比如：

```js
function HomePage() {  
    return <h1>Home Page</h1>;
}
HomePage.title = 'Home Page';
export default HomePage;
```

其中的 `title` 会附加到路由配置中。

##### 页面跳转

在Umi里，页面之间跳转有两种方式：声明式和命令式。

###### 声明式

通过Link使用，通常作为React组件使用。

```
import { Link } from ’umi';

export default () => {
	<Link to='/list'>Go to list page</Link>
}
```

###### 命令式

通过history使用，通常在事件处理中被调用。

```
import { history } from 'umi';

function goToListPage() {
	history.push('./list');
}
```

也可以直接从组件的属性中取得history

```
export default (props) => {
	<Button onClick={()=>props.history.push("/list")}>Go to list page</Button>
}
```

##### Mock数据

Mock数据是前端开发过程中必不可少的一环，是分离前后端开发的关键链路。通过预先跟服务器端约定好的接口，模拟请求数据甚至逻辑，能够让前端开发独立自主，不会被服务端的开发所阻塞。

###### 约定Mock文件

Umi约定/mock文件夹下所有文件为mock文件

如：

```bash
.
├── mock
    ├── api.ts
    └── users.ts
└── src
    └── pages
        └── index.tsx
```

`/mock` 下的 `api.ts` 和 `users.ts` 会被解析为 mock 文件。

Mock.js是常用的辅助生成模拟数据的三方库，借助他可以提升我们的mock数据能力。

##### 命令行工具

###### umi build

编译构建web产物。通常需要针对部署环境，做特定的配置和环境变量修改。

默认产物输出到项目的dist文件夹，你可以通过修改哦诶之outputPath指定产物输出目录。默认编译时会将public文件夹内的所有文件，原样拷贝到dist目录，如果你不需要这个特性，乐意通过配置chainWebpack来删除它。

###### umi dev

启动本地开发服务器进行项目的开发调试

启动在浏览器中运行的开发服务器，并监视源文件变化，自动热加载。

默认使用8000端口，如果8000端口被占用，将会使用8001端口，一次类推。你可以通过设置环境变量PORT来指定开发端口号。

###### umi generate

内置 生成器功能，内置的类型有page，用于生成最简压面。支持别名调用 umi g。

###### umi plugin

快速查看当前项目使用到的所有的umi插件。

###### umi help

umi命令行的简易帮助文档。

###### umi version

查看当前使用的umi的版本号，可以使用别名-v调用。

###### umi webpack

查看umi使用的webpack配置。

##### 使用CSS

Umi中约定src/global.css为全局样式，如果存在此文件，会被自动引入到入口文件最前面。

比如用于覆盖样式，

```css
.ant-select-selection {
  max-height: 51px;
  overflow: auto;
}
```

###### CSS Modules

Umi会自动识别CSS Modules的使用，你把他当做CSS Modules用时才是CSS Modules。

比如：

```js
// CSS Modules
import styles from './foo.css';

// 非 CSS Modules
import './foo.css';
```

###### CSS预处理器

Umi内置支持less，不支持sass和stylus，但如果有需求，可以通过chainWebpack配置或umi插件的形式支持。

##### 部署

###### 默认方案

Umi默认对新手友好，所以默认不做按需加载处理， umi build 后输出index.html 、umi.js和umi.css三个文件。

###### 不输出html文件

某些场景html文件交给后端输出，前端构建并不需要输出html文件，可配置环境变量HTML=none实现。

$ HTML=none umi build

##### 服务端渲染（SSR）

###### 什么事服务端渲染？

服务端渲染（Server-Side Rendering）,是指由**服务侧**完成页面的HTML结构拼接的页面处理技术，发送到浏览器，然后为其绑定状态与事件，成为完全可交互页面的过程。

###### SSR常用于以下两个场景：

1.有**SEO诉求**，用在搜索引擎检索以及社交分享，用在前台类应用。

2.**首屏渲染**时长有要求，常用在移动端，弱网情况下。

###### 什么事预渲染？

服务端渲染，首先得有后端服务器（一般是Node.js）才可以使用，如果我没有后端服务器，也想在上面提到的两个场景，那么推荐使用预渲染。

预渲染与服务端渲染唯一的不同点在于**渲染时机**，服务端渲染的时机是在用户访问时执行渲染（即**服务时渲染**，数据一般是最新的），预渲染的时机是在项目构建时，当用户访问时，数据不一定是最新的（如果数据没有实时性，则可以直接考虑预渲染）。

预渲染（Pre Render）在构建时执行渲染，将渲染后的HTML片段生成静态HTML文件。无需使用web服务器实时动态编译HTML，适用于**静态站点生成**。

##  Umi 服务端渲染特性

> 早在 Umi 2.8+ 版本时，Umi 已具备 SSR 能力，只是使用上对新手而言，门槛较高。

Umi 3 结合自身业务场景，在 SSR 上做了大量优化及开发体验的提升，具有以下特性：

- **开箱即用**：内置 SSR，一键开启，`umi dev` 即 SSR 预览，开发调试方便。
- **服务端框架无关**：Umi 不耦合服务端框架（例如 [Egg.js](https://eggjs.org/)、[Express](https://expressjs.com/)、[Koa](https://koajs.com/)），无论是哪种框架或者 Serverless 模式，都可以非常简单进行集成。
- **支持应用和页面级数据预获取**：Umi 3 中延续了 Umi 2 中的页面数据预获取（getInitialProps），来解决之前全局数据的获取问题。
- **支持按需加载**：按需加载 `dynamicImport` 开启后，Umi 3 中会根据不同路由加载对应的资源文件（css/js）。
- **内置预渲染功能**：Umi 3 中内置了预渲染功能，不再通过安装额外插件使用，同时开启 `ssr` 和 `exportStatic`，在 `umi build` 构建时会编译出渲染后的 HTML。
- **支持渲染降级**：优先使用 SSR，如果服务端渲染失败，自动降级为客户端渲染（CSR），不影响正常业务流程。
- **支持流式渲染**：`ssr: { mode: 'stream' }` 即可开启流式渲染，流式 SSR 较正常 SSR 有更少的 [TTFB](https://baike.baidu.com/item/TTFB)（发出页面请求到接收到应答数据第一个字节所花费的毫秒数） 时间。
- **兼容客户端动态加载**：在 Umi 2 中同时使用 SSR 和 dynamicImport（动态加载）会有一些问题，在 Umi 3 中可同时开启使用。
- **SSR 功能插件化**：Umi 3 内置的 SSR 功能基本够用，若不满足需求或者想自定义渲染方法，可通过提供的 API 来自定义。

![preview](https://pic4.zhimg.com/v2-98c7d2c7974a1aa694203e3135531e3b_r.jpg)

从以上表格来看，两者方案的优点和确定但很明显，**SSR更有利于首屏渲染，CSR更有利于页面交互。**

查看网页源代码，如果 `<div id="root">` DOM 里的元素不为空，则是 SSR，否则为 CSR。
