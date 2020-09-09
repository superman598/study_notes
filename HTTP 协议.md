# HTTP 协议

[TOC]

## 介绍

HTTP（hypertext transport protocol）协议也叫==超文本传输协议==，是一种基于 TCP/IP 的应用层通信协议，这个协议详细规定了浏览器和万维网服务器之间互相通信的规则。

协议主要规定了两方面的内容

* 客户端向服务器发送数据，称之为==请求报文==
* 服务器向客户端返回数据，称之为==响应报文==

## Fiddle 工具

Fiddler 是一个http协议调试代理工具，使用它我们可以抓取网页的所有请求与响应，也就是咱们俗称的抓包。

![1573826028707](D:%5Cproject%5Cgit%5Ch5200622-code%5Cday10%5C%E8%AF%BE%E5%A0%82%5C2-HTTP%5C%E8%AF%BE%E4%BB%B6%5Cassets%5C1573826028707.png)

## 内容

### 请求

HTTP 请求报文包括四部分

* 请求行
* 请求头

* 空行
* 请求体(请求体的格式是非常灵活的)

```http
// 请求行
GET http://localhost:3000/index.html?username=sunwukong&password=123123 HTTP/1.1
// 请求头
// 主机名
Host: localhost:3000
// 连接设置 keep-alive/close
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
Upgrade-Insecure-Requests: 1
// 客户端的字符串标志（用户代理）
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/64.0.3282.140 Safari/537.36
// 接受文本形式
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
// 接受的压缩方式
Accept-Encoding: gzip, deflate, br
// 接受的语言
Accept-Language: zh-CN,zh;q=0.9
// 在哪个页面发起的，请求的来源
Referer：// http://localhost:3000/hello.html
// 特殊的请求头
Cookie:
```

*  GET http://localhost:3000/hello.html HTTP/1.1：GET请求，请求服务器路径为http://localhost:3000/hello.html，?后面跟着的是请求参数（查询字符串），协议是HTTP 1.1版本，#后面跟着的是锚点。

* Host: localhost:3000：请求的主机名为localhost，端口号3000

* Connection: keep-alive：处理完这次请求后继续保持连接，默认为3000ms

* Pragma: no-cache：不缓存该资源，http 1.0的规定

* Cache-Control: no-cache： 不缓存该资源 http 1.1的规定，优先级更高

* Upgrade-Insecure-Requests: 1：告诉服务器，支持发请求的时候不用 http 而用 https

