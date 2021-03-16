##### HTTP技能之HTTP请求头响应头中常用字段详解

##### 常用标准请求字段

Accept设置接受的内容类型   

```
Accept:text/plain
```

Accept-Charset 设置接受的字符编码

```
Accept-Charset:utf-8
```

Accept-Encoding 设置接受的编码格式

```
Accept-Encoding:gzip, deflate
```

Accept-Datetime 设置接受的版本时间

```
Accept-Datetime: Thu, 31 May 2007 20:35:00 GMT
```

Accept-Language 设置接受的语言

```
Accept-Language: en-US
```

**Authorization**   设置HTTP身份验证的凭证

> Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==

**Cache-Control**   设置请求响应链上所有的缓存机制必须遵守的指令

> Cache-Control: no-cache

**Connection**  设置当前连接和hop-by-hop协议请求字段列表的控制选项

> Connection: keep-alive
>  Connection: Upgrade

**Content-Length**  设置请求体的字节长度

> Content-Length: 348

**Content-MD5** 设置基于MD5算法对请求体内容进行Base64二进制编码

> Content-MD5: Q2hlY2sgSW50ZWdyaXR5IQ==

**Content-Type**    设置请求体的MIME类型（适用POST和PUT请求）

> Content-Type: application/x-www-form-urlencoded

**Cookie**  设置服务器使用Set-Cookie发送的http cookie

> Cookie: $Version=1; Skin=new;

**Date**    设置消息发送的日期和时间

> Date: Tue, 15 Nov 1994 08:12:31 GMT

**Expect**  标识客户端需要的特殊浏览器行为

> Expect: 100-continue

**Forwarded**   披露客户端通过http代理连接web服务的源信息

> Forwarded: for=192.0.2.60;proto=http;by=203.0.113.43
>  Forwarded: for=192.0.2.43, for=198.51.100.17

**From**    设置发送请求的用户的email地址

> From: [user@example.com](https://link.jianshu.com?t=mailto:user@example.com)

**Host**    设置服务器域名和TCP端口号，如果使用的是服务请求标准端口号，端口号可以省略

> Host: en.wikipedia.org:8080
>  Host: en.wikipedia.org

**If-Match**    设置客户端的ETag,当时客户端ETag和服务器生成的ETag一致才执行，适用于更新自从上次更新之后没有改变的资源

> If-Match: "737060cd8c284d8af7ad3082f209582d

**If-Modified-Since**   设置更新时间，从更新时间到服务端接受请求这段时间内如果资源没有改变，允许服务端返回304 Not Modified

> If-Modified-Since: Sat, 29 Oct 1994 19:43:31 GMT

**If-None-Match**   设置客户端ETag，如果和服务端接受请求生成的ETage相同，允许服务端返回304 Not Modified

> If-None-Match: "737060cd8c284d8af7ad3082f209582d"

**If-Range**    设置客户端ETag，如果和服务端接受请求生成的ETage相同，返回缺失的实体部分；否则返回整个新的实体

> If-Range: "737060cd8c284d8af7ad3082f209582d"

**If-Unmodified-Since** 设置更新时间，只有从更新时间到服务端接受请求这段时间内实体没有改变，服务端才会发送响应

> If-Unmodified-Since: Sat, 29 Oct 1994 19:43:31 GMT

**Max-Forwards**    限制代理或网关转发消息的次数

> Max-Forwards: 10

**Origin**  标识跨域资源请求（请求服务端设置Access-Control-Allow-Origin响应字段）

> Origin: `http://www.example-social-network.com`

**Pragma**  设置特殊实现字段，可能会对请求响应链有多种影响

> Pragma: no-cache

**Proxy-Authorization** 为连接代理授权认证信息

> Proxy-Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==

**Range**   请求部分实体，设置请求实体的字节数范围，具体可以参见HTTP/1.1中的Byte serving

> Range: bytes=500-999

**Referer**     设置前一个页面的地址，并且前一个页面中的连接指向当前请求，意思就是如果当前请求是在A页面中发送的，那么referer就是A页面的url地址（轶事：这个单词正确的拼法应该是"referrer",但是在很多规范中都拼成了"referer"，所以这个单词也就成为标准用法）

> Referer: `http://en.wikipedia.org/wiki/Main_Page`

**TE**  设置用户代理期望接受的传输编码格式，和响应头中的Transfer-Encoding字段一样

> TE: trailers, deflate

**Upgrade** 请求服务端升级协议

> Upgrade: HTTP/2.0, HTTPS/1.3, IRC/6.9, RTA/x11, websocket

**User-Agent**  用户代理的字符串值

> User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:12.0) Gecko/20100101 Firefox/21.0

**Via** 通知服务器代理请求

> Via: 1.0 fred, 1.1 example.com (Apache/1.1)

**Warning** 实体可能会发生的问题的通用警告

> Warning: 199 Miscellaneous warning

##### 常用非标准请求头字段

------

**X-Requested-With**    标识Ajax请求，大部分js框架发送请求时都会设置它为XMLHttpRequest

