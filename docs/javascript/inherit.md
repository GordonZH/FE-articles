### 1.原型链
javascript中没有类的概念，需要利用**原型链**来模拟。
我们知道，构造函数、原型对象、实例之间有如下关系：
   1. 构造函数会自动获得一个prototype属性，指向其原型对象
   2. 原型对象在函数创建时自动获得，其中有一个constructor属性，指向其构造函数
   3. 实例中有一个`__proto__`指针，指向该类型的原型对象
  
如下图所示：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4472988-ffd94a03c4091f42.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

原型链实现继承，就是将一个类型的原型（子类型）指向另一个类型（父类型）的一个实例。
我们知道，在寻找标识符（也就是变量或者函数）时，会先寻找实例属性，找到了即返回，如果没有找到，即通过`__proto__ `指针去查找原型属性。
当用**父类型的实例**作为**子类型的原型对象**后，就可以找到父类型中的全部属性和方法，也就实现了继承，同时也切断了子类型构造函数与本来的原型对象的联系。如下图所示：

![原型链继承过程](http://upload-images.jianshu.io/upload_images/4472988-a169548ff296ac44.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```javascript
    //父类型构造函数
    function SuperType() {
        this.supername = "super"
    }
    //父类型原型对象
    SuperType.prototype.getSuperName = function () {
        console.log(this.supername)
    };
    //子类型构造函数
    function SubType() {
        this.subname = "sub";
    }
    //改变子类型原型属性的指向，图中1标记所示。
    //同时切断了子构造函数与它在创建时自动获得的原型对象的联系，如图中红叉所示。
    SubType.prototype = new SuperType();
    //添加子类型的方法
    SubType.prototype.getSubName = function () {
       console.log(this.supername)
    };
    var subInstance = new SubType();
    subInstance.getSuperName();//输出super,获得了父类型的方法
```
有一点需要注意，所有的实例都是Object类型的实例，这是因为函数创建时直接获得的原型对象的`__proto__`指针都是指向的`Object.prototype`。

![加入Object.prototype的原型链](http://upload-images.jianshu.io/upload_images/4472988-5e63107bd01f32f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

而此时，子类型的实例又可以作为另一个类型（孙子）的原型对象，孙子类型的实例又可以作为曾孙类型的原型对象，由此形成一个链条。

**纯原型链继承的问题**
一个类型（父）的实例作为另一个类型（子）的原型对象，那么父类型的**实例属性**就变成了子类型实例的**原型属性**，如果这属性恰好是一个引用类型，那么这个属性所对应的引用就会被所有子类实例所共享。一个子类型实例对这个属性的任何操作都会影响其他所有的子类型实例。

### 2.借用构造函数实现继承
暗中观察一个构造函数：
```javascript
  function Person(name,age,job) {
       this.name = name;
       this.age = age;
       this.job = job;
       this.sayName = function () {
           alert(this.name);
       };
   }
  var person1 = new Person();
```
 可以看到，构造函数中的属性都定义在`this`上。回忆一下使用`new`操作符创建一个实例时的步骤：新建一个对象；然后让`this`指向这个对象，执行构造函数；构造函数执行完后，这个对象上就有创建好的各种属性；最后返回这个对象。

同样的道理，我们可以在子类型构造函数中使用`call()`、`apply()`方法来改变构造方法的作用域，使其`this`指向子类构造函数的作用域，达到在子类型中创建父类型中定义的各种属性的目的。
```javascript
  //示例代码来自《JavaScript高级程序设计》
  function SuperType(name) {
       this.name = name;
   }
   function SubType() {
       SuperType.call(this,"gordenz");
       this.age = "29";
   }
   var instance = new SubType();
   console.log(instance.name); //gordenz，获得了定义在父类中的属性
   console.log(instance.age); //29
```
`call()`，`apply()`的区别：第一个参数都是函数执行的作用域 ；`apply()`的第二个参数接收一个参数数组，而`call()`方法则需要将接收的参数一一列在后面（即除了第一个参数外，可能有很多个参数）

**全部使用构造函数实现继承的问题**
1.无法实现同一函数的复用，每个实例都会创建一个副本；
2.子类型无法继承到定义在父类型的原型对象中的属性和方法。

### 3.组合继承
组合继承可以类比创建对象时的构造函数模式与原型模式。
即，使用**原型链继承原型属性和方法**，**使用构造函数继承实例属性**。
```javascript
  //示例代码来自《JavaScript高级程序设计》
  function SuperType(name) {
       this.name = name;
       this.colors = ["red","blue","green"]
   }
   SuperType.prototype.sayName = function () {
       alert(this.name)
   };
   function SubType(name,age) {

       //继承属性
       SuperType.call(this,name);

       this.age = age;
   }

   //继承方法
   SubType.prototype = new SuperType();

   //为作为原型对象的父类型实例添加constructor属性
   SubType.prototype.constructor = SubType;
  
   //添加属于子类的原型方法
   SubType.prototype.sayAge = function () {
       alert(this.age);
   };

   var instance1 = new SubType("gordenz",24);
   instance1.colors.push('black');
   console.log(instance1.colors); //"red,blue,green,black"
   instance1.sayName(); //gorden
   instance1.sayAge(); //24

   var instance2 = new SubType("huan",23);
   console.log(instance2.colors); //"red,blue,green"
   instance2.sayName(); //huan
   instance2.sayAge(); //23 

   //父类的原型方法都得到了共享，而引用类型的属性也不会在各实例间相互影响
```
这样就完成了一个较好的继承，两个实例都拥有自己的属性，同时又公用了方法。
相对之前的原型链继承，此处多了一个步骤，就是将父类型的实例作为子类型的原型对象后，为其手工添加了一个constructor属性，使其指向了我们逻辑上的构造函数。如图：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4472988-5cc074a1c93f5e5c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如图中2所示，我们手工为子类型的原型对象（它也是一个父类型的实例）指定了constructor属性，让他正确的指向子类型的构造函数。
如果我们没有手动指定这个constructor属性，那么此时他其实是指向的父类的构造函数。为什么呢？因为当前子类原型对象是一个父类型的实例，这个constructor属性继承自父类原型对象，所以他的指向是父类构造函数，这与我们的逻辑不符。
同时图中也展示了原型链的终点：null。