* TCP 和 UDP
* 三次握手，四次分手
* HTTP和HTTPS
* 状态码
* Webscoket
* 强缓存和协商缓存
* 跨域方式
* 存储
* XSS—跨网站指令码
* CSRF—跨站请求伪造
* 事件机制
* Event loop
* 渲染机制
* 优化渲染过程—懒加载

## TCP 和 UDP
TCP：传输控制协议(Transmission Control Protocol)
UDP：用户数据报协议 （UDP：User Datagram Protocol） 

## 三次握手，四次分手
## HTTP1和HTTP2
#### HTTP请求头

## 状态码
## WebSocket
## 强缓存和协商缓存
#### 浏览器缓存
缓存这东西，第一次必须获取到资源后，然后根据返回的信息来告诉如何缓存资源，可能采用的是强缓存，也可能告诉客户端浏览器是协商缓存，这都需要根据响应的header内容来决定的。
* 浏览器在请求某一资源时，会先获取该资源缓存的header信息，判断是否命中强缓存（cache-control和expires信息），若命中直接从缓存中获取资源信息，包括缓存header信息；本次请求根本就不会与服务器进行通信（状态码200）。
* 如果没有命中强缓存，浏览器会发送请求到服务器，请求会携带第一次请求返回的有关缓存的header字段信息（Last-Modified/If-Modified-Since和Etag/If-None-Match），由服务器根据请求中的相关header信息来比对结果是否协商缓存命中；若命中，则服务器返回新的响应header信息更新缓存中的对应header信息，但是并不返回资源内容，它会告知浏览器可以直接从缓存获取；否则返回最新的资源内容（状态码304）。
#### 强缓存总结 Header：
* cache-control: max-age=xxxx，public
客户端和代理服务器都可以缓存该资源；  
客户端在xxx秒的有效期内，如果有请求该资源的需求的话就直接读取缓存,statu code:200 ，如果用户做了刷新操作，就向服务器发起http请求  
* cache-control: max-age=xxxx，private
只让客户端可以缓存该资源；代理服务器不缓存  
客户端在xxx秒内直接读取缓存,statu code:200  
* cache-control: max-age=xxxx，immutable
客户端在xxx秒的有效期内，如果有请求该资源的需求的话就直接读取缓存,statu code:200 ，即使用户做了刷新操作，也不向服务器发起http请求  
* cache-control: no-cache
跳过设置强缓存，但是不妨碍设置协商缓存；一般如果你做了强缓存，只有在强缓存失效了才走协商缓存的，设置了no-cache就不会走强缓存了，每次请求都回询问服务端。  
* cache-control: no-store
不缓存，这个会让客户端、服务器都不缓存，也就没有所谓的强缓存、协商缓存了。  

#### 协商缓存步骤总结
发请求-->看资源是否过期-->过期-->请求服务器-->服务器对比资源是否真的过期-->过期-->返回200状态码-->客户端如第一次接收该资源一样，记下它的cache-control中的max-age、etag、last-modified等 // -->没过期-->返回304状态码-->客户端用缓存的老资源。

#### 为什么要有etag？(etag是新增的)
* 一些文件也许会周期性的更改，但是他的内容并不改变(仅仅改变的修改时间)，这个时候我们并不希望客户端认为这个文件被修改了，而重新get；
* 某些文件修改非常频繁，比如在秒以下的时间内进行修改，(比方说1s内修改了N次)，if-modified-since能检查到的粒度是秒级的，这种修改无法判断(或者说UNIX记录MTIME只能精确到秒)；
* 某些服务器不能精确的得到文件的最后修改时间。

#### 后端服务器nodejs设置:
res.setHeader('max-age': '3600 public')  
res.setHeader(etag: '5c20abbd-e2e8')  
res.setHeader('last-modified': Mon, 24 Dec 2018 09:49:49 GMT)  


## 优化渲染过程—懒加载
## 事件机制
## 跨域方式
## Event loop
## 存储
## 渲染机制
## XSS、CSRF



