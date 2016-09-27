#setTimeout&setInterval

JavaScript运行于单线程的环境，主JavaScript执行进程之外，浏览器维护一个等待队列，负责对代码队列进行排序，以便在下一次进程空闲时执行。Javascript进程会阻塞其他页面处理，因此，需要设定一些小间隔防止用户界面被锁定。或使用HTML5中提供的接口WebWorkers。

**JavaScript中没有任何代码是立即执行的，但是会在浏览器进程空闲时尽快执行**

setTimeout和setInterval都是定时器，setTimeout在指定时间之后调用回调函数；setInterval以指定的时间间隔执行回调函数。

JavaScript引擎单线程处理任务，任务在队列中排队，执行完当前任务开始另外一个任务。

实践中，setTimeout会在其完成当前任何延宕事件的事件处理器的执行以及完成文档当前状态更新后，告诉浏览器去启用setTimeout内的回调函数。setTimeout函数通过CPU中断来完成回调函数的执行。

setTimeout将回调任务从队列中选出，加入到另一个队列里。

>      alert(1);
>      setTimeout(function() {
>          alert(2);
>      }, 0);
>      alert(3);

以上代码执行结果是1，3，2

>      setTimeout(function() {
>          alert(1);
>      }, 0);
>      alert(2);

以上代码执行结果是2，1

###定时器作用——Yeilding Processing
浏览器中JavaScript的执行会阻塞页面的交互，对耗时较长的js代码段需要进行处理，可以通过定时器对循环类操作进行划分，一个时间段执行一小段循环；或者采用HTML5的新特性web workers实现js代码在单独的线程中运行。

当数组循环影响js执行效率时，通过设定定时器对循环进行分割可以有效地处理该类问题，即数组分块（array chunking）。

>       setTimeout(function() {
            var item = arr.shift();
            process(item);
            if(arr.length > 0) {
              setTimeout(arguments.callee, 100);
            }  
        })
