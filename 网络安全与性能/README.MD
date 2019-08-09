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
OSI(Open System Interconnection)开放系统互联参考模型，定义了网络互连的七层框架，由低到高分别为：物理层、数据链路层、网络层、传输层、会话层、表示层、应用层。  
#### TCP：传输控制协议(Transmission Control Protocol)  
是面向连接的协议，也就是说，在收发数据前，必须和对方建立可靠的连接。  
一个TCP连接必须要经过三次“对话”才能建立，断开需要四次会话。  
由TCP的三次握手和四次断开可以看出,TCP使用面向连接的通信方式,大大提高了数据通信的可靠性,使发送数据端和接收端在数据正式传输前就有了交互,为数据正式传输打下了可靠的基础。  
#### UDP：用户数据报协议 （UDP：User Datagram Protocol)  
（1） UDP是一个非连接的协议，传输数据之前源端和终端不建立连接，当它想传送时就简单地去抓取来自应用程序的数据，并尽可能快地把它扔到网络上。在发送端，UDP传送数据的速度仅仅是受应用程序生成数据的速度、计算机的能力和传输带宽的限制；在接收端，UDP把每个消息段放在队列中，应用程序每次从队列中读一个消息段。  
（2） 由于传输数据不建立连接，因此也就不需要维护连接状态，包括收发状态等，因此一台服务机可同时向多个客户机传输相同的消息。  
（3） UDP信息包的标题很短，只有8个字节（16位），相对于TCP的20个字节信息包的额外开销很小。  
（4） 吞吐量不受拥挤控制算法的调节，只受应用软件生成数据的速率、传输带宽、源端和终端主机性能的限制。  
（5）UDP使用尽最大努力交付，即不保证可靠交付，因此主机不需要维持复杂的链接状态表（这里面有许多参数）。  
（6）UDP是面向报文的。发送方的UDP对应用程序交下来的报文，在添加首部后就向下交付给IP层。既不拆分，也不合并，而是保留这些报文的边界，因此，应用程序需要选择合适的报文大小。  
#### TCP与UDP的区别：
TCP 是面向连接的，UDP 是面向无连接的  
UDP程序结构较简单  
TCP 是面向字节流的，UDP 是基于数据报的  
TCP 保证数据正确性，UDP 可能丢包  
TCP 保证数据顺序，UDP 不保证  

#### UDP应用场景：
直播：直播对实时性的要求比较高，宁可丢包，也不要卡顿的，所以很多直播应用都基于 UDP 实现了自己的视频传输协议  
实时游戏：游戏的特点也是实时性比较高，在这种情况下，采用自定义的可靠的 UDP 协议，自定义重传策略，能够把产生的延迟降到最低，减少网络问题对游戏造成的影响  
#### TCP 为什么是可靠连接
通过 TCP 连接传输的数据无差错，不丢失，不重复，且按顺序到达。  
TCP 报文头里面的序号能使 TCP 的数据按序到达  
报文头里面的确认序号能保证不丢包，累计确认及超时重传机制  
TCP 拥有流量控制及拥塞控制的机制  
  
## 三次握手，四次分手
确认ACK，仅当ACK=1时，确认号字段才有效。TCP规定，在连接建立后所有报文的传输都必须把ACK置1；  
同步SYN，在连接建立时用来同步序号。当SYN=1，ACK=0，表明是连接请求报文，若同意连接，则响应报文中应该使SYN=1，ACK=1；  
终止FIN，用来释放连接。当FIN=1，表明此报文的发送方的数据已经发送完毕，并且要求释放；  
序列号seq

#### 三次握手过程
* TCP服务器进程先创建传输控制块TCB，时刻准备接受客户进程的连接请求，此时服务器就进入了LISTEN（监听）状态；
* TCP客户进程也是先创建传输控制块TCB，然后向服务器发出连接请求报文，这是报文首部中的同部位SYN=1，同时选择一个初始序列号 seq=x ，此时，TCP客户端进程进入了 SYN-SENT（同步已发送状态）状态。TCP规定，SYN报文段（SYN=1的报文段）不能携带数据，但需要消耗掉一个序号。
* TCP服务器收到请求报文后，如果同意连接，则发出确认报文。确认报文中应该 ACK=1，SYN=1，确认号是ack=x+1，同时也要为自己初始化一个序列号 seq=y，此时，* TCP服务器进程进入了SYN-RCVD（同步收到）状态。这个报文也不能携带数据，但是同样要消耗一个序号。
* TCP客户进程收到确认后，还要向服务器给出确认。确认报文的ACK=1，ack=y+1，自己的序列号seq=x+1，此时，TCP连接建立，客户端进入ESTABLISHED（已建立连接）状态。TCP规定，ACK报文段可以携带数据，但是如果不携带数据则不消耗序号。
* 当服务器收到客户端的确认后也进入ESTABLISHED状态，此后双方就可以开始通信了。

