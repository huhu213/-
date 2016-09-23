###跨域

协议、域名、端口有一个不同，都是被浏览器当做不同的域。

#### 同源策略
同源策略限制了一个源（origin）中加载文本或脚本与来自其它源（origin）中资源的交互方式。

1. 限制一，不允许读取不同源的服务器中的数据
2. 限制二，不同域的框架之间进行js交互操作


### 解决跨域的方法

#### 一、跨域资源共享(Cross-Origin Resource Sharing)
跨域资源共享，定义了必须在访问跨域资源时，浏览器与服务器该如何沟通。CORS的基本思想就是，使用自定义的HTTP头部让浏览器与服务器进行沟通，从而决定请求或响应是否成功。

跨域的限制是由浏览器限制的，浏览器本身不允许跨域请求资源。服务器端对于CORS的支持，主要就是通过摄者Access-Control-Allow-Origin来进行(响应消息中)。如果浏览器检测到相应的设置，就允许Ajax进行跨域访问。

>      Access-Control-Allow-Credentials:true
>      Access-Control-Allow-Origin:https://segmentfault.com //表示服务器设置了允许该域名访问服务器，因此浏览器允许Ajax请求

#### 二、JSONP（JSON with Padding）

浏览器不允许跨域访问，即XMLHttpRequest不允许跨域请求资源，但是script标签包裹的文件（比如js），可以使当前网页获取其他服务器上的文件。通过在script标签中设定src为获取文件的服务器的url，可以获取到对应的数据。

JSONP的定义是JSON with Padding，即填充式JSON，将数据包含在函数的回调函数里。

**注意，JSONP方式获取的数据不是json，而是任意js，由js解释器执行(即浏览器)**

由于script标签中src对应的url返回的必须是可执行的js，因此通过将json数据包含在回调函数中，即JSON with Padding，使得客户端能接收到数据。

惯例上浏览器提供回调函数的名称当作送至服务器的请求中命名查询参数的一部分，比如
>      <script type="text/javascript" src="http://server2.example.com/RetrieveUser?UserId=1823&jsonp=parseResponse"></script>

服务器返回的是一个可执行的脚本，即函数parseResponse({"name": "test"})。

为了要启动一个JSONP呼叫（或者说，使用这个模式），你需要一个script 元素。因此，浏览器必须为每一个JSONP要求加（或是重用）一个新的、有所需 src值的script元素到HTML DOM里—或者说是“注入”这个元素。浏览器执行该元素，抓取src里的URL，并执行回传的 JavaScript。

也因为这样，JSONP被称作是一种“让使用者利用script元素注入的方式绕开同源策略”的方法。

#### jQuery中使用JSONP
jquery会自动生成一个全局函数来替换callback=?中的问号，之后获取到数据后又会自动销毁，实际上就是起一个临时代理函数的作用。$.getJSON方法会自动判断是否跨域，不跨域的话，就调用普通的ajax方法；跨域的话，则会以异步加载js文件的形式来调用jsonp的回调函数。

>       <script type="text/javascript"> 
>         $.getJSON('http://example.com/data.php?
>           callback=?,function(jsondata)'){
>            //处理获得的json数据
>         });
>       </script>

#### JSONP的优点/缺点
* 不受同源策略的限制
* 兼容性更好，在较低版本浏览器(不支持CORS)中也可以跨域获取资源，不需要XMLHttpRequest或者ActiveX的支持
* 只支持GET请求，不支持POST等其他类型的HTTP请求
* 只能解决HTTP请求跨域，不能解决不同域的两个页面之间进行js通信的问题

#### document.domain

>      <script type="text/javascript">
    function test(){
        var iframe = document.getElementById('￼ifame');
        var win = document.contentWindow;//可以获取到iframe里的window对象，但该window对象的属性和方法几乎是不可用的
        var doc = win.document;//这里获取不到iframe里的document对象
        var name = win.name;//这里同样获取不到window对象的name属性
    } 
    </script>
    <iframe id = "iframe" src="http://example.com/b.html" onload = "test()"></iframe>
    
document.domain可以解决不同子域框架间的交互。要求主域相同，且只能设置为自身或者更高一级的父域。

#### window.name

window对象的name属性被声明周期内当前窗口(window)的所有页面共享。每个页面对window.name都有读写的权限，window.name持久存在在一个窗口载入过的所有页面。

#### window.postMessage
H5新增的方式，允许在不同域间传递数据。

targetWindow(message, targetOrigin, [transfer]);

由test.html向内嵌的来自于test1.html的frame发送数据。

> test.html
> 
>      <div style="height: 500px; width: 80%; margin:auto">
		<iframe src="file:///Users/huhu/Desktop/html/test1.html" style="height: 400px; width: 100%; margin: auto; border: 1px solid #CCC"></iframe>
	</div>

>      <script type="text/javascript">
		window.onload = function() {
			window.frames[0].postMessage("hello", "*");
		}
		window.onmessage = function(event) {
			alert(event.data);
		}
	</script>

> test1.html
> 
>      <div style="height: 400px; background-color: #CCC"></div>

>      <script type="text/javascript">
		window.onmessage = function(event) {
			alert(event.data);
			event.source.postMessage('received', '*');
		}
	 </script>










