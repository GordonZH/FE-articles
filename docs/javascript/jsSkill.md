##### 1.比较两个对象是否相等
```javascript
function isEqual(a, b) {
    var aProps = Object.getOwnPropertyNames(a),
        bProps = Object.getOwnPropertyNames(b);
    //1.先比较属性的个数是否相等
    if (aProps.length != bProps.length) {
        return false;
    }
    //2.属性个数相等，再比较每个属性的值是否相等
    for (var i = 0; i < aProps.length; i++) {
        var propName = aProps[i];
        
        if (a[propName] !== b[propName]) {
            return false;
        }
    }
    return true;
}
```

##### 2.bind、call、apply
*  `bind`:Function.prototype.bind(context)，用于在**不调用函数**的情况下，将this**固定**在context上，并返回一个有新的`this`值的函数。
用法：
```javascript
var moneyGorden = {
        name:"gorden",
        money:1000,
        addMoney:function (x) {
            this.money += x;
            console.log(this.name+" has "+this.money) ;
        }
    };
    var moneyJack = {
        name:"jack",
        money:100
    };
    //直接调用
    moneyGorden.addMoney(100);//gorden has 1100
    
    //使用bind
    var newFuntionUseBind = moneyGorden.addMoney.bind(moneyJack);
    newFuntionUseBind(100);//jack has 200
    
    //可以看到bind返回的是一个新函数，原函数不受影响
    moneyGorden.addMoney(100);//gorden has 1200
```
*  `call`、`apply`: 与`bind`不同的是，这两个方法直接修改函数**运行**时的`this`值，并且可以给运行的函数传递参数，以参数列表的形式跟在context后面。他们两的不同在于参数传递，`call`跟一个参数列表，将参数全部列出来，而`apply`跟一个参数数组。可以记忆为 apply、array都以"a"开头，所以apply跟一个array。

* 重写bind方法
```javascript
Function.prototype.bind = Function.prototype.bind || function(context){
      var self = this;
      return function(){
      return self.apply(context, arguments);
    };
}
```
##### 3. this值的总结 [原文在这里](http://www.thatjsdude.com/interview/js2.html)
 1. 在全局作用域或者一个匿名函数中，`this`指向`window`对象
 2. 严格模式下，在一个立即执行函数(IIFE)中`this`是`undifined`，需要将`window`作为参数传入。
 3. 当在一个对象的环境（context）中执行函数市，`this`的值就指向这个对象
 4. 在`setTimeOut`中，`this`指向全局对象
 5. 使用构造函数创建一个对象时（使用`new`关键字），this指向这个新创建的对象
 6. 使用`bind`、`cal`l、`apply`指定this的值
 7. 在dom事件处理的handler中，`this`指向触发事件的元素

##### 4. 获取url中?后面的参数
```javascriot
function GetRequest() {
    var url = location.search; 
    var theRequest = new Object();
    if (url.indexOf("?") != -1) {
        var str = url.substr(1);
        strs = str.split("&");
        for(var i = 0; i < strs.length; i ++) {
            theRequest[strs[i].split("=")[0]]=unescape(strs[i].split("=")[1]);
        }
    }
    return theRequest;
}
```

##### 5.js强制横屏 [参考](https://www.jianshu.com/p/9c3264f4a405)
```html
<div id="fit-container">

    <div id="test">
        <span>hello</span>
    </div>

</div>
```
```css
*{
    margin: 0;
    padding: 0;
}
html,body{
    width : 100% ;
    height : 100% ;
}
#fit-container{
    position: fixed;
    top: 0;
    left: 0;
    background-color: white;
}
#test{
    height: 100%;
    width: 100%;
    background-color: #666666;
    display: flex;
    justify-content: center;
    align-items: center;
}
```

```javascript
(function () {
    function fitScreen() {
        var width = document.documentElement.clientWidth;
        var height =  document.documentElement.clientHeight;
        var $fc =  $('#fit-container');
        if( width > height ){
            //横屏
            $fc.width(width).height(height).css({
                'top': 0,
                'left': 0,
                'transform': 'none',
                'transform-origin': '50% 50%'
            });
        }
        else{
            //竖屏
            $fc.width(height).height(width).css({
                'top': (height-width)/2,
                'left': 0-(height-width)/2,
                'transform': 'rotate(90deg)',
                'transform-origin': '50% 50%'
            });
        }
    }
    var evt = "onorientationchange" in window ? "orientationchange" : "resize";
    fitScreen();
    window.addEventListener(evt, function() {
        fitScreen()
    }, false);
})();
```