#### 四次分手过程
* 客户端进程发出连接释放报文，并且停止发送数据。释放数据报文首部，FIN=1，其序列号为seq=u（等于前面已经传送过来的数据的最后一个字节的序号加1），此时，客户端进入FIN-WAIT-1（终止等待1）状态。 TCP规定，FIN报文段即使不携带数据，也要消耗一个序号。
* 服务器收到连接释放报文，发出确认报文，ACK=1，ack=u+1，并且带上自己的序列号seq=v，此时，服务端就进入了CLOSE-WAIT（关闭等待）状态。TCP服务器通知高层的应用进程，客户端向服务器的方向就释放了，这时候处于半关闭状态，即客户端已经没有数据要发送了，但是服务器若发送数据，客户端依然要接受。这个状态还要持续一段时间，也就是整个CLOSE-WAIT状态持续的时间。
* 客户端收到服务器的确认请求后，此时，客户端就进入FIN-WAIT-2（终止等待2）状态，等待服务器发送连接释放报文（在这之前还需要接受服务器发送的最后的数据）。
* 服务器将最后的数据发送完毕后，就向客户端发送连接释放报文，FIN=1，ack=u+1，由于在半关闭状态，服务器很可能又发送了一些数据，假定此时的序列号为seq=w，此时，服务器就进入了LAST-ACK（最后确认）状态，等待客户端的确认。
* 客户端收到服务器的连接释放报文后，必须发出确认，ACK=1，ack=w+1，而自己的序列号是seq=u+1，此时，客户端就进入了TIME-WAIT（时间等待）状态。注意此时TCP连接还没有释放，必须经过2∗MSL（最长报文段寿命）的时间后，当客户端撤销相应的TCB后，才进入CLOSED状态。
* 服务器只要收到了客户端发出的确认，立即进入CLOSED状态。同样，撤销TCB后，就结束了这次的TCP连接。可以看到，服务器结束TCP连接的时间要比客户端早一些。

## HTTP1.0和HTTP2.0
1.HTTP2使用的是二进制传送，HTTP1.X是文本（字符串）传送。  
HTTP1.X使用的是明文的文本传送，而HTTP2使用的是二进制传送，二进制传送的单位是帧和流，帧组成了流，同时流还有流ID标示。  
HTTP2帧具有优先级，允许客户端提供排序思路，以让服务器优先处理一部分请求，仍然是以二进制帧的形式返回数据。  
2.HTTP2支持多路复用  
因为有流ID，所以通过同一个http请求实现多个http请求传输变成了可能，可以通过流ID来标示究竟是哪个流从而定位到是哪个http请求。  
HTTP1中的Keep-Alive用于长连接而不必重新建立连接，然而keep-alive必须等本次请求彻底完成后才能发送下一个请求。  
3.HTTP2头部压缩  
HTTP2通过gzip和compress压缩头部然后再发送，同时客户端和服务器端同时维护一张头信息表，所有字段都记录在这张表中，这样后面每次传输只需要传输表里面的索引Id就行，通过索引ID就可以知道表头的值了。  
4.HTTP2支持服务器推送  
服务端可以主动推送资源给客户端，避免客户端花过多的时间逐个请求资源，这样可以降低整个请求的响应时间。  


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
通常为了减轻web服务器的负载，我们把js、css，img等静态资源分离到另一台独立域名的服务器上，在html页面中再通过相应的标签从不同域名下加载静态资源，而被浏览器允许，基于此原理，我们可以通过动态创建script，再请求一个带参网址实现跨域通信。  
缺点：只能实现get一种请求，不安全，XSS。
```javascript
function jsonp({url, params, cb}) {
    return new Promise((resolve, reject) => {
        let script = document.createElement('script')
        window[cb] = function (data) {
            resolve(data)
            document.body.removeChild(script)
        }
        params = {...params, cb}
        let arr = []
        for(let key in params) {
            arr.push(`${key}=${params[key]}`)
        }
        script.src = `${url}?${arr.join('&')}`
        document.body.appendChild(script)
    })
}

jsonp({
    url: 'https://www.baidu.com/su',
    params: {wd:'b'},
    cb: 'show'
}).then(data => {
    console.log(data)
})
```
### cors
思想就是使用自定义的HTTP头部让浏览器与服务器进行沟通，从而决定请求或响应是应该成功，还是应该失败。  
只服务端设置Access-Control-Allow-Origin即可，前端无须设置。  
```javascript
// Nodejs后台示例：
const express = require('express')

let app = express()
let whiteList = ['http://localhost:3000']
app.use(function(req, res, next) {
    let origin = req.header.origin
    if(whiteList.includes(origin)) {
        // 允许哪个源访问
        res.setHeader('Access-Control-Allow-Origin', origin)
        // 允许携带哪个头访问我
        res.setHeader('Access-Control-Allow-Header', 'name,show')
        // 允许哪个方法访问我
        res.setHeader('Access-Control-Allow-Methods', 'PUT')
        // 允许携带coolie
        res.setHeader('Access-Control-Allow-Credentials', true)
        // 预检存活时间
        res.setHeader('Access-Control-Max-Age', 6)
        // 允许前端获取哪个头(允许返回的头)
        res.setHeader('Access-Control-Expose-Header', 'name')

        if(req.method === 'OPTION') {
            res.end()
        }
    }
    next()
})
app.get('/getData', function(req,res) {
    res.setHeader('name', 'cj')
    res.end('bububu')
})
app.put('/getData', function(req,res) {
    res.end('bububu')
})

app.use(express.static(__dirname))
app.listen(4000)

// 前端ajax代码
```javascript
<script src="" async defer>
            let xhr = new XMLHttpRequest
            document.cookie = 'name=cj'
            xhr.withCredentials = true  //携带凭证，就带上了cookie
            xhr.open('GET', 'http://localhost:4000/getData', true)
            xhr.setRequestHeader('name','cj')
            xhr.onreadystatechange = function() {
                if(xhr.readyState === 4) {
                    if(xhr.status >= 200 && xhr.status < 300 || xhr.status === 304) {
                        console.log(xhr.response)
                        console.log(xhr.getResponseHeader('name'))
                    }
                }
            }
            xhr.send()
        </script>
