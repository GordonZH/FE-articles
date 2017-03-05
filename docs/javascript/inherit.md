### 1.ԭ����
javascript��û����ĸ����Ҫ����**ԭ����**��ģ�⡣
����֪�������캯����ԭ�Ͷ���ʵ��֮�������¹�ϵ��
   1. ���캯�����Զ����һ��prototype���ԣ�ָ����ԭ�Ͷ���
   2. ԭ�Ͷ����ں�������ʱ�Զ���ã�������һ��constructor���ԣ�ָ���乹�캯��
   3. ʵ������һ��`__proto__`ָ�룬ָ������͵�ԭ�Ͷ���
  
����ͼ��ʾ��

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4472988-ffd94a03c4091f42.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

ԭ����ʵ�ּ̳У����ǽ�һ�����͵�ԭ�ͣ������ͣ�ָ����һ�����ͣ������ͣ���һ��ʵ����
����֪������Ѱ�ұ�ʶ����Ҳ���Ǳ������ߺ�����ʱ������Ѱ��ʵ�����ԣ��ҵ��˼����أ����û���ҵ�����ͨ��`__proto__ `ָ��ȥ����ԭ�����ԡ�
����**�����͵�ʵ��**��Ϊ**�����͵�ԭ�Ͷ���**�󣬾Ϳ����ҵ��������е�ȫ�����Ժͷ�����Ҳ��ʵ���˼̳У�ͬʱҲ�ж��������͹��캯���뱾����ԭ�Ͷ������ϵ������ͼ��ʾ��

![ԭ�����̳й���](http://upload-images.jianshu.io/upload_images/4472988-a169548ff296ac44.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```javascript
    //�����͹��캯��
    function SuperType() {
        this.supername = "super"
    }
    //������ԭ�Ͷ���
    SuperType.prototype.getSuperName = function () {
        console.log(this.supername)
    };
    //�����͹��캯��
    function SubType() {
        this.subname = "sub";
    }
    //�ı�������ԭ�����Ե�ָ��ͼ��1�����ʾ��
    //ͬʱ�ж����ӹ��캯�������ڴ���ʱ�Զ���õ�ԭ�Ͷ������ϵ����ͼ�к����ʾ��
    SubType.prototype = new SuperType();
    //��������͵ķ���
    SubType.prototype.getSubName = function () {
       console.log(this.supername)
    };
    var subInstance = new SubType();
    subInstance.getSuperName();//���super,����˸����͵ķ���
```
��һ����Ҫע�⣬���е�ʵ������Object���͵�ʵ����������Ϊ��������ʱֱ�ӻ�õ�ԭ�Ͷ����`__proto__`ָ�붼��ָ���`Object.prototype`��

![����Object.prototype��ԭ����](http://upload-images.jianshu.io/upload_images/4472988-5e63107bd01f32f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

����ʱ�������͵�ʵ���ֿ�����Ϊ��һ�����ͣ����ӣ���ԭ�Ͷ����������͵�ʵ���ֿ�����Ϊ�������͵�ԭ�Ͷ����ɴ��γ�һ��������

**��ԭ�����̳е�����**
һ�����ͣ�������ʵ����Ϊ��һ�����ͣ��ӣ���ԭ�Ͷ�����ô�����͵�**ʵ������**�ͱ����������ʵ����**ԭ������**�����������ǡ����һ���������ͣ���ô�����������Ӧ�����þͻᱻ��������ʵ��������һ��������ʵ����������Ե��κβ�������Ӱ���������е�������ʵ����

### 2.���ù��캯��ʵ�ּ̳�
���й۲�һ�����캯����
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
 ���Կ��������캯���е����Զ�������`this`�ϡ�����һ��ʹ��`new`����������һ��ʵ��ʱ�Ĳ��裺�½�һ������Ȼ����`this`ָ���������ִ�й��캯�������캯��ִ�������������Ͼ��д����õĸ������ԣ���󷵻��������

ͬ���ĵ������ǿ����������͹��캯����ʹ��`call()`��`apply()`�������ı乹�췽����������ʹ��`this`ָ�����๹�캯���������򣬴ﵽ���������д����������ж���ĸ������Ե�Ŀ�ġ�
```javascript
  //ʾ���������ԡ�JavaScript�߼�������ơ�
  function SuperType(name) {
       this.name = name;
   }
   function SubType() {
       SuperType.call(this,"gordenz");
       this.age = "29";
   }
   var instance = new SubType();
   console.log(instance.name); //gordenz������˶����ڸ����е�����
   console.log(instance.age); //29
```
`call()`��`apply()`�����𣺵�һ���������Ǻ���ִ�е������� ��`apply()`�ĵڶ�����������һ���������飬��`call()`��������Ҫ�����յĲ���һһ���ں��棨�����˵�һ�������⣬�����кܶ��������

**ȫ��ʹ�ù��캯��ʵ�ּ̳е�����**
1.�޷�ʵ��ͬһ�����ĸ��ã�ÿ��ʵ�����ᴴ��һ��������
2.�������޷��̳е������ڸ����͵�ԭ�Ͷ����е����Ժͷ�����

### 3.��ϼ̳�
��ϼ̳п�����ȴ�������ʱ�Ĺ��캯��ģʽ��ԭ��ģʽ��
����ʹ��**ԭ�����̳�ԭ�����Ժͷ���**��**ʹ�ù��캯���̳�ʵ������**��
```javascript
  //ʾ���������ԡ�JavaScript�߼�������ơ�
  function SuperType(name) {
       this.name = name;
       this.colors = ["red","blue","green"]
   }
   SuperType.prototype.sayName = function () {
       alert(this.name)
   };
   function SubType(name,age) {

       //�̳�����
       SuperType.call(this,name);

       this.age = age;
   }

   //�̳з���
   SubType.prototype = new SuperType();

   //Ϊ��Ϊԭ�Ͷ���ĸ�����ʵ�����constructor����
   SubType.prototype.constructor = SubType;
  
   //������������ԭ�ͷ���
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

   //�����ԭ�ͷ������õ��˹������������͵�����Ҳ�����ڸ�ʵ�����໥Ӱ��
```
�����������һ���Ϻõļ̳У�����ʵ����ӵ���Լ������ԣ�ͬʱ�ֹ����˷�����
���֮ǰ��ԭ�����̳У��˴�����һ�����裬���ǽ������͵�ʵ����Ϊ�����͵�ԭ�Ͷ����Ϊ���ֹ������һ��constructor���ԣ�ʹ��ָ���������߼��ϵĹ��캯������ͼ��

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4472988-5cc074a1c93f5e5c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

��ͼ��2��ʾ�������ֹ�Ϊ�����͵�ԭ�Ͷ�����Ҳ��һ�������͵�ʵ����ָ����constructor���ԣ�������ȷ��ָ�������͵Ĺ��캯����
�������û���ֶ�ָ�����constructor���ԣ���ô��ʱ����ʵ��ָ��ĸ���Ĺ��캯����Ϊʲô�أ���Ϊ��ǰ����ԭ�Ͷ�����һ�������͵�ʵ�������constructor���Լ̳��Ը���ԭ�Ͷ�����������ָ���Ǹ��๹�캯�����������ǵ��߼�������
ͬʱͼ��Ҳչʾ��ԭ�������յ㣺null��