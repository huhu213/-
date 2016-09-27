## HTML5新技术

### 1. Web Workers

Web Works是一种脚本操作在独立于web应用主线程的后台线程中运行的机制，从而使得耗时的操作均可以在单独的线程中运行，而不阻塞/减慢主线程的运行。运行在后台的任务通过Worker接口定义，可以发送消息给创建该任务的执行环境(window)。

JavaScript执行会阻塞用户与页面的交互。

Worker是一个对象，通过构造函数来引入需要独立运行的js文件；**workers的执行上下文不是当前的window对象，而是由DedicatedWorkerGlobalScope表示的对象（共享的js文件通过ShareWorkerGlobalScope来包装）。

在Workers中，不能直接操作DOM对象，或者操作window对象下的一些默认的方法和属性。但是可以操作WebSockets，IndexedDB，或者FireFox专用的Data Store API。具体见[Workers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Functions_and_classes_available_to_workers)

Workers和主线程的通信通过postMessage实现，通过onmessage(Message)事件进行监听。传递的数据是基于拷贝，而不是基于共享的。

Workers可以产生新的worker，只要这些worker同源。同时，workers通过XMLHTTPRequest进行network I/O，但是XMLHttpRequest中的responseXML和channel属性均为null。
