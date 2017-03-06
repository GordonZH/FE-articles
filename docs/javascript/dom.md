### 1.Node（节点）级别：
```javascript
// someNode 表示一个节点的引用

//全部子节点，一个nodeList对象
someNode.childNodes

//父节点
someNode.parentNode

//上一个节点
someNode.previousSibling

//下一个节点
someNode.nextSibling

//第一个子节点
someNode.firstChild
someNode.firstChild == someNode.childNodes[0] //true

//最后一个子节点
someNode.lastChild
someNode.lastChild == someNode.childNodes[someNode.childNodes.length-1]//true

//是否有子节点
someNode.hasChildNodes() //有一个或多个子节点时 true

//新增节点到childNodes末尾，并返回新增的节点
var returnNode = someNode.appendChild(newNode)
returnNode == someNode.lastChild   //true

//对已经在childNodes中的节点使用appendChild()，相当于移动到最后
var returnNode = someNode.appendChild(someNode.firstChild)
returnNode == someNode.lastChild //true

//插入节点到参照节点之前，返回被插入的节点
someNode.insertBefore(insertNode,referenceNode)

//替换第二个节点，返回并从dom中移除它
someNode.replaceChild(insertNode,replaceNode)

//移除节点，仍归文档所有但在文档中没有位置
someNode.removeChild(toBeRemoveNode)

//克隆节点
someNode.cloneNode(true) //深复制，会同时复制它所包含的子节点
someNode.cloneNode(false) //浅复制，仅复制当前节点
```
### 2.document（文档）级别：
```javascript
//元素获取---------------------------------------------------------------------------
//id
var ele = document.getElementById(eleId)

//tagName(获得一个类似数组的动态HTMLCollection对象,)
var eleCollecton = document.getElementsByTagName(tagName);
eleCollecton.namedItem(eleName); //用该方法获取被命名的元素
eleCollecton[eleName]; //用方括号+命名获取元素

//name（多用于表单）
document.getElementsByName(eleName);

//class获取
ele.getElementsByClassName(className);

//css选择器
document.querySelector(CssSelector)     //返回匹配的第一个元素
document.querySelectorAll(CssSelector)  //返回匹配元素的Nodelist

//快捷方式---------------------------------------------------------------------------
//指向<html>
var html = document.documentElement;
html == document.childNodes[0] //true
html == document.firstChild    //true

//指向<body>
var body = document.body;

//文档标题
var pageTitle = document.title;
pageTitle = "new title";

//网页请求相关
document.URL        //完整url
document.domain     //域名
document.referrer   //来源

//其他
document.anchors //所有带name的<a>元素
document.forms   //所有的<form>元素
document.images  //所有的<img>元素
document.links   //所有带href的<a>元素
```
### 3.element（元素）级别：
```javascript  
//元素属性，可直接赋值修改
var ele = document.getElementById(eleId);
ele.id          // id
ele.className   // class
ele.title       // 附加说明
ele.lang        // 语言
ele.dir         // 文字方向

//获取特性 attributeName = id、class、title、lang、dir及data-xx
ele.getAttribute(attributeName) 
//使用getAttribute()与直接使用元素属性的区别：获取样式和事件处理时
ele.getAttribute('style')  //返回css字符串
ele.style                  //返回一个style对象 
ele.getAttribute('onclick')//返回函数字符串
ele.onclick                //返回一个js函数  

//设置特性
ele.setAttribute(attributeName,value)

//attributes属性 元素所有特性的动态集合(一个NamedNodeMap对象)
//获取或设置特性的值    
ele.attributes.getNamedItem(attributeName).nodeValue 
//删除给定名称的特性，并返回特性节点
ele.attributes.removeNamedItem(attributeName).nodeValue 
//新增一个特性节点
ele.attributes.setNamedItem(attributeName)
//返回pos位置的节点             
ele.attributes.item(pos).nodeValue 

//创建元素
document.createElement(tagName);
document.createElement('<div id=\"myNewDiv\"></div>'); //IE特有

//文档片段 一段不在文档中的元素集合，多用于作为临时容器
document.createDocumentFragment()

//元素遍历    
ele.childElementCount       //返回子元素的个数
ele.firstElementChild       //第一个子元素，firstChild的元素版
ele.lastElementChild        //最后一个子元素，lastChild的元素版
ele.previousElementSibling  //前一个兄弟元素 previousSibling元素版
ele.nextElementSibling      //后一个兄弟元素 nextSibling 元素版

//获取类名
ele.className //包含所有类名的字符串，修改单个类名需字符串转数组解析
ele.classList //class集合属性
ele.classList.add(className)        //添加一个类
ele.classList.contains(className)   //是否包含一个类
ele.classList.remove(className)     //移除一个类
ele.classList.toggle(className)     //有，移除/没有，添加

//修改样式
ele.style.[驼峰写法的css属性] //可以同时用于设置或者获取
ele.style.width = "100px" 
ele.style.height = "50px"
ele.style.backgroundColor = "red"
ele.style.borderBottom = "1px solid black"

//获得计算样式
ele.currentStyle                                            //ie
document.defaultView.getComputedStyle(ele,[null][":after"]) //其他

//插入/修改
ele.innerHTML  //ele内部的html元素字符串
ele.outerHTML  //包含ele本身以及其子元素的字符串
                     
```