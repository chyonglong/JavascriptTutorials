#### Javascript 进阶|高阶函数

```
什么是高阶函数
一：函数作为参数传递
    1、 传递给函数的隐含参数：arguments
    2、事件机制
    3、回调函数
    4、作为一个加工器的加工剂
二：函数作为返回值输出
    1、闭包
    2、做一个通用函数
```

##### 什么是高阶函数

高阶函数英文叫 Higher-order function。高阶函数是对其他函数进行操作的函数，操作可以是将它们作为参数，或者返回它们。简单总结为高阶函数是一个接收函数作为参数或者将函数作为返回输出的函数。

在编程语言中，一等公民可以作为函数参数，可以作为函数返回值，也可以赋值给变量。
对于JavaScript来说，函数可以赋值给变量，也可以作为函数参数，还可以作为函数返回值，因此JavaScript中函数是一等公民。

下面把函数作为参数传递与函数作为返回值输出分别阐述！

##### 一：函数作为参数传递

总结函数作为参数传递的四种情况

- 1、传递给函数的隐含参数：arguments
- 2、事件机制
- 3、回调函数
- 4、作为一个加工器的加工剂
  
###### 1、 传递给函数的隐含参数：arguments

当进行函数调用时，除了指定的参数外，还创建一个隐含的对象——arguments。arguments是一个类似数组但不是数组的对象，说它类似是因为它具有数组一样的访问性质，可以用arguments[index]这样的语法取值，拥有数组长度属性length。
arguments对象存储的是实际传递给函数的参数，而不局限于函数声明所定义的参数列表，例如：

``` javascript
function func(a,b){
     alert(a);
     alert(b);
     for(var i=0;i＜arguments.length;i++){
           alert(arguments[i]);
     }
}
func(1,2,3);  
```

代码运行时会依次显示：1，2，1，2，3。因此，在定义函数的时候，即使不指定参数列表，仍然可以通过arguments引用到所获得的参数，这给编程带来了很大的灵活性。arguments对象的另一个属性是callee，它表示对函数对象本身的引用，这有利于实现无名函数的递归或者保证函数的封装性
例如使用递归来计算1到n的自然数之和：

``` javascript
var sum=function(n){
      if(1==n)return 1;
      else return n+sum(n-1);
}
alert(sum(100));
```

其中函数内部包含了对sum自身的调用，然而对于JavaScript来说，函数名仅仅是一个变量名，在函数内部调用sum即相当于调用一个全局变量，不能很好的体现出是调用自身，所以使用arguments.callee属性会是一个较好的办法：

``` javascript
var sum=function(n){
      if(1==n)return 1;
      else return n+arguments.callee(n-1);
}
alert(sum(100));  
```

###### 2、事件机制

事实上，将函数作为参数传递，或者是将函数赋值给其他变量是所有事件机制的基础。

例如，如果需要在页面载入时进行一些初始化工作，可以先定义一个init的初始化函数，再通过window.οnlοad=init;语句将其绑定到页面载入完成的事件。这里的init就是一个函数对象，它可以加入window的onload事件列表。

``` javascript
window.onload = init;
function init () {
    console.log("hihi");
}
```

或者在

``` javascript
var btn1 = document.getElementById("divArea");
btn1.addEventListener("click", function () {
    alert('divArea')
}, false);

<body>
    <div id="divArea" style="width: 200px;height: 200px;background-color: aqua;">
    </div>
</body>
```

###### 3、回调函数

setTimeout 或者ajax 出去， 等时间到或处理完后调用该回调函数！

``` javascript
setTimeout(function() {
    console.log("Hello, 我是回调函数!");
}, 1000);

ajax( "http://some.url", function() {
    console.log("Hello, 我是回调函数!");
} );
```

###### 4、作为一个加工器的加工剂

一般来说，函数就是加工器，最简单的输入a,b，获得a,b的和；加工器的意思是，对原始数据做处理得到不一样的值；而加工剂就是在里面对原始数据的处理调用到这个传进来的函数，炒一个菜加盐变咸的，加醋变酸！

