# JSON对象和字符串之间的相互转换 – JSON.parse() 和 JSON.stringify()

JSON.parse(string) ：接受一个 JSON 字符串并将其转换成一个 JavaScript 对象。
JSON.stringify(obj) ：接受一个 JavaScript 对象并将其转换为一个 JSON 字符串。



js 代码:
``` Javascript
var a={"name":"tom","sex":"男","age":"24"};
 
var b='{"name":"Mike","sex":"女","age":"29"}';
 
var aToStr=JSON.stringify(a);
 
var bToObj=JSON.parse(b);
 
console.log(typeof(aToStr)); ?//string
 
console.log(typeof(bToObj));//object
```
尽管这些方法通常用在对象上，但它们也可以在数组上使用：

JavaScript 代码:
``` Javascript
const myArr = ['bacon', 'letuce', 'tomatoes'];
 
const myArrStr = JSON.stringify(myArr);
 
console.log(myArrStr);
// "["bacon","letuce","tomatoes"]"
 
console.log(JSON.parse(myArrStr));
// ["bacon","letuce","tomatoes"]
```

## 参考
[JSON对象和字符串之间的相互转换 – JSON.parse() 和 JSON.stringify()](https://www.css88.com/archives/3919)

[你所不知道的JSON.parse() 和 JSON.stringify() – 高级用法](https://www.css88.com/archives/8735)
