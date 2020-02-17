# Javascript对象的引用，对象的拷贝

js基本数据类型（undefined,boolean,number,string,null）

``` javascrip
var a = new String("123");
var b = new String("123");
alert(a==b); //结果返回false
```
那么问题来了，自定义对象不是 按值比较的？
总结：基本数据类型是值比较，非基本数据类型比对的内存地址。
``` javascrip
var a = new Object();
a.price = 173;
var b = a;
b.price = 170; //b更改了属性值，a的属性值一起会被改变
```

1、场景
除了基本类型跟null,对象之间的赋值，只是将地址指向同一个，而不是真正意义上的拷贝

将一个对象赋值给另外一个对象。

一个对象重新赋值也会重新指想新的对象地址
``` javascript
var a = [1,2,3];
var b = a;
b.push(4); // b中添加了一个4
console.log("a:",a); // a变成了[1,2,3,4]  
console.log("b:",b); // b[1,2,3,4]  
var c = [5,6,7];
var b = c;
console.log("b重新指向c"); // b[1,2,3,4]  
c.push(8)
console.log("b:",b); // a变成了[5,6,7,8]
console.log("c:",c); // b变成了[5,6,7,8] 
```
自定义对象
``` javascrip
var obj = {a:10};
var obj2 = obj;
obj2.a = 20; // obj2.a改变了，
alert(obj.a); // 20，obj的a跟着改变
```  
这就是由于对象类型直接赋值，只是将引用指向同一个地址，导致修改了obj会导致obj2也被修改

2、浅拷贝
所以，我们需要封装一个函数，来对对象进行拷贝，通过for in 循环获取基本类型，赋值每一个基本类型，才能真正意义上的复制一个对象
``` javascrip
var obj = {a:10};
function copy(obj){
    var newobj = {};
    for ( var attr in obj) {
        newobj[attr] = obj[attr];
    }
    return newobj;
}
var obj2 = copy(obj);
obj2.a = 20;
alert(obj.a); //10
```
这样就解决了对象赋值的问题。

3、深拷贝
但是这里存在隐患，如果obj中，a的值不是10，而是一个对象，这样就会导致在for in中，将a这个对象的引用赋值为新对象，导致存在对象引用的问题。
``` javascrip
var obj = {a:{b:10}};
function copy(obj){
    var newobj = {};
    for ( var attr in obj) {
        newobj[attr] = obj[attr];
    }
    return newobj;
}
var obj2 = copy(obj);
obj2.a.b = 20;
alert(obj.a.b); //20
```
因此，由于这个copy对象只是对第一层进行拷贝，无法拷贝深层的对象，这个copy为浅拷贝，我们需要通过递归，来拷贝深层的对象。将copy改造成递归即可
``` javascrip
var obj = {a:{b:10}};
function deepCopy(obj){
    if(typeof obj != 'object'){
        return obj;
    }
    var newobj = {};
    for ( var attr in obj) {
        newobj[attr] = deepCopy(obj[attr]);
    }
    return newobj;
}
var obj2 = deepCopy(obj);
obj2.a.b = 20;
alert(obj.a.b); //10  
```
## 参考
[js中值引用和地址引用](https://www.kancloud.cn/lmw6412036/js/347021)
[JS对象的引用，对象的拷贝](https://www.kancloud.cn/lmw6412036/js/347022)


