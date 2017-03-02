### 1.一些概念
  * 执行环境（`execution context`）：JavaScript代码都有都有自己的执行环境，其中定义了变量或者函数有权利访问的所有其他数据。
  * 变量对象（`variable object`）：与执行环境一一对应，每个执行环境都有一个变量对象，执行环境中定义的变量和对象都保存在这个对象中。如果执行环境是一个函数，变量对象即为其活动对象。
  * 全局执行环境（全局对象）：最外围的执行环境，根据宿主确定。浏览器中为`window`对象，Node中为`global`对象

###  2.作用域链
  当js代码执行时，会创建一个变量对象的作用域链，用以保证对可访问的变量和函数的**有序**访问。详看下图。

![作用域链示意](http://upload-images.jianshu.io/upload_images/4472988-88f5bab7ece32082.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在上图中可以看到，一共有三个变量对象：`inner()`函数的变量对象、`outer()`函数的变量对象以及全局变量对象。

在js代码执行的时候，会从当前的执行环境对应的变量对象中寻找变量，如果没有找到，则会沿着作用域链（图中红色箭头的方向）从里向外一层一层的寻找，一直寻找到全局变量对象中。

例如：在`inner`函数中，想要访问`a`变量的值，则先在`inner`的变量对象中寻找，未找到；进而在`inne`r函数的包含环境，也就是`outer`函数的变量对象中寻找，仍未找到；则在`outer`函数的包含环境，也就是`window`对象中寻找，最终找到`a`并访问。如果在整个作用域链上未找到，则会报错。

### 3.with语句延长作用域链
  `with`的作用是将写在其中的代码的作用域设置到一个特定的对象中。在函数中使用相当于将传入的变量对象添加到当前作用域链的前端。
 ```javascript
   var a = "aa";
   function outer() {
       var b = "bb";
       function inner() {
           var c = "cc";
           //可以访问a,b,c
           with(location){
               //将location对象传入with语句，将location的变量对象加到当前作用域链的前端
               //href变量来自location，c是沿着作用域链在inner变量对象中找到
               var url = href + c;
               console.log(url)
           }
       }
       //可以访问a,b,不能访问c
       inner();
   }
  //可以访问a,不能访问b,c
  outer();
  ```

![with语句延长后的作用域链](http://upload-images.jianshu.io/upload_images/4472988-2482b1ab260b5f17.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 4.JS中的块级作用域问题
先看一段代码
 ```javascript
    if('a' in window) {
        var a = 10;
    }
    alert(a);//10
     ```
按照一般（C,java）的思维，`a`变量应该是`if()`语句中的一个局部变量，执行完后就应该销毁，所以后面的`alert`语句是不能访问到`a`变量的。但是在if执行完后，仍可以访问到`a`变量。
这里有两个点：1.没有块级作用域 2.变量声明提升
js中没有块级作用域，所有用`var`声明的变量，都会被放到最近的执行环境的变量对象中，此时的`a`就被挂在了`window`对象上（如果变量没有使用`var`声明，则无论最近的执行环境是什么，都会被添加到全局对象中）。
较为常用的例子是：
```javascript
   for(var i=0; i < 10; i++){
        doSomething(i)
    }
    alert(i);//10
     ```
for循环中的计数变量在使用后不会被销毁，所以当要在一个函数中使用多个for循环时，不能像C，JAVA中一样，都使用一个循环变量。
> 在es6中，可以使用`let`替代`var` 做for循环中的循环变量