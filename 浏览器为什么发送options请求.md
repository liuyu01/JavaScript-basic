##### 浏览器为什么发送options请求

跨域请求中，options请求是浏览器自发起的preflight request(预检请求)，以检测实际请求是否可以被浏览器接受。

preflight request请求报文中有两个需要关注的首部字段：

（1）Access-Control-Request-Method:告知服务器实际请求所用的HTTP方法；

（2）Access-Control-Request-Headers:告知服务器实际请求所携带的自定义首部字段。

同时服务器也会添加origin header，告知服务器实际请求的客户端的地址。服务器基于从预检请求获得的信息来判断，是否接受接下来的实际请求。

服务器所返回的Access-Control-Allow-Methods首部字段将所有允许的请求方法告知客户端，返回将所有Access-Control-Request-Headers首部字段将所有允许的自定义首部字段告知客户端。此外，服务器端可返回Access-Control-Max-Age首部字段，允许浏览器在指定时间内，无需再发送预检请求，直接用本次结果即可。

在我们开发过程中出现的浏览器自发起的options请求就是上面的第二种情况。实际上，跨域请求中的“复杂请求”发出前会进行一次方法是options的preflight request。
