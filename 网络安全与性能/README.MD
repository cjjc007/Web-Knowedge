* TCP 和 UDP
* 三次握手，四次分手
* HTTP和HTTPS
* 状态码
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
1XX	Informational（信息性状态码）	    接受的请求正在处理  
2XX	Success（成功状态码）	           请求正常处理完毕  
3XX	Redirection（重定向状态码）	      需要进行附加操作以完成请求  
4XX	Client Error（客户端错误状态码） 	服务器无法处理请求  
5XX	Server Error（服务器错误状态码） 	服务器处理请求出错  
  
200 OK：请求已正常处理。  
204 No Content：请求处理成功，但没有任何资源可以返回给客户端，一般在只需要从客户端往服务器发送信息，而对客户端不需要发送新信息内容的情况下使用。  
206 Partial Content：是对资源某一部分的请求，该状态码表示客户端进行了范围请求，而服务器成功执行了这部分的GET请求。响应报文中包含由Content-Range指定范围的实体内容。  
301 Moved Permanently：资源的uri已更新，你也更新下你的书签引用吧。永久性重定向，请求的资源已经被分配了新的URI，以后应使用资源现在所指的URI。  
302 Found：资源的URI已临时定位到其他位置了，姑且算你已经知道了这个情况了。临时性重定向。和301相似，但302代表的资源不是永久性移动，只是临时性性质的。换句话说，已移动的资源对应的URI将来还有可能发生改变。  
303 See Other：资源的URI已更新，你是否能临时按新的URI访问。该状态码表示由于请求对应的资源存在着另一个URL，应使用GET方法定向获取请求的资源。  
303状态码和302状态码有着相同的功能，但303状态码明确表示客户端应当采用GET方法获取资源，这点与302状态码有区别。  
当301,302,303响应状态码返回时，几乎所有的浏览器都会把POST改成GET，并删除请求报文内的主体，之后请求会自动再次发送。  
  
304 Not Modified：资源已找到，但未符合条件请求。该状态码表示客户端发送附带条件的请求时（采用GET方法的请求报文中包含If-Match，If-Modified-Since，If-None-Match，If-Range，If-Unmodified-Since中任一首部）服务端允许请求访问资源，但因发生请求未满足条件的情况后，直接返回304。  
307 Temporary Redirect：临时重定向。与302有相同的含义。  
400 Bad Request：服务器端无法理解客户端发送的请求，请求报文中可能存在语法错误。  
401 Unauthorized：该状态码表示发送的请求需要有通过HTTP认证（BASIC认证，DIGEST认证）的认证信息。  
403 Forbidden：不允许访问那个资源。该状态码表明对请求资源的访问被服务器拒绝了。（权限，未授权IP等）  
404 Not Found：服务器上没有请求的资源。路径错误等。  
500 Internal Server Error：貌似内部资源出故障了。该状态码表明服务器端在执行请求时发生了错误。也有可能是web应用存在bug或某些临时故障。  
503 Service Unavailable：抱歉，我现在正在忙着。该状态码表明服务器暂时处于超负载或正在停机维护，现在无法处理请求。  


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
### jsonp
### cors
### postMessage
### ducument.name
### location.hash
### http-proxy
### nginx
### webscoket

## Event loop

## 存储
### cooket
### session
### localStorage
### sessionStorage

## 渲染机制
## XSS、CSRF



