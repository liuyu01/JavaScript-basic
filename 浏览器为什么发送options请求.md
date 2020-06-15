##### 浏览器为什么发送options请求

跨域请求中，options请求是浏览器自发起的preflight request(预检请求)，以检测实际请求是否可以被浏览器接受。

preflight request请求报文中有两个需要关注的首部字段：

（1）Access-Control-Request-Method:告知服务器实际请求所用的HTTP方法；

（2）Access-Control-Request-Headers:告知服务器实际请求所携带的自定义首部字段。

同时服务器也会添加origin header，告知服务器实际请求的客户端的地址。服务器基于从预检请求获得的信息来判断，是否接受接下来的实际请求。

服务器所返回的Access-Control-Allow-Methods首部字段将所有允许的请求方法告知客户端，返回将所有Access-Control-Request-Headers首部字段将所有允许的自定义首部字段告知客户端。此外，服务器端可返回Access-Control-Max-Age首部字段，允许浏览器在指定时间内，无需再发送预检请求，直接用本次结果即可。

在我们开发过程中出现的浏览器自发起的options请求就是上面的第二种情况。实际上，跨域请求中的“复杂请求”发出前会进行一次方法是options的preflight request。

我们会发现，在很多post,put,delete等请求之前，会有一次options请求。

根本原因就是，W3C规范这样要求了！在跨域请求中，分为简单请求（get和部分post，post时content-type属于application/x-www-form-urlencoded，multipart/form-data，text/plain中的一种）和复杂请求。而复杂请求发出之前，就会出现一次options请求。

什么是options请求呢？它是一种探测性的请求，通过这个方法，客户端可以在采取具体资源请求之前，决定对该资源采取何种必要措施，或者了解服务器的性能。

在ajax中出现options请求，也是一种提前探测的情况，ajax跨域请求时，如果请求的是json，就属于复杂请求，因此需要提前发出一次options请求，用以检查请求是否是可靠安全的，如果options获得的回应是拒绝性质的，比如404\403\500等http状态，就会停止post、put等请求的发出。

虽然在下面的参考文献中有人提出可以取消options请求，但是实测后发现是不行的，jquery封装之后，更不能轻易取消。因此，靠javascript客户端取消options请求是不可能的，只能通过服务端对options请求做出正确的回应，这样才能保证options请求之后，post、put等请求可以被发出。但是，我们不能允许所有的options请求，而应该是有条件的，所以最好是通过一个特殊的机制，去验证客户端发出的options请求数据是否是符合服务端的条件的，如果不满足，返回403，则客户端会取消原有的post计划。

前台跨域post请求，由于CORS（cross origin resource share）规范的存在，浏览器会首先发送一次options嗅探，同时header带上origin，判断是否有跨域请求权限，服务器响应access control allow origin的值，供浏览器与origin匹配，如果匹配则正式发送post请求。

如果有服务器程序权限，设置，比如jsp中，设置header access control allow origin等于*，就可以得到跨域访问的目的。

##### 请问http请求header中referer和origin的区别?

origin主要是用来说明最初请求是从哪里发起的；
origin只用于Post请求，而Referer则用于所有类型的请求；
origin的方式比Referer更安全点吧。

##### origin,referer和host区别

1. Host
   描述请求将被发送的目的地，包括，且仅仅包括域名和端口号。
   在任何类型请求中，request都会包含此header信息。

2. Origin
   用来说明请求从哪里发起的，包括，且仅仅包括协议和域名。
   这个参数一般只存在于CORS跨域请求中，可以看到response有对应的header：Access-Control-Allow-Origin。

3. Referer
   告知服务器请求的原始资源的URI，其用于所有类型的请求，并且包括：协议+域名+查询参数（注意，不包含锚点信息）。