* User-Agent: Mozilla/5.0 (...：与浏览器和OS相关的信息。有些网站会显示用户的系统版本和浏览器版本信息，这都是通过获取User-Agent头信息而来的

* Accept: text/html,...：告诉服务器，当前客户端可以接收的文档类型。q 相当于描述了客户端对于某种媒体类型的喜好系数，该值的范围是 0-1。默认为1

*  Accept-Encoding: gzip, deflate, br：支持的压缩格式。数据在网络上传递时，服务器会把数据压缩后再发送

* Accept-Language: zh-CN,zh;q=0.9：当前客户端支持的语言，可以在浏览器的工具选项中找到语言相关信息

### 响应

HTTP 响应报文也包括四个部分

- 响应行
- 响应头
- 空行
- 响应体

```http
HTTP/1.1 200 OK （响应行）
// 响应头
X-Powered-By: Express
Accept-Ranges: bytes
Cache-Control: public, max-age=0
Last-Modified: Wed, 21 Mar 2018 13:13:13 GMT
ETag: W/"a9-16248b12b64"
Content-Type: text/html; charset=UTF-8
Content-Length: 169
Date: Thu, 22 Mar 2018 12:58:41 GMT
Connection: keep-alive

// 响应体（内容格式非常灵活）
// html/css/javascript/图片/字体/JSON格式的字符串
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>首页</title>
</head>
<body>
  <h1>网站首页</h1>
</body>
</html>
```

* HTTP/1.1  HTTP协议版本

* （响应状态码）200 响应成功，404 找不到页面，403禁止访问，500服务器内部错误

  ​	1 临时响应 	2成功响应	3跳转响应	4客户端错误	5服务端错误

* OK 响应的状态字符串，与响应状态码一一对应

* X-Powered-By: Express：自定义的头，表示用的框架，一般不返回容易造成安全漏洞。

* Accept-Ranges: bytes：告诉浏览器支持多线程下载

* Cache-Control: public, max-age=0：强制对所有静态资产进行缓存，即使它通常不可缓存。

  控制缓存，private私有，只允许客户端缓存数据，中间代理不允许缓存。

  max-age指定多久缓存一次

* Last-Modified: Wed, 21 Mar 2018 13:13:13 GMT：这个资源最后一次被修改的日期和时间

* ETag: W/"a9-16248b12b64"：请求资源的标记/ID

* Content-Type: text/html; charset=UTF-8：返回响应体资源类型（内容类型）

* Content-Length: 169：响应体的长度

* Date: Thu, 22 Mar 2018 12:58:41 GMT：提供了日期的时间标志，标明响应报文是什么时间创建的（响应时间）

* Expires： 过期时间

* Server：BWS/1.1 (服务器信息)

* Set-Cookie: （设置cookie）

* Strict-Transport-Security: HTTPS强制转换

* Traceid: 跟踪id

* X-Ua-Compatible: IE = Edge,chrome=1  使用当前主机最新的IE 解析器解析页面

* Content-length：响应体的长度（单位：字节）

#### 创建服务

```js
// 1. 引入http模块
const http = require("http");

//定义一个端口变量
let port = 8000;

// 4. 引入url模块
const url = require("url");

// 2. 调用方法创建服务 request请求报文的封装，response是对响应报文的封装
const server = http.createServer(function(request,response){
    // 获取url的路径部分,这个url输出出来如下图所示；
    // url.parse()方法将url解析成对象形式，容易访问
    let url = url.parse(request.url);
    let query = url.parse(request.url,true).query
    
    // 获取url中的pathname和查询字符串
    console.log(url.pathname); // /s
    console.log(query); // wd=computer
    // 访问查询字符串的具体内容
    console.log(query.wd);
    console.log(query.price);
    
    // 设置响应
    response.end("Hello HTTP server")
})
// 3. 监听端口，启动服务（端口，就是计算机的服务窗口。计算机总共有65536个窗口，建议使用>1024的窗口）
server.listen(port,function(){
    console.log(`服务器启动，${port}端口监听中`);
});
```

![image-20200907174139830](C:%5CUsers%5CAdministrator%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200907174139830.png)



#### request-body

```js
const http = require("http");
const qs = require("querystring");

const server = http.createServer(function(request,response){
    //1. 声明变量
    let body = "";
    //2. 绑定事件
    request.on("data",chunk=>{
        body += chunk;
    });
    request.on("end",()=>{
        const data = qs.parse(body);
        console.log(data);
        
        // 设置响应状态码
        response.statusCode = 404;
        // 设置响应状态字符串
        response.statusMessage = "zhaobudao";
        
        // 设置响应头信息
        response.setHeader = ("class","H5200622");
        response.setHeader = ("Content-type","text/html;charset=utf-8");
        // 如果用于结束提示的内容较多时，使用write方法，但是最后必须用end结束；如果内容较少，直接用end方法即可；
        response.write("xxxxxxxxxx")；
        response.end();  
    }) 
});

server.listen(8000,()=>{
    console.log("服务已经启动，8000端口正在监听....");
})
```

#### POST 和 GET的区别

GET 和 POST 都是HTTP 的请求方法

1. GET 主要用于获取数据， POST 主要用于提交数据
2. GET 传递数据有大小限制，一般是4k，而POST没有限制
3. 安全性：GET的参数是暴露在url中，所以相对来说不是特别安全；而POST是将数据放置在请求体中，相对来说较为安全
4. 参数传递的位置，GET请求参数放置在url中，而POST的请求参数是在请求体中；
5. GET请求报文请求方法的内容为GET,POST请求报文请求方法的内容为POST;

#### GET和POST的发送场景

##### GET

1. 浏览器地址栏输入url访问网页
2. 点击a链接页面跳转
3. link href属性
4. script src属性
5. img src属性
6. form表单， method =“GET” 表单提交时
7. AJAX GET 类型请求

##### POST

1. form表单，method=”POST" 表单提交时
2. AJAX  POST类型请求

#### 使用chrome网络控制台查看请求和响应内容（==重点==）

![网络控制台查看报文的方式](D:%5Cproject%5Cgit%5Ch5200622-code%5Cday11%5C%E7%AC%94%E8%AE%B0%5C%E7%BD%91%E7%BB%9C%E6%8E%A7%E5%88%B6%E5%8F%B0%E6%9F%A5%E7%9C%8B%E6%8A%A5%E6%96%87%E7%9A%84%E6%96%B9%E5%BC%8F.png)



### WEB 服务

使用 nodejs 创建 HTTP 服务器

```js
//1. 引入 http 内置模块
var http = require('http');

//2. 创建服务
var server = http.createServer(function(request, response){
    response.end('hello world!!');
});

//3. 监听端口
server.listen(8000);
```

* request 是对请求报文的封装对象
* response 是对响应的封装对象

#### 获取请求

```js
//获取请求方法
console.log(request.method);

//获取http版本
console.log(request.httpVersion);

//获取请求路径
console.log(request.url);

//获取请求头
console.log(request.headers);

//获取请求体
request.on('data', function(chunk){})
request.on('end', function(){});
```

#### 设置响应

```js
//设置状态码
response.statusCode = 200;

//设置响应头
response.setHeader('content-type','text/html;charset-utf-8');

//设置响应体
response.write('body');

//结束
response.end();

```



## 附录

### 响应状态码

响应状态码是服务器对结果的标识，常见的状态码有以下几种：

- 200：请求成功，浏览器会把响应体内容（通常是html）显示在浏览器中；
- 301：重定向，被请求的旧资源永久移除了（不可以访问了），将会跳转到一个新资源，搜索引擎在抓取新内容的同时也将旧的网址替换为重定向之后的网址；
- 302：重定向，被请求的旧资源还在（仍然可以访问），但会临时跳转到一个新资源，搜索引擎会抓取新的内容而保存旧的网址；
- 304：（Not Modified）请求资源未被修改，浏览器将会读取缓存；
- 403：forbidden 禁止的
- 404：请求的资源没有找到，说明客户端错误的请求了不存在的资源；
- 500：请求资源找到了，但服务器内部出现了错误；

  

### Sec-Fetch-* 请求头

<https://www.w3.org/TR/fetch-metadata/#sec-fetch-mode-header>