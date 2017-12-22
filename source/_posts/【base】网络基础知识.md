---
layout: _posts
title: 【base】网络基础知识
date: 2017-11-30 10:54:33
tags: [网络,http,基础]
description: 网络：http请求头，响应头，状态码，http过程，跨域，网络安全等
---

## http请求
一篇写的比较完善的文章：
[http请求](https://www.cnblogs.com/yin-jingyu/archive/2011/08/01/2123548.html)

### http请求格式
HTTP请求格式主要有四部分组成，分别是：请求行、请求头、空行、消息体，每部分内容占一行；
```
<request-line>		//请求行
<general-headers>   //请求头
<request-headers>	//请求头
<entity-headers>	//请求头
<empty-line>		//空行
[<message-body>]	//消息体
```

#### 请求行
请求行是请求消息的第一行，由三部分组成：
1、请求方法（GET/POST/DELETE/PUT/HEAD）
2、请求资源的URI路径
3、HTTP的版本号

如：
```
GET /index.html HTTP/1.1
```

#### 请求头
请求头中的信息有和缓存相关的头（Cache-Control，cookie）、客户端身份信息（User-Agent）等等
如：
```
cache-control:no-cache
user-agent:Mozilla/5.0 (Linux; Android 6.0; Nexus 5 Build/MRA58N) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.87 Mobile Safari/537.36
等...
```

#### 消息体(Query String Parameters)
请求体是客户端发给服务端的请求数据（非必须）；
如：
```
pagenum: 1
```

### 响应格式
服务器接收处理完请求后返回一个HTTP相应消息给客户端。
HTTP响应消息的格式包括：状态行、响应头、空行、消息体。
```
<status-line>
<general-headers>
<response-headers>
<entity-headers>
<empty-line>
[<message-body>]
```

#### 状态行
状态行位于相应消息的第一行，有HTTP协议版本号，状态码和状态说明三部分构成。
如：
```
HTTP/1.1 200 OK
```

#### 响应头
响应头是服务器传递给客户端用于说明服务器的一些信息，以及将来继续访问该资源时的策略。
如：
```
cache-control:private
cache-control:no-cache
content-encoding:gzip
content-type:text/html;charset=UTF-8
date:Sat, 16 Dec 2017 07:51:48 GMT
expires:0
server:nginx
set-cookie:f=n
status:200
vary:Accept-Encoding
Connection:keep-alive
等
```

#### 响应体
响应体是服务端返回给客户端的HTML文本内容，或者其他格式的数据，比如：视频流、图片或者音频数据。


## 状态码

#### 1xx - 信息提示
这些状态代码表示`临时的响应`。客户端在收到常规响应之前，应准备接收一个或多个 1xx 响应。

#### 2xx - 成功
这类状态代码表明服务器成功地接受了客户端请求。
200 - OK 一切正常，对GET和POST请求的应答文档跟在后面。
201 - Created 服务器已经创建了文档，Location头给出了它的URL。
202 - Accepted 已经接受请求，但处理尚未完成。
203 - Non-Authoritative Information 文档已经正常地返回，但一些应答头可能不正确，因为使用的是文档的拷贝，非权威性信息（HTTP 1.1新）。
204 - No Content 没有新文档，浏览器应该继续显示原来的文档。如果用户定期地刷新页面，而Servlet可以确定用户文档足够新，这个状态代码是很有用的。
205 - Reset Content 没有新的内容，但浏览器应该重置它所显示的内容。用来强制浏览器清除表单输入内容（HTTP 1.1新）。
206 - Partial Content 客户发送了一个带有Range头的GET请求，服务器完成了它（HTTP 1.1新）。

#### 3xx - 重定向

#### 4xx - 客户端错误
400 - Bad Request 请求出现语法错误。
401 - Unauthorized 访问被拒绝
403 - Forbidden 资源不可用
404 - Not Found 无法找到指定位置的资源
405 - Method Not Allowed 请求方法（GET、POST、HEAD、Delete、PUT、TRACE等）对指定的资源`不适用`，用来访问本页面的 HTTP 谓词不被允许（方法不被允许）
406 - Not Acceptable 指定的资源已经找到，但它的MIME类型和客户在Accpet头中所指定的不兼容，客户端浏览器不接受所请求页面的 MIME 类型
407 - Proxy Authentication Required 要求进行代理身份验证
408 - Request Timeout 在服务器许可的等待时间内，客户一直没有发出任何请求。客户可以在以后重复同一请求

`MIME类型`是一种通知客户端其接收文件的多样性的机制:文件后缀名在网页上并没有明确的意义
如：
```
text/plain
text/html
image/jpeg
image/png
audio/mpeg
audio/ogg
audio/*
video/mp4
application/octet-stream
application/json
```

#### 5xx - 服务器错误
500 - Internal Server Error 服务器遇到了意料不到的情况，不能完成客户的请求。
501 - Not Implemented 服务器不支持实现请求所需要的功能，页眉值指定了未实现的配置。
502 - Bad Gateway 服务器作为网关或者代理时，为了完成请求访问下一个服务器，但该服务器返回了非法的应答。 亦说Web 服务器用作网关或代理服务器时收到了无效响应。
503 - Service Unavailable 服务不可用，服务器由于维护或者负载过重未能应答。
504 - Gateway Timeout 网关超时
505 - HTTP Version Not Supported 服务器不支持请求中所指明的HTTP版本


## http请求过程

#### 1、建立TCP连接
在HTTP工作开始之前，Web浏览器首先要通过网络与Web服务器建立连接，该连接是通过TCP来完成的，该协议与IP协议共同构建Internet，即著名的TCP/IP协议族，因此Internet又被称作是TCP/IP网络。HTTP是比TCP更高层次的应用层协议，根据规则，只有低层协议建立之后才能，才能进行更层协议的连接。

#### 2、Web浏览器向Web服务器发送请求命令
一旦建立了TCP连接，Web浏览器就会向Web服务器发送请求命令

例如：GET/sample/hello.jsp HTTP/1.1
#### 3、Web浏览器发送请求头信息
浏览器发送其请求命令之后，还要以头信息的形式向Web服务器发送一些别的信息，之后浏览器发送了一**空白行**来通知服务器，它已经结束了该头信息的发送。

#### 4、Web服务器应答
客户机向服务器发出请求后，服务器会客户机回送应答，
HTTP/1.1 200 OK
应答的第一部分是协议的版本号和应答状态码

#### 5、Web服务器发送应答头信息
正如客户端会随同请求发送关于自身的信息一样，服务器也会随同应答向用户发送关于它自己的数据及被请求的文档。

#### 6、Web服务器向浏览器发送数据
Web服务器向浏览器发送头信息后，它会发送一个空白行来表示头信息的发送到此为结束，接着，它就以Content-Type应答头信息所描述的格式发送用户所请求的实际数据

#### 7、Web服务器关闭TCP连接
一般情况下，一旦Web服务器向浏览器发送了请求数据，它就要关闭TCP连接，然后如果浏览器或者服务器在其头信息加入了这行代码
**Connection:keep-alive**
TCP连接在发送后将仍然保持打开状态，于是，浏览器可以继续通过相同的连接发送请求。保持连接节省了为每个请求建立新连接所需的时间，还节约了网络带宽。

## 跨域

关于跨域问题，一直遇到的都是get跨域，在车友圈项目的开发中才遇到了post跨域的问题，虽然最后把页面放到了后端服务器上，不跨域，避免了这个问题，但是过程还是很有意义。

跨域方法汇总：
+ 1、避免跨域，将页面和接口部署在同一个站点下面

+ 2、ngix反向代理
直接使用Web服务器apache，nginx等的反向代理的方式，将需要跨域的请求发送到当前域，在Web容器配置中做请求转发，像nginx这样的反向代理很擅长做这样的事情。

+ 3、JSONP（get）

+ 4、form表单
+ 5、设置document.domain
+ 6、设置window.name
+ 7、CORS
+ 8、fetch
+ 9、WebSocket
+ 10、postMessage

get跨域（jsonp）
post跨域（CORS）
利用表单跨域（不好处理返回结果）



## 网络安全