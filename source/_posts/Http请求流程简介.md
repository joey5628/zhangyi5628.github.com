---
title: HTTP请求流程简介
date: 2016-05-13 14:37:09
categories: Web
tags: [HTTP,TCP]
---
最近看了“[一次完整的HTTP事务是怎样一个过程？](http://www.linux178.com/web/httprequest.html)”这篇博客深有感触，就想做个总结，把对于前端来说更重要的部分做个摘录。  
HTTP属于TCP/IP模型中的应用层协议, TCP协议实现了数据流的传输。然而，人们更加习惯以文件为单位传输资源，比如文本文件，图像文件，超文本文档(hypertext document)。
超文本文档中包含有超链接，指向其他的资源。超文本文档是万维网(World Wide Web，即www)的基础。
HTTP协议解决文件传输的问题。HTTP是应用层协议，主要建立在TCP协议之上(偶尔也可以UDP为底层)。

### 一次完整的HTTP请求的流程大体是这样：
域名解析 ---> 发起TCP的3次握手 ---> 建立TCP连接后发起HTTP请求 ---> 服务器响应HTTP请求 --->浏览器接收到Html代码 ---> 浏览器解析Html代码，并请求html代码中的资源（js、css、image等）--->浏览器对页面进行渲染呈现给用户

### HTTP请求类型
#### 1、GET：获取一个文档
大部分被传输到浏览器的html,js,css,image等资源都是通过GET方式发送出去的，它是获取资源的主要方法。
#### 2、POST：发送数据至服务器
受url长度、安全型等限制大多数获取数据的请求都不使用GET方法，而是使用POST。使用POST可以发送更多的数据，也更安全，URL不再被改写。POST发送的数据位于http请求的主体中。一般使用POST来提交表单。
#### 3、HEAD：接收头部信息
HEAD和GET很相似，只不过HEAD不接受HTTP响应的内容部分。当你发送了一个HEAD请求，那就意味着你只对HTTP头部感兴趣，而不是文档本身。
这个方法可以让浏览器判断页面是否被修改过，从而控制缓存。也可判断所请求的文档是否存在。
例如，假如你的网站上有很多链接，那么你就可以简单的给他们分别发送HEAD请求来判断是否存在死链，这比使用GET要快很多。
#### 4、PUT：(webdav) 上传 
#### 5、DELETE：(webdav) 删除 
#### 6、OPTIONS：返回请求的资源所支持的方法的方法 
#### 7、TRACE: 追求一个资源请求中间所经过的代理 
### 请求的协议
http/0.9: stateless
http/1.0: MIME, keep-alive (保持连接), 缓存
http/1.1: 更多的请求方法，更精细的缓存控制，持久连接(persistent connection) 比较常用
### HTTP请求报文头部信息
Accept  就是告诉服务器端，我接受那些MIME类型
Accept-Encoding  这个看起来是接受那些压缩方式的文件
Accept-Lanague   告诉服务器能够发送哪些语言 
Connection       告诉服务器支持keep-alive特性
Cookie           每次请求时都会携带上Cookie以方便服务器端识别是否是同一个客户端
Host             用来标识请求服务器上的那个虚拟主机，比如Nginx里面可以定义很多个虚拟主机
                 那这里就是用来标识要访问那个虚拟主机。
User-Agent       用户代理，一般情况是浏览器，也有其他类型，如：wget curl 搜索引擎的蜘蛛等
条件请求首部
If-Modified-Since 是浏览器向服务器端询问某个资源文件如果自从什么时间修改过，那么重新发给我，这样就保证服务器端资源文件更新时，浏览器再次去请求，而不是使用缓存中的文件
安全请求首部：
Authorization    客户端提供给服务器的认证信息；
MIME 遵循以下格式：major/minor 主类型/次类型 例如：
```
image/jpg
image/gif
text/html
video/quicktime
appliation/x-httpd-php
```
### HTTP响应头部信息
Connection            使用keep-alive特性
Content-Encoding      使用gzip方式对资源压缩
Content-type          MIME类型为html类型，字符集是 UTF-8
Date                  响应的日期
Server                使用的WEB服务器
Transfer-Encoding:chunked   分块传输编码 是http中的一种数据传输机制，允许HTTP由网页服务器发送给客户端应用（通常是网页浏览器）的数据可以分成多个部分，分块传输编码只在HTTP协议1.1版本（HTTP/1.1）中提供
Vary  这个可以参考（http://blog.csdn.net/tenfyguo/article/details/5939000）
X-Pingback  参考（http://blog.sina.com.cn/s/blog_bb80041c0101fmfz.html）
### HTTP响应状态码
1xx: 信息性状态码 
&emsp;&emsp;100, 101
2xx: 成功状态码 
&emsp;&emsp;200：OK
3xx: 重定向状态码
&emsp;&emsp;301: 永久重定向, Location响应首部的值仍为当前URL，因此为隐藏重定向;
&emsp;&emsp;302: 临时重定向，显式重定向, Location响应首部的值为新的URL
&emsp;&emsp;304：Not Modified  未修改，比如本地缓存的资源文件和服务器上比较时，发现并没有修改，服务器返回一个304状态码，告诉浏览器，你不用请求该资源，直接使用本地的资源即可。
4xx: 客户端错误状态码
&emsp;&emsp;404: Not Found  请求的URL资源并不存在
5xx: 服务器端错误状态码
&emsp;&emsp;500: Internal Server Error  服务器内部错误
&emsp;&emsp;502: Bad Gateway  前面代理服务器联系不到后端的服务器时出现
&emsp;&emsp;504：Gateway Timeout  这个是代理能联系到后端的服务器，但是后端的服务器在规定的时间内没有给代理服务器响应

