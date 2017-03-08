##### 1.事件捕获与事件冒泡
**事件捕获** ：事件从顶层元素(document)传播到具体发生事件的元素（Netscape）
**事件冒泡** ：事件从具体元素传播到顶层元素 （IE）
![事件流模型](http://upload-images.jianshu.io/upload_images/4472988-4e9b81ae298c61d0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 2.事件处理
1. HTML 事件处理
```html
<div id="clickItem" onclick="clickMe(this)"></div>
<script>
    function clickMe() {
        alert(arguments[0].id+" clicked!");
    }
</script>
```
2. DOM0 事件处理
```html
<div id="clickItem"></div>
<script>
    var clickDiv = document.getElementById("clickItem");
    clickDiv.onclick = function(){
        //事件处理程序中this指向发生事件的对象
        alert(this.id+" clicked!");
    }
</script>
```
3. DOM2 事件处理
```html
<div id="clickItem"></div>
<script>
    var clickDiv = document.getElementById("clickItem");
    function handler(){
        alert(this.id+" clicked!");
    }
    function handler2(){
        alert(this.id+" clicked again!");
    }

    //第一个参数：事件类型
    //第二个参数：处理程序
    //第三个参数：true，捕获阶段处理/false，冒泡阶段处理
    clickDiv.addEventListener('click',handler,false);

    //可以添加多个处理程序，按照添加的顺序执行
    clickDiv.addEventListener('click',handler2,false);

    //dom2的事件处理程序只能这样移除
    clickDiv.removeEventListener('click',handler,false)
</script>
```
4. IE事件处理
```html
<div id="clickItem"></div>
<script>
    var clickDiv = document.getElementById("clickItem");
    function handler(){
        alert(this.id+" clicked!");
    }
    function handler2(){
        alert(this.id+" clicked again!");
    }
    //第一个参数：事件类型，需要加上"on"前缀
    //第二个参数：处理程序
    clickDiv.attachEvent('onclick',handler);
    //可以添加多个处理程序，与DOM不同，IE按照添加顺序的反序执行，这里先输出again
    clickDiv.attachEvent('onclick',handler2);
    //IE事件处理程序移除
    clickDiv.detachEvent('onclick',handler)
</script>
```
##### 3.事件对象
1 事件对象：在处理程序（handler）中传入，生命周期为处理期间
```javascript
event.currentTarget     //处理事件的目标
event.target            //事件的目标DOM
event.type              //事件类型
event.eventPhase        //事件阶段
event.preventDefault()  //取消事件的默认行为
event.stopImmediatePropagation() //立即取消任何捕获或者冒泡行为
event.stopPropagation() //取消冒泡，不再传播事件
``` 
2 .IE 事件对象：
```javascript
event.srcElement     //事件的目标IE
event.cancelBubble() //取消冒泡IE
```
##### 4.通用的事件监听对象
```javascript

    var EventUtil = {
        //添加监听
        addHandler:function(ele,handler,type){
            if(ele.addEventListener){
                ele.addEventListener(type,handler,false);
            } else if(ele.attachEvent){
                ele.attachEvent("on"+type,handler);
            } else{
                ele["on"+type] = handler;
            }
        },
        //移除监听
        removeHandler:function(ele,handler,type){
            if(ele.removeEventListener){
                ele.removeEventListener(type,handler,false);
            }else if(ele.detachEvent){
                ele.detachEvent("on"+type,handler);
            }else{
                ele["on"+type] = null;
            }
        },
        //获取事件对象
        getEvent:function(event){
            return event ? event : window.event;
        },
        //获取事件目标
        getTarget:function(event){
            return event.target || event.srcElement;
        },
        //取消默认行为
        preventDefault:function(event){
            if(event.preventDefault){
                event.preventDefault();
            }else{
                event.returnValue = false;
            }
        },
        //阻止冒泡
        stopPropagation:function(event){
            if(event.stopPropagation){
                event.stopPropagation();
            }else{
                event.cancelBubble = true;
            }
        }
    }
```