> X-Requested-With: XMLHttpRequest

**DNT** 请求web应用禁用用户追踪

> DNT: 1 (Do Not Track Enabled)
>  DNT: 0 (Do Not Track Disabled)

**X-Forwarded-For** 一个事实标准，用来标识客户端通过HTTP代理或者负载均衡器连接的web服务器的原始IP地址

> X-Forwarded-For: client1, proxy1, proxy2
>  X-Forwarded-For: 129.78.138.66, 129.78.64.103

**X-Forwarded-Host**    一个事实标准，用来标识客户端在HTTP请求头中请求的原始host,因为主机名或者反向代理的端口可能与处理请求的原始服务器不同

> X-Forwarded-Host: en.wikipedia.org:8080
>  X-Forwarded-Host: en.wikipedia.org

**X-Forwarded-Proto**   一个事实标准，用来标识HTTP原始协议，因为反向代理或者负载均衡器和web服务器可能使用http,但是请求到反向代理使用的是https

> X-Forwarded-Proto: https

**Front-End-Https** 微软应用程序和负载均衡器使用的非标准header字段  Front-End-Https: on
 **X-Http-Method-Override**  请求web应用时，使用header字段中给定的方法（通常是put或者delete）覆盖请求中指定的方法（通常是post）,如果用户代理或者防火墙不支持直接使用put或者delete方法发送请求时，可以使用这个字段

> X-HTTP-Method-Override: DELETE

**X-ATT-DeviceId**  允许更简单的解析用户代理在AT&T设备上的MakeModel/Firmware

> X-Att-Deviceid: GT-P7320/P7320XXLPG

**X-Wap-Profile**   设置描述当前连接设备的详细信息的xml文件在网络中的位置

> x-wap-profile: `http://wap.samsungmobile.com/uaprof/SGH-I777.xml`

**Proxy-Connection**    早起HTTP版本中的一个误称，现在使用标准的connection字段

> Proxy-Connection: keep-alive

**X-UIDH**  服务端深度包检测插入的一个唯一ID标识Verizon Wireless的客户

> X-UIDH: ...

**X-Csrf-Token,X-CSRFToken,X-XSRF-TOKEN**   防止跨站请求伪造

> X-Csrf-Token: i8XNjC4b8KVok4uw5RftR38Wgp2BFwql

**X-Request-ID,X-Correlation-ID**   标识客户端和服务端的HTTP请求

> X-Request-ID: f058ebd6-02f7-4d3f-942e-904344e8cde5

##### 常用标准响应头字段

------

**Access-Control-Allow-Origin** 指定哪些站点可以参与跨站资源共享

> Access-Control-Allow-Origin: *

**Accept-Patch**    指定服务器支持的补丁文档格式，适用于http的patch方法

> Accept-Patch: text/example;charset=utf-8

**Accept-Ranges**   服务器通过byte serving支持的部分内容范围类型

> Accept-Ranges: bytes

**Age** 对象在代理缓存中暂存的秒数

> Age: 12

**Allow**   设置特定资源的有效行为，适用方法不被允许的http 405错误

> Allow: GET, HEAD

**Alt-Svc** 服务器使用"Alt-Svc"（Alternative Servicesde的缩写）头标识资源可以通过不同的网络位置或者不同的网络协议获取

> Alt-Svc: h2="http2.example.com:443"; ma=7200

**Cache-Control**   告诉服务端到客户端所有的缓存机制是否可以缓存这个对象，单位是秒

> Cache-Control: max-age=3600

**Connection**  设置当前连接和hop-by-hop协议请求字段列表的控制选项

> Connection: close

**Content-Disposition** 告诉客户端弹出一个文件下载框，并且可以指定下载文件名

> Content-Disposition: attachment; filename="fname.ext"

**Content-Encoding**    设置数据使用的编码类型

> Content-Encoding: gzip

**Content-Language**    为封闭内容设置自然语言或者目标用户语言

> Content-Language: en

**Content-Length**  响应体的字节长度

> Content-Length: 348

**Content-Location**    设置返回数据的另一个位置

> Content-Location: /index.htm

**Content-MD5** 设置基于MD5算法对响应体内容进行Base64二进制编码

> Content-MD5: Q2hlY2sgSW50ZWdyaXR5IQ==

**Content-Range**   标识响应体内容属于完整消息体中的那一部分

> Content-Range: bytes 21010-47021/47022

**Content-Type**    设置响应体的MIME类型

> Content-Type: text/html; charset=utf-8

**Date**    设置消息发送的日期和时间

> Date: Tue, 15 Nov 1994 08:12:31 GMT

**ETag**    特定版本资源的标识符，通常是消息摘要

> ETag: "737060cd8c284d8af7ad3082f209582d"

**Expires** 设置响应体的过期时间

> Expires: Thu, 01 Dec 1994 16:00:00 GMT

**Last-Modified**   设置请求对象最后一次的修改日期

> Last-Modified: Tue, 15 Nov 1994 12:45:26 GMT

