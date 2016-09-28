##BFC--块级元素格式上下文

Block Formatting Context提供了一个环境，HTML元素在这个环境中按照一定规则进行布局。一个环境中的元素不会影响到其它环境中的布局。也可以说BFC就是一个作用范围。可以把它理解成是一个独立的容器，并且这个容器的里box的布局，与这个容器外的毫不相干。

处于同一个BFC中的两个垂直窗口的margin会重叠，即边界塌陷；
生成 block formatting context 的元素不会和在流中的子元素发生空白边折叠。解决这种问题的办法是要为两个容器添加具有BFC的包裹容器，比如将每个子元素包裹在父元素中，且触发父元素的BFC。

Block Formatting Context就是页面上的一个隔离的独立容器，容器里面的子元素不会在布局上影响到外面的元素，反之也是如此。

[相关文章](http://www.cnblogs.com/pigtail/archive/2013/01/23/2871627.html)

###产生BFC
* float值不为none
* overflow值不为visible
* display的值为table-cell, table-caption, inline-block中的任何一个
* position的值不为relative和static

**清除浮动时，设置overflow:hidden也是为了创建父元素的BFC，不受子元素的浮动影响；IE6下设置zoom: 1即可**

##Layout——IE渲染引擎
在 IE 浏览器中，一个元素要么自己对自身的内容进行组织和计算大小， 要么依赖于包含块来计算尺寸和组织内容。渲染引擎采用了 ‘hasLayout’ 属性，属性值可以为 true 或 false。 当一个元素的 ‘hasLayout’ 属性值为 true 时，我们说这个元素有一个布局（layout），或拥有布局，即对应元素是不是触发了BFC。

IE设定了一些元素默认拥有layout，比如：
> <html>, <body>
<table>, <tr>, <th>, <td>
<img>
<hr>
<input>, <button>, <select>, <textarea>, <fieldset>, <legend>
<iframe>, <embed>, <object>, <applet>
<marquee>

或者通过以下CSS触发Layout：
>
display: inline-block
height: (除 auto 外任何值)
width: (除 auto 外任何值)
float: (left 或 right)
position: absolute
writing-mode: tb-rl
zoom: (除 normal 外任意值)
min-height: (任意值)
min-width: (任意值)
max-height: (除 none 外任意值)
max-width: (除 none 外任意值)
overflow: (除 visible 外任意值，仅用于块级元素)
overflow-x: (除 visible 外任意值，仅用于块级元素)
overflow-y: (除 visible 外任意值，仅用于块级元素)
position: fixed
