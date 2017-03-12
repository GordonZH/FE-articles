#数组方法汇总
### 数组拼接为字符串
```javascript
array.join('|');
```
### 模拟栈和队列
```javascript
//将任意数量的参数添加到数组末尾，并返回当前数组长度
array.push('real');

//移除数组的最后一项，并将最后一项返回
array.pop();

//移除数组的第一项，并将第一项返回
array.shift();

//将任意数量的参数添加到数组开头，并返回当前数组长度
array.unshift('really');
```
### 排序
```javascript
//反序数组(不是排序)
array.reverse();

//排序数组,可以传入一个排序方法，返回经过排序的数组
array.sort(compareFunction);
```
### 数组操作
```javascript
//链接数组，根据当前数组创建一个新数组，并将参数添加到这个新数组中并返回结果数组
array.concat("");

//截取数组，将当前数组的start、end-1位置截取并返回 slice=裁切，切下
array.slice(startPos,endPos);

//粘接数组
//删除：2个参数，起始位置、删除的项数
array.splice(startPos,num);
//插入：3个参数，起始位置、删除数量、任意数量的插入项
array.splice(startPos,0,insertItems);
//替换：3个参数，起始位置、输出数量、任意数量插入项
array.splice(startPos,num,insertItems);
```

### 查找item在数组中的位置
```javascript
//正序,找到返回位置，未找到返回-1
array.indexOf(item);
//逆序,找到返回位置，未找到返回-1
array.lastIndexOf(item);
```

### 迭代方法
```javascript
//参数：yourFunction(item,index,array),(第二个参数为this对象，可选)
//对每一项运行给定函数，全部为true则返回true
array.every(yourFunction,[scope]);

//对每一项运行给定函数，返回该函数返回值为true的项组成的数组
array.filter(yourFunction,[scope]);

//对每一项运行给定函数，无返回值
array.forEach(yourFunction,[scope]);

//对每一项运行给定函数，返回每次函数执行的结果组成的数组，返回的是运行结果的数组
array.map(yourFunction,[scope]);

//对每一项运行给定函数，只要有一项返回true，则返回true 与every()对应
array.some(yourFunction,[scope]);
```
### 归并方法
```javascript
//yourFunction(prev,current,index,array)
//正序遍历
array.reduce(yourFunction);
//逆序遍历
array.reduceRight(yourFunction);
```