**Link**    设置与其他资源的类型关系

> Link: </feed>; rel="alternate"

**Location**    在重定向中或者创建新资源时使用

> Location: `http://www.w3.org/pub/WWW/People.html`

**P3P** 以P3P:CP="your_compact_policy"的格式设置支持P3P(Platform for Privacy Preferences Project)策略，大部分浏览器没有完全支持P3P策略，许多站点设置假的策略内容欺骗支持P3P策略的浏览器以获取第三方cookie的授权

> P3P: CP="This is not a P3P policy! See `http://www.google.com/support/accounts/bin/answer.py?hl=en&answer=151657` for more info."

**Pragma**  设置特殊实现字段，可能会对请求响应链有多种影响

> Pragma: no-cache

**Proxy-Authenticate**  设置访问代理的请求权限

> Proxy-Authenticate: Basic

**Public-Key-Pins** 设置站点的授权TLS证书

> Public-Key-Pins: max-age=2592000; pin-sha256="E9CZ9INDbd+2eRQozYqqbQ2yXLVKB9+xcprMF+44U1g=";

**Refresh** "重定向或者新资源创建时使用，在页面的头部有个扩展可以实现相似的功能，并且大部分浏览器都支持
 `<meta http-equiv="refresh" content="5; url=http://example.com/">`

> Refresh: 5; url=`http://www.w3.org/pub/WWW/People.html`

**Retry-After** 如果实体暂时不可用，可以设置这个值让客户端重试，可以使用时间段（单位是秒）或者HTTP时间

> Example 1: Retry-After: 120
>  Example 2: Retry-After: Fri, 07 Nov 2014 23:59:59 GMT

**Server**  服务器名称

> Server: Apache/2.4.1 (Unix)

**Set-Cookie**  设置HTTP Cookie

> Set-Cookie: UserID=JohnDoe; Max-Age=3600; Version=1

**Status**  设置HTTP响应状态

> Status: 200 OK

**Strict-Transport-Security**   一种HSTS策略通知HTTP客户端缓存HTTPS策略多长时间以及是否应用到子域

> Strict-Transport-Security: max-age=16070400; includeSubDomains

**Trailer** 标识给定的header字段将展示在后续的chunked编码的消息中

> Trailer: Max-Forwards

**Transfer-Encoding**   设置传输实体的编码格式，目前支持的格式： chunked, compress, deflate, gzip, identity

> Transfer-Encoding: chunked

**TSV** Tracking Status Value，在响应中设置给DNT(do-not-track),可能的取值

 　　"!" — under construction

 　　"?" — dynamic

 　　"G" — gateway to multiple parties

 　　"N" — not tracking

 　　"T" — tracking

 　　"C" — tracking with consent

 　　"P" — tracking only if consented

 　　"D" — disregarding DNT

 　　"U" — updated

> TSV: ?

**Upgrade** 请求客户端升级协议

> Upgrade: HTTP/2.0, HTTPS/1.3, IRC/6.9, RTA/x11, websocket

**Vary**    通知下级代理如何匹配未来的请求头已让其决定缓存的响应是否可用而不是重新从源主机请求新的

> Example 1: Vary: *
>  Example 2: Vary: Accept-Language

**Via** 通知客户端代理，通过其要发送什么响应

> Via: 1.0 fred, 1.1 example.com (Apache/1.1)

**Warning** 实体可能会发生的问题的通用警告

> Warning: 199 Miscellaneous warning

**WWW-Authenticate**    标识访问请求实体的身份验证方案

> WWW-Authenticate: Basic

**X-Frame-Options** 点击劫持保护：

 　　deny frame中不渲染

 　　sameorigin 如果源不匹配不渲染

 　　allow-from 允许指定位置访问

 　　allowall 不标准，允许任意位置访问

> X-Frame-Options: deny

##### 常用非标准响应头字段

------

**X-XSS-Protection**    过滤跨站脚本

> X-XSS-Protection: 1; mode=block

**Content-Security-Policy, X-Content-Security-Policy,X-WebKit-CSP** 定义内容安全策略

> X-WebKit-CSP: default-src 'self'

**X-Content-Type-Options**  唯一的取值是"",阻止IE在响应中嗅探定义的内容格式以外的其他MIME格式

> X-Content-Type-Options: nosniff

**X-Powered-By**    指定支持web应用的技术

> X-Powered-By: PHP/5.4.0

**X-UA-Compatible** 推荐首选的渲染引擎来展示内容，通常向后兼容，也用于激活IE中内嵌chrome框架插件
 `<meta http-equiv="X-UA-Compatible" content="chrome=1" />`

> X-UA-Compatible: IE=EmulateIE7
>  X-UA-Compatible: IE=edge
>  X-UA-Compatible: Chrome=1

**X-Content-Duration**  提供音视频的持续时间，单位是秒，只有Gecko内核浏览器支持

> X-Content-Duration: 42.666

**Upgrade-Insecure-Requests**   标识服务器是否可以处理HTTPS协议

