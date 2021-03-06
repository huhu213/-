#CSS基础

###盒子模型

盒子模型规定了元素框处理元素内容，包含margin，border，padding以及content。

IE盒子中，width = padding + content
>
>     box-sizing: border-box

标准盒子中，width = content
>
>     box-sizing: content-box

###块级元素和行内元素的特性和区别

行内元素在水平方向上排列，块级元素在垂直方向上排列。行内元素设置width、height、margin上下、padding上下无效，通过line-height设定高度。

text-align属性作用于块级元素内的行内元素的居中对齐。

###常用的图片格式

png/ipeg/gif/svg/webp等。webp由google开发的一种旨在加快图片加载速度的图片格式，压缩后体积大约只有jpg的2/3，能节省大量服务器带宽资源和数据空间。

### 垂直居中

* 绝对居中
>      .absolute-center {
>           position: absolute;
>           top: 0, left: 0, right: 0, bottom: 0;
> 			    margin: auto;
>      }

需设置内容元素的高

### CSS中link和@import的区别
* link是HTML标签，@import是CSS关键字，用于引入样式
* 页面加载时，link对应的css文件会同时被加载，而@import引入的样式则在页面加载完成之后才进行加载

### CSS清除浮动

1. 使用带clear属性的空元素
>     <div class="container">
           <div style="float: left"></div>
           <div style="float: left"></div>
           <div style="clear: both"></div>
      </div>

2. 使用css的overflow属性-设置父元素overflow:hidden；子元素高度大于父元素时，会使得超出部分被隐藏,不支持IE6
3. 使用伪元素:after
>     <div class="container">
           <div style="float: left"></div>
           <div style="float: left"></div>
      </div>
      .container:after {
          content: '\0020';
          display: block;
          height: 0;
          clear: both;
      }
      .container {
          zoom: 1;//兼容IE6，通过Layout实现页面布局
      }

4. 使用邻接元素处理

DOM树和渲染树不是一一对应的，不可见的元素比如head不会被渲染，display:none的元素也不会添加到渲染树种，但visibility: hidden的元素会。
