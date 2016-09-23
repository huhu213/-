#CSS Sprite

CSS Sprite通过将多张图片合成到一张图片上，减少页面加载过程中的http请求数量，减小图片总size，以及提前加载图片，提高网站性能。通过background-position定位大图上的子图进行图片获取。

使用CSS合成器生成整张大图，或者通过PS等编辑工具合成，合成后可以得到每张图对应的CSS样式如下：
>     .sprite1 { 
>         background: url(csssprite.png) no-repeat -0px -1024px;
>         width: 640px;
>         height: 620px;
>      }
>      .sprite2 {
>           background: url(csssprite.png) no-repeat -0px -1644px;
>           width: 450px; 
>           height: 450px; 
>       }
>       .sprite3 { 
>            background: url(csssprite.png) no-repeat -450px -1644px; 
>            width: 450px; 
>            height: 450px;
>       }