> Upgrade-Insecure-Requests: 1

**X-Request-ID,X-Correlation-ID**   标识一个客户端和服务端的请求

> X-Request-ID: f058ebd6-02f7-4d3f-942e-904344e8cde5


如果想要让客户端可以访问到其他的首部信息，可以将它们在 `Access-Control-Expose-Headers` 里面列出来。

##### Access-Control-Allow-Headers

响应首部Access-Control-Allow-Headers用于{{glossary("preflight request")}}(预检请求)中，列出了将会在正式请求的）{{HTTPHeader("Access-Control-Request-Headers")}}字段中出现的首部信息。

##### Access-Control-Expose-Headers

响应首部Access-Control-Expose-Headers 列出了哪些首部可以作为响应的一部分暴露给外部。

默认情况下，只有七种 [simple response headers](https://developer.mozilla.org/zh-CN/docs/Glossary/Simple_response_header) （简单响应首部）可以暴露给外部：

- [`Cache-Control`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cache-Control)
- [`Content-Language`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Language)
- [`Content-Length`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Length)
- [`Content-Type`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Type)
- [`Expires`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Expires)
- [`Last-Modified`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Last-Modified)
- [`Pragma`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Pragma)

如果想要让客户端可以访问到其他的首部信息，可以将它们在 `Access-Control-Expose-Headers` 里面列出来。

##### Access-Control-Allow-Headers

响应首部Access-Control-Allow-Headers用于{{glossary("preflight request")}}(预检请求)中，列出了将会在正式请求的）{{HTTPHeader("Access-Control-Request-Headers")}}字段中出现的首部信息。

##### http网络请求返回不同的statusCode

###### 201-206都表示服务器成功处理了请求的状态代码，说明网页可以正常访问。

​    200（成功） 服务器已成功处理了请求。通常，这表示服务器提供了请求的网页。

​    201（已创建） 请求成功且服务器已创建了新的资源。

​    202（已接受） 服务器已接受了请求，但尚未对其进行处理。

​    203（非授权信息） 服务器已成功处理了请求，但返回了可能来自另一来源的信息。

​    204（无内容） 服务器成功处理了请求，但未返回任何内容。

​    205（重置内容） 服务器成功处理了请求，但未返回任何内容。与 204 响应不同，此响应要求请求者重置文档视图（例如清除表单内容以输入新内容）。

​    206（部分内容） 服务器成功处理了部分 GET 请求。

###### 300-3007表示的意思是：要完成请求，您需要进一步进行操作。通常，这些状态代码是永远重定向的。

​    300（多种选择） 服务器根据请求可执行多种操作。服务器可根据请求者 来选择一项操作，或提供操作列表供其选择。

​    301（永久移动） 请求的网页已被永久移动到新位置。服务器返回此响应时，会自动将请求者转到新位置。您应使用此代码通知搜索引擎蜘蛛网页或网站已被永久移动到新位置。

​    302（临时移动） 服务器目前正从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。会自动将请求者转到不同的位置。但由于搜索引擎会继续抓取原有位置并将其编入索引，因此您不应使用此代码来告诉搜索引擎页面或网站已被移动。

​    303（查看其他位置） 当请求者应对不同的位置进行单独的 GET 请求以检索响应时，服务器会返回此代码。对于除 HEAD 请求之外的所有请求，服务器会自动转到其他位置。

​    304（未修改） 自从上次请求后，请求的网页未被修改过。服务器返回此响应时，不会返回网页内容。

​    如果网页自请求者上次请求后再也没有更改过，您应当将服务器配置为返回此响应。由于服务器可以告诉 搜索引擎自从上次抓取后网页没有更改过，因此可节省带宽和开销。

​    305（使用代理） 请求者只能使用代理访问请求的网页。如果服务器返回此响应，那么，服务器还会指明请求者应当使用的代理。

​    307（临时重定向） 服务器目前正从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。会自动将请求者转到不同的位置。但由于搜索引擎会继续抓取原有位置并将其编入索引，因此您不应使用此代码来告诉搜索引擎某个页面或网站已被移动。

###### 4XXHTTP状态码表示请求可能出错，会妨碍服务器的处理。

​    400（错误请求） 服务器不理解请求的语法。

​    401（身份验证错误） 此页要求授权。您可能不希望将此网页纳入索引。

​    403（禁止） 服务器拒绝请求。

​    404（未找到） 服务器找不到请求的网页。例如，对于服务器上不存在的网页经常会返回此代码。

​    例如：http://www.0631abc.com/20100aaaa，就会进入404错误页面

​    405（方法禁用） 禁用请求中指定的方法。

​    406（不接受） 无法使用请求的内容特性响应请求的网页。

​    407（需要代理授权） 此状态码与 401 类似，但指定请求者必须授权使用代理。如果服务器返回此响应，还表示请求者应当使用代理。

​    408（请求超时） 服务器等候请求时发生超时。

​    409（冲突） 服务器在完成请求时发生冲突。服务器必须在响应中包含有关冲突的信息。服务器在响应与前一个请求相冲突的 PUT 请求时可能会返回此代码，以及两个请求的差异列表。

​    410（已删除） 请求的资源永久删除后，服务器返回此响应。该代码与 404（未找到）代码相似，但在资源以前存在而现在不存在的情况下，有时会用来替代 404 代码。如果资源已永久删除，您应当使用 301 指定资源的新位置。

​    411（需要有效长度） 服务器不接受不含有效内容长度标头字段的请求。

​    412（未满足前提条件） 服务器未满足请求者在请求中设置的其中一个前提条件。

​    413（请求实体过大） 服务器无法处理请求，因为请求实体过大，超出服务器的处理能力。

​    414（请求的 URI 过长） 请求的 URI（通常为网址）过长，服务器无法处理。

​    415（不支持的媒体类型） 请求的格式不受请求页面的支持。

​    416（请求范围不符合要求） 如果页面无法提供请求的范围，则服务器会返回此状态码。

​    417（未满足期望值） 服务器未满足"期望"请求标头字段的要求。

###### 500至505表示的意思是：服务器在尝试处理请求时发生内部错误。这些错误可能是服务器本身的错误，而不是请求出错。

​       500（服务器内部错误） 服务器遇到错误，无法完成请求。

​        501（尚未实施） 服务器不具备完成请求的功能。例如，当服务器无法识别请求方法时，服务器可能会返回此代码。

​        502（错误网关） 服务器作为网关或代理，从上游服务器收到了无效的响应。

​        503（服务不可用） 目前无法使用服务器（由于超载或进行停机维护）。通常，这只是一种暂时的状态。

​        504（网关超时） 服务器作为网关或代理，未及时从上游服务器接收请求。

​        505（HTTP 版本不受支持） 服务器不支持请求中所使用的 HTTP 协议版本。

HTTP的状态码有很多种，主要有1xx(临时响应)、2xx(成功)、3xx(已重定向)、4xx(请求错误)以及5xx(服务器错误)五个大类，每个大类还对应一些具体的分类。平时我们接触比较多的是200、400、500等。

204 No Content

意思等同于请求执行成功，但是没有数据，浏览器不用刷新页面，也不用导向新的页面。

304 Not Modified

304状态码或许不应该认为是一种错误，而是对客户端有缓存情况下服务端的一种响应。

整个请求响应过程如下：

客户端在请求一个文件的时候，发现自己缓存的文件有Last Modified，那么在请求中会包含 If Modified Since，这个时间就是缓存文件的Last Modified。因此，如果请求中包含If Modified Since，就说明已经有缓存在客户端。服务端只要判断这个时间和当前请求的文件 的修改时间留可以确定是返回304还是200。

对于静态文件，例如：CSS、图片、服务器会自动完成Last Modified和If Modified Since的比较，完成缓存或者更新。但是对于动态页面，就是动态产生的页面，往往没有包含Last Modified信息，这样浏览器、网关等都不会做缓存，也就是在每次请求的时候都完成一个200的请求。

因此，对于动态页面做缓存加速，首先要在Response的HTTP Header中增加Last Modified 定义，其次根据Request 中的If Modified Since 和被请求内容的更新时间来返回200或304。虽然在返回304的时候已经做了一次数据库查询，但是可以避免接下来更多的数据库查询，并且没有返回页面内容而只是一个HTTP Header，从而大大的降低带宽的消耗，对于用户的体验也是提高。

##### 常考面试题

1.Post和Get的区别？

```
Post 和 Get 是 HTTP 请求的两种方法。

（1）从应用场景上来说，GET 请求是一个幂等的请求，一般 Get 请求用于对服务器资源不会产生影响的场景，比如说请求一个网页。而 Post 不是一个幂等的请求，一般用于对服务器资源会产生影响的情景。比如注册用户这一类的操作。

（2）因为不同的应用场景，所以浏览器一般会对 Get 请求缓存，但很少对 Post 请求缓存。

（3）从发送的报文格式来说，Get 请求的报文中实体部分为空，Post 请求的报文中实体部分一般为向服务器发送的数据。

（4）但是 Get 请求也可以将请求的参数放入 url 中向服务器发送，这样的做法相对于 Post 请求来说，一个方面是不太安全，因为请求的 url 会被保留在历史记录中。并且浏览器由于对 url 有一个长度上的限制，所以会影响 get 请求发送数据时的长度。这个限制是浏览器规定的，并不是 RFC 规定的。还有就是 post 的参数传递支持更多的数据类型。
```

2.TLS/SSL中什么一定要用三个随机数，来生成“回话密钥”？

```
客户端和服务器都需要生成随机数，以此来保证每次生成的秘钥都不相同。使用三个随机数，是因为 SSL 的协议默认不信任每个主机都能产生完全随机的数，如果只使用一个伪随机的数来生成秘钥，就很容易被破解。通过使用三个随机数的方式，增加了自由度，一个伪随机可能被破解，但是三个伪随机就很接近于随机了，因此可以使用这种方法来保持生成秘钥的随机性和安全性。
```

3.SSL 连接断开后如何恢复？

```
一共有两种方法来恢复断开的 SSL 连接，一种是使用 session ID，一种是 session ticket。

使用 session ID 的方式，每一次的会话都有一个编号，当对话中断后，下一次重新连接时，只要客户端给出这个编号，服务器如果有这个编号的记录，那么双方就可以继续使用以前的秘钥，而不用重新生成一把。目前所有的浏览器都支持这一种方法。但是这种方法有一个缺点是，session ID 只能够存在一台服务器上，如果我们的请求通过负载平衡被转移到了其他的服务器上，那么就无法恢复对话。

另一种方式是 session ticket 的方式，session ticket 是服务器在上一次对话中发送给客户的，这个 ticket 是加密的，只有服务器能够解密，里面包含了本次会话的信息，比如对话秘钥和加密方法等。这样不管我们的请求是否转移到其他的服务器上，当服务器将 ticket 解密以后，就能够获取上次对话的信息，就不用重新生成对话秘钥了。
```

#### 4. RSA 算法的安全性保障？

```
对极大整数做因数分解的难度决定了 RSA 算法的可靠性。换言之，对一极大整数做因数分解愈困难，RSA 算法愈可靠。现在1024位的 RSA 密钥基本安全，2048位的密钥极其安全。
```

#### 5. DNS 为什么使用 UDP 协议作为传输层协议？

```
DNS 使用 UDP 协议作为传输层协议的主要原因是为了避免使用 TCP 协议时造成的连接时延。因为为了得到一个域名的 IP 地址，往往会向多个域名服务器查询，如果使用 TCP 协议，那么每次请求都会存在连接时延，这样使 DNS 服务变得很慢，因为大多数的地址查询请求，都是浏览器请求页面时发出的，这样会造成网页的等待时间过长。

使用 UDP 协议作为 DNS 协议会有一个问题，由于历史原因，物理链路的最小MTU = 576，所以为了限制报文长度不超过576，UDP 的报文段的长度被限制在 512 个字节以内，这样一旦 DNS 的查询或者应答报文，超过了 512 字节，那么基于 UDP 的DNS 协议就会被截断为 512 字节，那么有可能用户得到的 DNS 应答就是不完整的。这里 DNS 报文的长度一旦超过限制，并不会像 TCP 协议那样被拆分成多个报文段传输，因为 UDP 协议不会维护连接状态，所以我们没有办法确定那几个报文段属于同一个数据，UDP 只会将多余的数据给截取掉。为了解决这个问题，我们可以使用 TCP 协议去请求报文。

DNS 还存在的一个问题是安全问题，就是我们没有办法确定我们得到的应答，一定是一个安全的应答，因为应答可以被他人伪造，所以现在有了 DNS over HTTPS 来解决这个问题。
```

#### 6. 当你在浏览器中输入 Google.com 并且按下回车之后发生了什么？

```
（1）首先会对 URL 进行解析，分析所需要使用的传输协议和请求的资源的路径。如果输入的 URL 中的协议或者主机名不合法，将会把地址栏中输入的内容传递给搜索引擎。如果没有问题，浏览器会检查 URL 中是否出现了非法字符，如果存在非法字符，则对非法字符进行转义后再进行下一过程。

（2）浏览器会判断所请求的资源是否在缓存里，如果请求的资源在缓存里并且没有失效，那么就直接使用，否则向服务器发起新的请求。

（3）下一步我们首先需要获取的是输入的 URL 中的域名的 IP 地址，首先会判断本地是否有该域名的 IP 地址的缓存，如果有则使用，如果没有则向本地 DNS 服务器发起请求。本地 DNS 服务器也会先检查是否存在缓存，如果没有就会先向根域名服务器发起请求，获得负责的顶级域名服务器的地址后，再向顶级域名服务器请求，然后获得负责的权威域名服务器的地址后，再向权威域名服务器发起请求，最终获得域名的 IP 地址后，本地 DNS 服务器再将这个 IP 地址返回给请求的用户。用户向本地 DNS 服务器发起请求属于递归请求，本地 DNS 服务器向各级域名服务器发起请求属于迭代请求。

（4）当浏览器得到 IP 地址后，数据传输还需要知道目的主机 MAC 地址，因为应用层下发数据给传输层，TCP 协议会指定源端口号和目的端口号，然后下发给网络层。网络层会将本机地址作为源地址，获取的 IP 地址作为目的地址。然后将下发给数据链路层，数据链路层的发送需要加入通信双方的 MAC 地址，我们本机的 MAC 地址作为源 MAC 地址，目的 MAC 地址需要分情况处理，通过将 IP 地址与我们本机的子网掩码相与，我们可以判断我们是否与请求主机在同一个子网里，如果在同一个子网里，我们可以使用 APR 协议获取到目的主机的 MAC 地址，如果我们不在一个子网里，那么我们的请求应该转发给我们的网关，由它代为转发，此时同样可以通过 ARP 协议来获取网关的 MAC 地址，此时目的主机的 MAC 地址应该为网关的地址。

（5）下面是 TCP 建立连接的三次握手的过程，首先客户端向服务器发送一个 SYN 连接请求报文段和一个随机序号，服务端接收到请求后向服务器端发送一个 SYN ACK报文段，确认连接请求，并且也向客户端发送一个随机序号。客户端接收服务器的确认应答后，进入连接建立的状态，同时向服务器也发送一个 ACK 确认报文段，服务器端接收到确认后，也进入连接建立状态，此时双方的连接就建立起来了。

（6）如果使用的是 HTTPS 协议，在通信前还存在 TLS 的一个四次握手的过程。首先由客户端向服务器端发送使用的协议的版本号、一个随机数和可以使用的加密方法。服务器端收到后，确认加密的方法，也向客户端发送一个随机数和自己的数字证书。客户端收到后，首先检查数字证书是否有效，如果有效，则再生成一个随机数，并使用证书中的公钥对随机数加密，然后发送给服务器端，并且还会提供一个前面所有内容的 hash 值供服务器端检验。服务器端接收后，使用自己的私钥对数据解密，同时向客户端发送一个前面所有内容的 hash 值供客户端检验。这个时候双方都有了三个随机数，按照之前所约定的加密方法，使用这三个随机数生成一把秘钥，以后双方通信前，就使用这个秘钥对数据进行加密后再传输。

（7）当页面请求发送到服务器端后，服务器端会返回一个 html 文件作为响应，浏览器接收到响应后，开始对 html 文件进行解析，开始页面的渲染过程。

（8）浏览器首先会根据 html 文件构建 DOM 树，根据解析到的 css 文件构建 CSSOM 树，如果遇到 script 标签，则判端是否含有 defer 或者 async 属性，要不然 script 的加载和执行会造成页面的渲染的阻塞。当 DOM 树和 CSSOM 树建立好后，根据它们来构建渲染树。渲染树构建好后，会根据渲染树来进行布局。布局完成后，最后使用浏览器的 UI 接口对页面进行绘制。这个时候整个页面就显示出来了。

（9）最后一步是 TCP 断开连接的四次挥手过程。
```

#### 7. 谈谈 CDN 服务？

```
CDN 是一个内容分发网络，通过对源网站资源的缓存，利用本身多台位于不同地域、不同运营商的服务器，向用户提供资源就近访问的功能。也就是说，用户的请求并不是直接发送给源网站，而是发送给 CDN 服务器，由 CDN 服务器将请求定位到最近的含有该资源的服务器上去请求。这样有利于提高网站的访问速度，同时通过这种方式也减轻了源服务器的访问压力。
```

#### 8. 什么是正向代理和反向代理？

```
我们常说的代理也就是指正向代理，正向代理的过程，它隐藏了真实的请求客户端，服务端不知道真实的客户端是谁，客户端请求的服务都被代理服务器代替来请求。

反向代理隐藏了真实的服务端，当我们请求一个网站的时候，背后可能有成千上万台服务器为我们服务，但具体是哪一台，我们不知道，也不需要知道，我们只需要知道反向代理服务器是谁就好了，反向代理服务器会帮我们把请求转发到真实的服务器那里去。反向代理器一般用来实现负载平衡。
```

#### 9. 负载平衡的两种实现方式？

```
一种是使用反向代理的方式，用户的请求都发送到反向代理服务上，然后由反向代理服务器来转发请求到真实的服务器上，以此来实现集群的负载平衡。

另一种是 DNS 的方式，DNS 可以用于在冗余的服务器上实现负载平衡。因为现在一般的大型网站使用多台服务器提供服务，因此一个域名可能会对应多个服务器地址。当用户向网站域名请求的时候，DNS 服务器返回这个域名所对应的服务器 IP 地址的集合，但在每个回答中，会循环这些 IP 地址的顺序，用户一般会选择排在前面的地址发送请求。以此将用户的请求均衡的分配到各个不同的服务器上，这样来实现负载均衡。这种方式有一个缺点就是，由于 DNS 服务器中存在缓存，所以有可能一个服务器出现故障后，域名解析仍然返回的是那个 IP 地址，就会造成访问的问题。
```

#### 10. http 请求方法 options 方法有什么用？

```
 OPTIONS 请求与 HEAD 类似，一般也是用于客户端查看服务器的性能。这个方法会请求服务器返回该资源所支持的所有 HTTP 请求方法，该方法会用'*'来代替资源名称，向服务器发送 OPTIONS 请求，可以测试服务器功能是否正常。JS 的 XMLHttpRequest对象进行 CORS 跨域资源共享时，对于复杂请求，就是使用 OPTIONS 方法发送嗅探请求，以判断是否有对指定资源的访问权限。
```

#### 11. http1.1 和 http1.0 之间有哪些区别？

```
http1.1 相对于 http1.0 有这样几个区别：
（1）连接方面的区别，http1.1 默认使用持久连接，而 http1.0 默认使用非持久连接。http1.1 通过使用持久连接来使多个 http 请求复用同一个 TCP 连接，以此来避免使用非持久连接时每次需要建立连接的时延。
（2）资源请求方面的区别，在 http1.0 中，存在一些浪费带宽的现象，例如客户端只是需要某个对象的一部分，而服务器却将整个对象送过来了，并且不支持断点续传功能，http1.1 则在请求头引入了 range 头域，它允许只请求资源的某个部分，即返回码是 206（Partial Content），这样就方便了开发者自由的选择以便于充分利用带宽和连接。

（3）缓存方面的区别，在 http1.0 中主要使用 header 里的 If-Modified-Since,Expires 来做为缓存判断的标准，http1.1 则引入了更多的缓存控制策略例如 Etag、If-Unmodified-Since、If-Match、If-None-Match 等更多可供选择的缓存头来控制缓存策略。

（4）http1.1 中还新增了 host 字段，用来指定服务器的域名。http1.0 中认为每台服务器都绑定一个唯一的 IP 地址，因此，请求消息中的 URL 并没有传递主机名（hostname）。但随着虚拟主机技术的发展，在一台物理服务器上可以存在多个虚拟主机，并且它们共享一个IP地址。因此有了 host 字段，就可以将请求发往同一台服务器上的不同网站。

（5）http1.1 相对于 http1.0 还新增了很多方法，如 PUT、HEAD、OPTIONS 等。
```

#### 13. 即时通讯的实现，短轮询、长轮询、SSE 和 WebSocket 间的区别？

```
短轮询和长轮询的目的都是用于实现客户端和服务器端的一个即时通讯。

短轮询的基本思路就是浏览器每隔一段时间向浏览器发送 http 请求，服务器端在收到请求后，不论是否有数据更新，都直接进行响应。这种方式实现的即时通信，本质上还是浏览器发送请求，服务器接受请求的一个过程，通过让客户端不断的进行请求，使得客户端能够模拟实时地收到服务器端的数据的变化。这种方式的优点是比较简单，易于理解。缺点是这种方式由于需要不断的建立 http 连接，严重浪费了服务器端和客户端的资源。当用户增加时，服务器端的压力就会变大，这是很不合理的。

长轮询的基本思路是，首先由客户端向服务器发起请求，当服务器收到客户端发来的请求后，服务器端不会直接进行响应，而是先将这个请求挂起，然后判断服务器端数据是否有更新。如果有更新，则进行响应，如果一直没有数据，则到达一定的时间限制才返回。客户端 JavaScript 响应处理函数会在处理完服务器返回的信息后，再次发出请求，重新建立连接。长轮询和短轮询比起来，它的优点是明显减少了很多不必要的 http 请求次数，相比之下节约了资源。长轮询的缺点在于，连接挂起也会导致资源的浪费。

SSE(Sever-Sent Event服务器推送事件) 的基本思想是，服务器使用流信息向服务器推送信息。严格地说，http 协议无法做到服务器主动推送信息。但是，有一种变通方法，就是服务器向客户端声明，接下来要发送的是流信息。也就是说，发送的不是一次性的数据包，而是一个数据流，会连续不断地发送过来。这时，客户端不会关闭连接，会一直等着服务器发过来的新的数据流，视频播放就是这样的例子。SSE 就是利用这种机制，使用流信息向浏览器推送信息。它基于 http 协议，目前除了 IE/Edge，其他浏览器都支持。它相对于前面两种方式来说，不需要建立过多的 http 请求，相比之下节约了资源。

上面三种方式本质上都是基于 http 协议的，我们还可以使用 WebSocket 协议来实现。WebSocket 是 Html5 定义的一个新协议，与传统的 http 协议不同，该协议允许由服务器主动的向客户端推送信息。使用 WebSocket 协议的缺点是在服务器端的配置比较复杂。WebSocket 是一个全双工的协议，也就是通信双方是平等的，可以相互发送消息，而 SSE 的方式是单向通信的，只能由服务器端向客户端推送信息，如果客户端需要发送信息就是属于下一个 http 请求了。
```

#### 14. 怎么实现多个网站之间共享登录状态
```
在多个网站之间共享登录状态指的就是单点登录。多个应用系统中，用户只需要登录一次就可以访问所有相互信任的应用系统。

我认为单点登录可以这样来实现，首先将用户信息的验证中心独立出来，作为一个单独的认证中心，该认证中心的作用是判断客户端发送的账号密码的正确性，然后向客户端返回对应的用户信息，并且返回一个由服务器端秘钥加密的登录信息的 token 给客户端，该token 具有一定的有效时限。当一个应用系统跳转到另一个应用系统时，通过 url 参数的方式来传递 token，然后转移到的应用站点发送给认证中心，认证中心对 token 进行解密后验证，如果用户信息没有失效，则向客户端返回对应的用户信息，如果失效了则将页面重定向会单点登录页面。
```
