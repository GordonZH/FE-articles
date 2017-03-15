###  &&  与操作符  `num1 && num2`
1. 当两个操作数为布尔值时，表达式的值：
 * 有`false`为`false`
 * 两个均为`true`为`true`
2. 当有一个值不为布尔值时
 *  当第一个操作数不是布尔值，但是其计算值为`true`时，返回第二个操作数
 * 当第一个操作数为`false`时，不会对第二个操作数进行计算（短路操作）
  ```javascript
//如果$不存在,则把jQuery赋值给window.$；如果$存在，则不执行后面的表达式
$ === undefined && (window.$ =  jQuery);  
//相当于
($ === undefined) && (window.$ =  jQuery);  
```
 * 当其中一个值是 `NaN`、`Null`、`Undifined`时，则返回这三个值
***
### || 或操作符 `num1 || num2`
1. 同`&&`一样，当两个操作数为布尔值时，表达式的值为
  * 有`true`为`true`
  * 两个均为`false`为`false`
2. 当有一个值不为布尔值时
  * 当第一位操作数的计算值为`false`时，返回第二个操作数 。一个广泛的应用：使用`|| `操作符为变量设置默认值，当`realValue`为空或者`undifined`时，其计算值为`false`，所以`a`值为`defaultValue`

    ```javascript
     var a = realValue || defaultValue
     ```
  * 当第一位为`true`时，不再计算第二个值，直接返回第一个值（短路操作）
  * 两个值均为对象时，返回第一个
  * 两个值均为`NaN`、`Null`、`Undifined`时，返回这三个值
***
### ! 非操作符 !num
```javascript
  !false      //true
  !"string"   //false
  !0          //true
  !NaN        //true
  !""         //true
  !1234       //false
```
***
### +、-、* 、/  加减乘除
* 与`NaN`进行计算时，所有值均为`NaN`
* 若果有一个操作数不是数值，则使用其经`Number()`转换后的值进行计算
  ```javascript
     Number('')          //0
     Number('string')    //NaN
     Number(true)        //1
     Number(false)       //0
     Number(null)        //0
     Number(undefined )  //NaN
     Number(NaN)         //NaN
  ```
* `+`操作符
  * 如果两个操作数都是数值，则执行求和
  * 如果有一个是字符串，则执行字符串拼接
  * `+`是一个独立操作符，每一个`+`都是独立进行的，依次进行计算
  ```javascript
  var num1 = 5;
  var num2 = 10;
  var msg1 = num1 + num2 +" is the sum";      //15 is the sum
  var msg2 = "the sum is "+ num1 + num2;      //the sum is 510
  var msg3 = "the sum is "+ (num1 + num2);    //the sum is 15
  ```
***
### ==、!=、===、!== 相等/不相等/全等/不全等
* == 、!= 操作符会在进行比较前转换数据类型
  ```javascript
 null == undefined   //true
 "NaN" == NaN        //false
 5 == NaN            //false
 NaN == NaN          //false NaN不等于自身
 NaN != NaN          //true
  ```
* === 、!== 全等、不全等操作符不会转换数据类型
  ```javascript
 "55" == 55   //true
 "55" === 55  //false
```

### >、< 比较操作
* 数值：比较大小
* 字符串：按位比较字符在字符编码中的位置
* 字符串与数值：转换为数值后进行比较
```javascript
"a" < 3     //false "a" 转换为NaN 再与3进行比较，值为fasle
"23" < 3    //true 字符串"23"转换为数值23进行比较，值为fasle
```