node.js实现mock

Koa -- 基于Node.js平台的下一代web开发框架



需要使用 koa 、koa-bodyparser、koa-router、koa2-cors、mockjs2插件

 处理请求体 __koa-bodyparser__
\- 非GET请求，比如说post请求 ，包括表单提交的form内的数据，都能轻松获取
\- ctx.request.body 获取form中的数据

\- 处理路由 __koa-router__
\- 获取查询字符串 ctx.query
\- 获取/xxx/:id ctx.params.id
\- koa-bodyparser是解析请求体数据的，koa-router中可以通过ctx.query||ctx.params获取url上的参数

koa-router 一款为了koa设计的路由中间件，包含如下特性：

1. 使用app.get, app.put, app.post等方式的路由方式

2. URL命名参数

3. 路由名称根据URL生成

4. 路由嵌套

5. 多个路由中间件

中间件：处于 HTTP Request 和 HTTP Response 中间，用来实现某种中间功能， app.use() 用来加载中间件。

中间件默认有两个参数，`(Context, next)`, 只要调用 `next()` 就把执行权交给下一个中间件。

router.allowedMethods()作用： 这是官方文档的推荐用法,我们可以 看到 router.allowedMethods()用在了路由匹配 router.routes()之后,所以在当所有路由中间件最后调用.此时根据 ctx.status 设置 response 响应头 

```javascript
由于middleware的顺序很重要，这个koa-bodyparser必须在router之前被注册到app对象上
```

```
const Koa = require('koa');
const cors = require('koa2-cors');

const app = new Koa();
const router = require('koa-router')(); // 集中处理URL，根据不同的URL调用不同的处理函数
const bodyParser = require('koa-bodyparser'); // 解析原始的request请求，把解析的参数绑定到 ctx.request.body中

app.use(cors());
app.use(bodyParser());
app.use(router.routes());
app.use(router.allowedMethods());
```

###### koa2-cors 实现应答跨域请求

###### koa-bodyparser中间件

这个中间件可以将post请求的参数转为json格式返回