```
```

### postMessage
postMessage是HTML5 XMLHttpRequest Level 2中的API，且是为数不多可以跨域操作的window属性之一，它可用于解决以下方面的问题：  
a.） 页面和其打开的新窗口的数据传递  
b.） 多窗口之间消息传递  
c.） 页面与嵌套的iframe消息传递  
d.） 上面三个场景的跨域数据传递  

用法：postMessage(data,origin)方法接受两个参数  
data： html5规范支持任意基本类型或可复制的对象，但部分浏览器只支持字符串，所以传参时最好用JSON.stringify()序列化。  
origin： 协议+主机+端口号，也可以设置为*,表示可以传递给任意窗口，如果要指定和当前窗口同源的话设置为"/"。  
```javascript
// a.com/index.html中的代码：
<iframe id="ifr" src="b.com/index.html"></iframe>
<script type="text/javascript">
window.onload = function() {
    var ifr = document.getElementById('ifr');
    var targetOrigin = 'http://b.com';  // 若写成'http://b.com/c/proxy.html'效果一样
                                        // 若写成'http://c.com'就不会执行postMessage了
    ifr.contentWindow.postMessage('I was there!', targetOrigin);
    window.onmessage = function(e){
      console.log(e);
    }
};
</script>

// b.com/index.html中的代码：
<script type="text/javascript">
    window.onmessage = function(e){
      console.log(e)
      e.source.postMessage('i am coming', e.targetOrigin)
    }
</script>
```
### document.domain
此方案仅限主域相同，子域不同的跨域应用场景。  
实现原理：两个页面都通过js强制设置document.domain为基础主域，就实现了同域。  
```javascript
1.）父窗口：(www.domain.com/a.html)
<iframe id="iframe" src="http://child.domain.com/b.html"></iframe>
<script>
    document.domain = 'domain.com';
    var user = 'admin';
</script>

2.）子窗口：(child.domain.com/b.html)
<script>
    document.domain = 'domain.com';
    // 获取父窗口中变量
    alert('get js data from parent ---> ' + window.parent.user);
</script>
```

