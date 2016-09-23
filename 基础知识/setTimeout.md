#setTimeout&setInterval

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
