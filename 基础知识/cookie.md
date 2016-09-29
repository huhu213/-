#cookie

cookies是服务器端用于在http客户端保存状态的位于http请求header域中的一部分，帮助服务端在无状态的http协议上，保存一个有状态的session。

服务端通过在response的header域包含Set-Cookie： sid=""像客户端发送session id等数据，则之后客户端所有在该域的请求都会带上cookie，且cookie中包含session id信息。

服务端可以通过Path/Domain指定客户端发送cookie数据到每一个path和每一个指定的domain的子域。

父域可以获取子域的cookie，子域间不能跨域读取cookie。服务器端忽略Domain时，cookie只会被发送给origin server。当父域客户端发送cookie时，若没有指定Domain的值，可能会默认为当前主机名，从而可能会将cookie同时发送到子域(和浏览器有关)。若服务器

> == Server -> User Agent ==
Set-Cookie: SID=31d4d96e407aad42; Path=/; Domain=example.com
   == User Agent -> Server ==
   Cookie: SID=31d4d96e407aad42

cookie优点：

* 极高的扩展性和可用性
* 为客户端数据持久化提供方便，减轻服务器端存储压力

但是也有对应的弊端：

* 每个域名下生成的cookie数量(IE6-20, IE7/Firefox-50, chrome/safari无硬性限制)和大小有限制(4KB)
* 安全性问题，cookie中保存了所有session信息
* 有些数据必须保存在服务器端，比如为了防止重复提交表单

##cookie free

cookie信息在js/css/images/flash等静态资源请求时，并没有发挥作用，当图片等资源请求较多时，对页面加载速度有一定影响。解决方案四启用和主站不同的域名来放置静态资源，即cookie free。

##localStorage/sessionStorage

localStorage受跨域的限制，localStorage对象是基于域而存在的。不同子域下的localStorage不能互相访问，可以通过在子域中设定当前document.domain将localStorage绑定到同一个父域。