### window.name + iframe
a和b同源，和c不同源  
a先引用c，c把值放在window.name，把a引用的地址改成b  
```javascript
// localhost:3000/1.html
  <iframe src="http://localhost:4000/3.html" frameborder="0" id="frame" onload="load"></iframe>
  <script src="" async defer>
      let first = TextTrackCue
      function load() {
          if(first){
              let frame = document.getElementById('iframe')
              frame.src = 'http://localhost:3000/2.html'
              first = false
          } else {
              console.log(frame.contentWindow.name)
          }
      }
  </script>
  
// localhost:3000/2.html
// 中间代理页，内容为空即可

// localhost:4000/3.html
<script src="" async defer>
    window.name = 'hahaha'    
</script>
```
### location.hash + iframe
实现原理： a欲与b跨域相互通信，通过中间页c来实现。 三个页面，不同域之间利用iframe的location.hash传值，相同域之间直接js访问来通信。
具体实现：A域：a.html -> B域：b.html -> A域：c.html，a与b不同域只能通过hash值单向通信，b与c也不同域也只能单向通信，但c与a同域，所以c可通过parent.parent访问a页面所有对象。
```javascript
// a.html：(www.domain1.com/a.html)
<iframe id="iframe" src="http://www.domain2.com/b.html" style="display:none;"></iframe>
<script>
    var iframe = document.getElementById('iframe');
    // 向b.html传hash值
    setTimeout(function() {
        iframe.src = iframe.src + '#user=admin';
    }, 1000);
    // 开放给同域c.html的回调方法
    function onCallback(res) {
        alert('data from c.html ---> ' + res);
    }
</script>

// b.html：(www.domain2.com/b.html)
<iframe id="iframe" src="http://www.domain1.com/c.html" style="display:none;"></iframe>
<script>
    var iframe = document.getElementById('iframe');

    // 监听a.html传来的hash值，再传给c.html
    window.onhashchange = function () {
        iframe.src = iframe.src + location.hash;
    };
</script>

// c.html：(www.domain1.com/c.html)
<script>
    // 监听b.html传来的hash值
    window.onhashchange = function () {
        // 再通过操作同域a.html的js回调，将结果传回
        window.parent.parent.onCallback('hello: ' + location.hash.replace('#user=', ''));
    };
</script>
```
### http-proxy
### nginx
### webscoket
WebSocket protocol是HTML5一种新的协议。它实现了浏览器与服务器全双工通信，同时允许跨域通讯，是server push技术的一种很好的实现。

## Event loop

## 存储
### cooket
1、前后端数据交互过程中HTTP是无状态的，只是接到请求，然后发送数据，并不关心对方的身份；  
2、不是长久性的链接，对于请求要记住对方的身份比较困难，所以出现了cookie：就是为了在客户端保持用户身份。  
浏览器会开辟文件存放cookie，不删除就一直存在，而每一次请求时，会在HTTP头里带上cookie信息，这样子后端就知道是谁了。  
存储方式是key-value的键值对存在本地加密文件里。  
还受域名和路径的限制，一个请求只能操作本地cookie  
属性：  
name / domain:生效的域名 / path:生效的路径 / expire:过期时间 / httpOnly:禁止客户端修改

### session
在服务器上保存用户信息的机制。  
服务器接到请求后，会生成sessionId,存放在cookie里面，再次请求的时候就知道谁发的请求了。  
cookie消失，session也就消失。  

### localStorage
cookie本身小，只有4k，把所有本地信息都放在cookie里不科学，当一些只与前台有关的数据，不需要发给服务端的那些，就会占用资源。  
localStorege容量有5m，也只在当前域名下使用，只用key-value的形式存储  
没有过期时间，浏览器关闭也不会消失。  
只有手动删除才会消失。  

### sessionStorage
与localStorage不同就是会在会话结束浏览器关闭后消失。

## 渲染机制
## XSS、CSRF
#### XSS：跨站脚本攻击
XSS其实就是Html的注入问题，攻击者的输入没有经过严格的控制进入了数据库，最终显示给来访的用户，导致可以在来访用户的浏览器里以浏览用户的身份执行Html代码，数据流程如下：攻击者的Html输入—>web程序—>进入数据库—>web程序—>用户浏览器。  
* 避免方法：
1、将用户所提供的内容进行过滤。  
2、很多时候可以使用HTTP头指定内容的类型，使得输出的内容避免被作为HTML解析。  
('Content-Type: text/javascript; charset=utf-8');  
即可强行指定输出内容为文本/JavaScript脚本（顺便指定了内容编码），而非可以引发攻击的HTML。  

#### CSRF：跨站请求伪造（冒充用户之手，伪造请求）
绝大多数网站是通过 cookie 等方式辨识用户身份（包括使用服务器端 Session 的网站，因为 Session ID 也是大多保存在 cookie 里面的），再予以授权的。所以要伪造用户的正常操作，最好的方法是通过 XSS 或链接欺骗等途径，让用户在本机（即拥有身份 cookie 的浏览器端）发起用户所不知道的请求。   
要完成一次CSRF攻击，受害者必须依次完成两个步骤：  
1.登录受信任网站A，并在本地生成Cookie。  
2.在不登出A的情况下，访问危险网站B。  

防御 CSRF 的方法：  
针对表单比较可行的解决方案遵循三步：  
1.在用户登录时，设置一个CSRF的随机token，同时种植在用户的cookie中，当用户浏览器关闭或者再次登录、退出时，清除token;  
2.在表单中，生成一个隐藏域，它的值就是cookie中随机token，  
3.表单提交后，在web服务器端，判断表单中是token是否和用户cookie中的token一致，如果不一致或者为空，就判断为CSRF攻击。  

