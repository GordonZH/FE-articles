#���鷽������
### ����ƴ��Ϊ�ַ���
```javascript
array.join('|');
```
### ģ��ջ�Ͷ���
```javascript
//�����������Ĳ�����ӵ�����ĩβ�������ص�ǰ���鳤��
array.push('real');

//�Ƴ���������һ��������һ���
array.pop();

//�Ƴ�����ĵ�һ�������һ���
array.shift();

//�����������Ĳ�����ӵ����鿪ͷ�������ص�ǰ���鳤��
array.unshift('really');
```
### ����
```javascript
//��������(��������)
array.reverse();

//��������,���Դ���һ�����򷽷������ؾ������������
array.sort(compareFunction);
```
### �������
```javascript
//�������飬���ݵ�ǰ���鴴��һ�������飬����������ӵ�����������в����ؽ������
array.concat("");

//��ȡ���飬����ǰ�����start��end-1λ�ý�ȡ������ slice=���У�����
array.slice(startPos,endPos);

//ճ������
//ɾ����2����������ʼλ�á�ɾ��������
array.splice(startPos,num);
//���룺3����������ʼλ�á�ɾ�����������������Ĳ�����
array.splice(startPos,0,insertItems);
//�滻��3����������ʼλ�á������������������������
array.splice(startPos,num,insertItems);
```

### ����item�������е�λ��
```javascript
//����,�ҵ�����λ�ã�δ�ҵ�����-1
array.indexOf(item);
//����,�ҵ�����λ�ã�δ�ҵ�����-1
array.lastIndexOf(item);
```

### ��������
```javascript
//������yourFunction(item,index,array),(�ڶ�������Ϊthis���󣬿�ѡ)
//��ÿһ�����и���������ȫ��Ϊtrue�򷵻�true
array.every(yourFunction,[scope]);

//��ÿһ�����и������������ظú�������ֵΪtrue������ɵ�����
array.filter(yourFunction,[scope]);

//��ÿһ�����и����������޷���ֵ
array.forEach(yourFunction,[scope]);

//��ÿһ�����и�������������ÿ�κ���ִ�еĽ����ɵ����飬���ص������н��������
array.map(yourFunction,[scope]);

//��ÿһ�����и���������ֻҪ��һ���true���򷵻�true ��every()��Ӧ
array.some(yourFunction,[scope]);
```
### �鲢����
```javascript
//yourFunction(prev,current,index,array)
//�������
array.reduce(yourFunction);
//�������
array.reduceRight(yourFunction);
```