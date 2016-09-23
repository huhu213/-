#Reflow&Repaint

###Reflow

reflow，回流，指的是布局引擎为frame计算图形的过程。frame是一个矩形，拥有宽高和相对父容器的偏移。frame用来显示盒模型（content model），一个盒模型可能显示为多个frame，比如换行的文本每行都会显示为一个frame。

>  A reflow involves changes that affect the layout of a portion of the page(or the whole page). Reflow of an element causes the subsequent reflow of all child and ancestor elements as well as any elements following it in the DOM.

repaint，重绘，发生在元素的可见性发生变化但布局不发生变化时，比如outline，visibility或background color.
>   A repaint occurs when changes are made to an elements skin that changes visibility, but do not affect its layout.

> Reflows are very expensive in terms of performance, and is one of the main causes of slow DOM scripts, especially on devices with low processing power, such as phones. In many cases, they are equivalent to laying out the entire page again.

**Hint: ionic在高版本os的手机上运行比较顺畅，reflow带来的页面re-render可能也是一部分原因。**

写CSS时可能会引起Reflow的因素：

1. Resizing the window，改变窗口大小
2. Changing the font，改变字体大小
3. Adding or removing a stylesheet，添加或移除样式
4. Content changes，比如在input框中输入文本
5. Activation pseudo class伪类样式的触发
6. Script manipulating the DOM，js脚本计算DOM
7. Calculating offsetWidth and offsetHeight，计算偏移量(元素在viewport内可见空间的宽和高)
8. Setting a property of the style attribute，更改样式属性

避免reflow的方法

1. 尽量在DOM树的最里层改变元素的样式
2. 避免设置太多的内联样式
3. 将动画设置到定位为fixed或绝对的元素上，不会影响到其他元素的布局，只引起repaint，而不是full reflow
4. 避免table layout
5. IE下 避免通过js修改css

浏览器通过限制reflow影响邻接节点或者合并一些小的reflow为一个大的reflow(Mozilla's Dirty reflows)来提高性能。
