###  0.注
本文代码来自于《JavaScript高级程序设计》一书，非原创，算是小弟读书笔记。而且我也不认为自己写的实例代码能比Zakas的更清晰易懂。主要记录下自己认为干货的部分以及加入自己的理解。文中示意图等均为原创。
### 1.对象？
  ECMA的定义：无序属性的集合，其属性可以包含基本值、对象或者函数。
  我自己的理解，对象就是一个包含了若干键值对的无序列表。
### 2.创建对象的方式
 * 工厂模式（其实并没什么卵用）
  使用一个函数封装创建对象的全过程：创建一个临时对象、为这个对象添加属性、最后将这个对象返回
```javascript
  //代码来自《JavaScript高级程序设计》
   function createPerson(name,age,job) {
       var o = new Object();
       o.name = name;
       o.age = age;
       o.job = job;
       o.sayName = function () {
           alert(this.name);
       };
       return o;
   }
   var person1 = createPerson('gordenZ','24','front-end engineer');
```

* 构造函数模式（问题也大，不会直接用）
  ```javascript
   //代码来自《JavaScript高级程序设计》
   function Person(name,age,job) {
       this.name = name;
       this.age = age;
       this.job = job;
       this.sayName = function () {
           alert(this.name);
       };
   }
   var person1 = new Person('gordenZ','24','front-end engineer');
  ```
这个例子中，创建了一个构造函数`Person`，并使用`new`操作符实例化了一个对象`person1`。
**为何要使用new 操作符？**
想像一下不使用new的情况：
```javascript
var person1 = Person('gordenZ','24','front-end engineer');
 console.log(name);//gordenZ
```
如果不使用new操作符，相当于在当前环境下直接执行`Person`这个函数（构造函数也是函数，仅仅是第一个字母大写了）。这里的执行环境是`window`，那么`Person`中的`this`就指向了全局的`window`对象，所以`name`、`age`、`job`、`sayName`等属性都被创建在了全局对象上，可以直接被访问到。
**new操作符干的事情**：
  1.创建一个新对象
  2.将构造函数的this指向这个对象（这样就不再是创建到`window`上了）
  3.执行构造函数
  4.返回新对象
*~~其实跟工厂模式没有什么区别嘛~~*
**构造函数模式问题：**
    构造函数模式创建的每个对象的实例都是**完全**独立的，意味着每个实例中的属性都是不同的，这对于值是基本数据类型的属性来说还好，但是对于值是一个实现同一功能的函数的属性来说就显得冗余了，每一个实例都会有一个自己的函数，有多少个实例就会创建多少个相同的函数（不要忘了，每个函数都是一个对象）。

* 原型模式
js中每一个函数都有一个`prototype`（即原型）属性，指向一个对象，其功能是保存用这个函数类型创建的实例所共享的属性和方法。所以我们可以把需要在各个实例上共享的属性和方法放到这个类型的`prototype`中，即可只创建一次而被各实例所共享。
```javascript
function Person() {
}
Person.prototype.name = "gordenZ";
Person.prototype.age = "24";
Person.prototype.job = "front-end engineer";
Person.prototype.sayName = function () {
    alert(this.name);
};
 var person1 = new Person();
 var person2 = new Person();

  console.log(person1.sayName === person2.sayName); //true
```
其中，`Person`是**构造函数**、`Person.prototype`是**原型对象**、`person1`和`person2`是`Person`的**实例**，这三者的关系如下图：
![构造函数、原型对象、实例之间的关系](http://upload-images.jianshu.io/upload_images/4472988-4b167932d711a0cd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
每一个函数（也就包括了**构造函数**）在创建都会获得一个`prototype`属性，指向了这个函数的原型对象。所有的**原型对象**都默认有一个`constructor`·属性，指向构造函数（`Person.prototype.constructor == Person //true`）。在根据**构造函数**创建的实例中，有一个`__proto__`指针，指向了该类型的**原型对象**。
可以看到，现在的`name`、`age`、`job`、`sayName`被所有实例共享了。
  1.每个实例又可以添加自己独立的属性 
```javascript
person1.sex = "man"
```
  2.可以在实例中添加与原型中属性相同的属性，这样会屏蔽掉原型中的同名属性。寻找属性的方式是，首先在实例本身找，找到了就返回，如果没有找到，则到`__proto__`指针指向的原型对象上寻找。
 ```javascript
person1.name = "grey"
console.log(person1.name) //grey 
 ```
  3.其他实例若没有添加，则不受影响
 ```javascript
console.log(person2.name) //gordenZ。
 ```
  4.判断是原型属性还是实例属性：`hasOwnProperty()`
```javascript
console.log(person1.hasOwnProperty('name'));//true
console.log(person2.hasOwnProperty('name'));//false
```
  5.判断对象是否是一个实例的原型：`isPrototypeOf()`
```javascript
console.log(Person.prototype.isPrototypeOf(person1));//true
console.log(Person.prototype.isPrototypeOf(person1));//true
```
6.获取一个实例的原型：` Object.getPrototypeOf()`
```javascript
console.log(Object.getPrototypeOf(person1) == Person);//true
```
**原型模式的问题**
高度共享带来问题，在共享的属性中，如果是数据属性，可以用在实例上添加同名属性来屏蔽，但如果某个属性是一个引用类型，那么在任何一个实例上对其进行修改，都会影响到所有实例。


* 组合使用构造函数模式和原型模式
  1.将每个实例自有的属性写在构造函数中，即实例属性
  2.将所有实例共享的方法写在原型对象中，即原型属性
  3.若直接使用对象字面量替换原型对象，需要将`constructor`属性补全
```javascript
function Person(name,age,job) {
        this.name = name;
        this.age = age;
        this.job = job;
        this.friends = ["aa","bb"];
}
Person.prototype = {
    //补全constructor属性
    constructor:Person,
    sayName:function () {
       alert(this.name)
    }
};
var person1 = new Person();
var person2 = new Person();
```