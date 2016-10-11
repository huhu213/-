### XSS攻击
XSS, Crossing Site Scripting，跨站脚本漏洞，是web应用程序出现漏洞，使得跨站脚本向网页中写入恶意脚本或HTML代码，因此也称为HTML Injection，HTML注入漏洞。

大部分Web漏洞，都和用户输入信息的处理有关。

#### 1. Non-persistant/Reflect 非持久型/反射型

指的是，浏览器每次都需要在参数中提交恶意数据才能触发的跨站脚本。

比如像http://localhost/test.php?name=huhu<scirpt>恶意代码</script>这类在url中直接提交恶意数据；或者通过表单post恶意数据:
>
>     <form action="http//localhost/test.php" method="post">
>         <input name="name" value="<script>alert(123)</script>" type="button" />
>         <input name="ss" type="submit" value="xss测试" />
>     </form>

点击xss测试，会alert出123

#### 2. Persistant/Stored 持久型/存储型

指的是，通过提交恶意数据到存储位置，比如文本文件、数据库等，导致浏览器将恶意数据读取到页面的漏洞，存到服务器等

常见于，bbs、邮箱等从数据库读取出的页面中。

造成蠕虫传播

#### 3. 本地XSS

htm/html/hta
chm/mht文件

运行本地的js代码


XSS注入: 在页面中引入js代码

关键字替换，注入方式很多种，不能完全

防范XSS cross site js
用户信息处理

Cookie HttpOnly:
包含在HTTP Response Header里，浏览器支持该字段的情况下，不允许被client side script访问，从而防止恶意script文件将cookie信息发送到第三方
在session cookie里也可以设置, WEB-INF/web.xml
>
>     <session-config>
> 	      <cookie-config>
>            <http-only>true</http-only>
>        </cookie-config>
>     </session-config>
>

get->post
http header referrer: browser not support empty
ID校验 用户的id
token
