##### 前端请求流程

在Ant Design Pro中，一个完整的前端UI交互到服务端处理流程是这样的：

1. UI组件交互操作；
2. 调用model的effect;
3. 调用统一管理的service请求函数；
4. 使用封装的request.ts发送请求；
5. 获取服务端返回；
6. 然后调用reducer改变state;
7. 更新model。

其中，`utils/request.ts` 是基于 [fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) 的封装，便于统一处理 POST，GET 等请求参数，请求头，以及错误提示信息等。

###### 处理异步请求

在处理复杂的异步请求的时候，很容易让逻辑混乱，陷入嵌套陷阱，所以Ant Design Pro的底层基础框架dva使用effect的方式来管理同步化异步请求。

通过generator和yield使得异步调用的逻辑处理跟同步一样。

同源策略全称叫《浏览器的同源策略》，它是浏览器内建的一种安全机制。那么我们不要使用浏览器请求就能完美解决问题了。对于前端来说最方便的自然就是node.js了。
