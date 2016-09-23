# JavaScript函数

ECMAScript中函数Function是引用类型的一种，每个函数都是Function类型的实例，且与其他引用类型一样有属性和方法。

**函数名是指向该函数对象的指针，并不与某个函数绑定。**
多个函数名可以指向同一个函数对象。
> 
>     var sum = function(a, b) {
>         return a + b;
>     }
>     sum(10, 20);//30
> 
> 
>     var anotherSum = sum;
>     anotherSum(10, 20);//30
> 
>     sum = null;
>     anotherSum(10, 20);//30
>    
> 

**函数没有重载**

由于函数名是指针，因此同名函数后面的会覆盖前面的函数，解析器不会根据形参个数和类型进行函数匹配。
>
>     function sum(a) {
>	      return a + 10;
>     }
>     function sum(a, b) {
> 	   	   return a + b;
>     }
> 
>     var result = sum(10);
>     此时result的值是NaN，而不是20；
>     由于没有重载，因此此时调用sum(10)执行的是第二个函数；
>     由于第二个参数b的值是undefined，因此result的值是undefined+10的结果，所以是NaN。

**函数表达式和函数声明**

函数声明

>     function f()

会首先被解析器读取，并使其执行在任何代码之前；

而函数表达式

>     var f = function() {};

则需要在执行它时才会被解析。比如下面的例子：

>     alert(sum(10, 10));
>     function sum(a, b) { return a + b;}
> 
>     alert(sum(10, 10));
>     var sum = function(a, b) { return a + b;}

对于第一个语句执行完全正确，alert结果为20；而第二个由于sum没有声明，因此报错。

函数定义语句，即函数声明会被hoist到顶部，包括函数内部的变量和方法，都会hoist到顶部。

>     var a = "hello";
>     function b() {
>         alert(a);//undefined
>         var a = "world";
>         alert(a);//world
>     }
>     b();

由于函数声明会被hoist到顶部，因此在执行第一句alert(a)时，整个函数被hoist到变量a的定义之前，此时函数内部的a是undefined的；而第二次a则表示是函数内部定义的变量，即world。

函数表达式则是通过

>     var f = function() {};

将匿名函数(lambda函数)赋值给变量。

**递归函数的实现**

1. arguments.callee()调用原函数，只在非严格模式下使用；
2. 通过命名函数表达式实现，在严格模式和非严格模式下都能使用。

>
>     var factorial = (function f(num) {
> 	       if(num < 1) return 1;
>          else return num * f(num-1); 
>     })



**函数内部属性this和arguments**

arguments是类数组对象，包含传入函数的所有参数信息，有一个callee指针属性，指向拥有该arguments对象的函数。
> 
>     function factorial(num) {
>         if(num <= 1) return 1;
>         else return num * arguments.callee(num - 1);
>     }

使用callee的好处是，递归函数不和函数名绑定，即   
>     var f = factorial;
>     alert(f(5));//120

**this**是一个指针，引用的是函数据以执行的环境对象。
>     var color="red";
>     var o = {"color": "blue"};
>     fucntion sayColor() {
> 	       alert(this.color);//red
>     }
> 
>     sayColor.call(o);//blue

ECMAScript5中规范了caller属性，用来指向调用当前函数的函数。若函数作用在全局，则返回结果为null。

严格模式下，arguments.callee会报错，同时函数的caller属性不能被赋值。

**函数属性和方法**

函数作为对象，可以拥有自己的属性和方法，比如上文提到的caller属性。另外一个属性是length，表示函数希望接收的命名参数的个数。

最最重要的函数属性是**prototype**属性。后面会专门介绍，JavaScript中的面向对象和继承都依赖于prototype属性。该属性不可枚举，使用for-in无法发现。

每个函数包含两个非继承的方法apply和call，用于在指定作用域中调用函数。ECMAScript5还定义了bind函数，通过创建一个函数的实例，将函数绑定到指定的对象中。

>     var f = sayColor.bind(o);

将函数sayColor绑定到对象o上，创建一个新函数f，该函数的作用域是对象o，而不是window。


**this指针的存在状态**

* 直接调用——this指向window（ECMAScript5的严格模式下，没有window，此时this指向的是undefined）

>     function f() {alert("hello");}

* 作为对象的方法调用——this指向该对象

>     var o = {"color": "blue"};
>     o.f = function() {alert(o.color);}

* 通过apply(), call(), bind()函数指定this指向的对象
* 作为构造函数调用

>     1.创建一个空对象
>     2.将该对象传递给构造函数，作为this指针的指向对象，同时该对象作为该函数的作用上下文
>     3.除非显示地返回值，则创建的新对象就作为构造函数的返回值






