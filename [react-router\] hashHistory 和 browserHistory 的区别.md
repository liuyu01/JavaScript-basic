##### [react-router\] hashHistory 和 browserHistory 的区别

react-router提供了三种方式来实现路由，并没有默认的路由，需要在声明路由的时候，显式指定所使用的路由。

- browserHistory
- hashHistory
- createMemoryHistory

官方推荐使用browserHistory

使用hashHistory，浏览器的url是这样的：/#/user/liuna?_k=adsels

在url的前面会有一个#  可以理解为锚点 每个路由前面都会有# 这样不至于路由出问题找不到页面

优点：不需要后端配置

使用browserHistory，它是使用浏览器内置的 [History](https://link.jianshu.com/?t=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FAPI%2FHistory) API 实现的。浏览器的url是这样的：/user/liuna

路由是怎么样的就会展示什么样的

这样看起来当然是browserHistory更好一些，但是它需要server端支持。同时，后端也需要特殊配置，来处理这些可能发送到后端的路由，不光是因为用户可能手动刷新页面，还有另外一个更重要的原因。在不支持 History 特性的旧式浏览器中，react-router就会直接重新加载页面（显然后端必须识别这些路由）。

使用hashHistory时，因为有#的存在，浏览器不会发送request，react-router自己根据url去render相应的模块。

使用browserHistory时，从/到user/liuna，浏览器会向server发送request，所以server要做特殊请求，比如用的express的话，你需要handle所有的路由，`app.get('*', (req, res) => { ... })`，使用了 nginx 的话，nginx也要做相应的配置。

如果只是静态页面，就不需要用browserHistory,直接hashHistory就好了。