作为加工剂，函数的调用肯定要返回值并且被调用的函数所用， 要不然它作为参数的意义就没有了， 所以该作为参数的函数一般肯定是有返回值的；

譬如我自己实现一个类似数组函数map的功能！就是简单的把每个原始数组的元素调用传进来的函数，再把参数的函数返回来的值return 回去！

``` javascript
Array.prototype.mymap = function (fn) {
    let arr2 = []
    for (let i = 0; i < this.length; i++) {
        arr2.push(fn(this[i]));
    }
    return arr2;
}

const arr1 = [1, 2, 3, 4];
const arr2 = arr1.mymap((a) => { return a * 2 });

console.log(arr2) // [2,4,6,8]
```

再如自己下一个数组排序的功能，函数用来比较前后两个值
``` javascript
Array.prototype.mysort = function (fn) {
    let arr2 = []
    arr2.push(this[0]);
    for (let i = 1; i < this.length; i++) {

        if (fn(this[i - 1], this[i]) > 0) {
            arr2.unshift(this[i])
        }
        else {
            arr2.push(this[i]);
        }
    }
    return arr2;
}
const arr1 = [1, 2, 3, 4];
var arr3 = arr1.mysort((a, b) => { return b - a });

console.log(arr3) // [4,3,2,1]
```

我想大家留意到了，JavaScript中内置的高阶函数如 Array.prototype.map，Array.prototype.filter，Array.prototype.reduce和Array.prototype.sort, 它们接受一个函数作为参数，并应用这个函数到列表的每一个元素。

##### 二：函数作为返回值输出

让函数继续返回一个可执行的函数，意味着运算过程是可延续的, 居然函数作为返回值，那肯定是要被调用的，对吧！关键是为什么要把函数做返回值来调用! 列举两个作用闭包和通用函数！

###### 1、闭包

闭包, 调用函数修改私有变量

var advanceCount = (function (){
    var count = 0
    return function(){count ++ }
})()

advanceCount()
advanceCount()
advanceCount()

###### 2、做一个通用函数

函数封装, 做一个通用函数!

例如判断一个变量的类型！

``` javascript
var isString = function(obj){
    return Object.prototype.toString.call(obj) === '[object String]';
}
var isArray = function(obj){
    return Object.prototype.toString.call(obj) === '[object Array]';
}
var isNumber = function(obj){
    return Object.prototype.toString.call(obj) === '[object Number]';
}
/**************
    封装下
 **************/
var isType = function(type){
    return function (obj){
        return Object.prototype.toString.call( obj ) === '[object '+type+']';
    }
};
var isString = isType('String');
console.log(isString('str'))
```

再譬如一个对象的排序，实现可以不同按不同字段排序，我们可以每个字段排序起一个函数，但是如果我有n多个字段的时候，就会代码冗余, 所以可以通过传参和返回函数建一个通用的函数！

``` javascript
var studentsData =
    [{ name: "loren", age: 17 },
    { name: "mike", age: 16 },
    { name: "frank", age: 18 }];


function compareFunctionByName(obj1, obj2) {

    if (obj1.name > obj2.name) {
        return -1
    } else {
        return 1
    }

};

function compareFunctionByAge(obj1, obj2) {
    if (obj1.age > obj2.age) {
        return -1
    } else {
        return 1
    }
};

console.log(JSON.stringify(studentsData.sort(compareFunctionByName)))

console.log(JSON.stringify(studentsData.sort(compareFunctionByAge)))

/**************
    封装下
 **************/
function createCompareFunction(field) {
    return function (obj1, obj2) {
        if (obj1[field] > obj2[field]) {
            return -1
        } else {
            return 1
        }
    };
}

console.log(JSON.stringify(studentsData.sort(createCompareFunction("name"))))
console.log(JSON.stringify(studentsData.sort(createCompareFunction("age"))))

```

参考：
- https://blog.fundebug.com/tags/JavaScript%E6%B7%B1%E5%85%A5%E6%B5%85%E5%87%BA/
- https://www.cnblogs.com/kongxianghai/archive/2013/03/24/2979856.html
- JavaScript设计模式与开发实践