#### Javascript 进阶|闭包


##### 举几个个例子为什么要用this?
- 1、 this 提供了一种更优雅的方式来隐式“传递”一个对象引用，因此可以将 API 设计得更加简洁并且易于复用。
- 2、 对象生成
- 3、 实现继承

1、 this 提供了一种更优雅的方式来隐式“传递”一个对象引用，因此可以将 API 设计得更加简洁并且易于复用。
``` javascript
function ask(){
    return this.name + " ask";
}

function answer(){
    return this.name + " answer";
}

var you ={
    name:'you'
}
var i = {
    name: 'i'
}

ask.call(you)
answer.call(i)

// 如果不通过this, 就必须传参
function ask(who){
    return who.name + " ask";
}

function answer(who){
    return who.name + " answer";
}

var you ={
    name:'you'
}
var i = {
    name: 'i'
}

ask(you)
answer(i)
```

2、 对象生成
``` javascript
function foo(a) { this.a = a;
    let obj={}
    obj.a = a
    return obj
}
var bar = foo(2)
console.log( bar.a ); // 2
```

new 约等于隐藏了 函数开头生成 this 的对象 同时最后返回 this

``` javascript
function foo(a) {
    // -- let this={}
    this.a = a;
    // -- return = this
}
var bar = new foo(2); 
console.log( bar.a ); // 2
```

3、 实现继承

``` javascript
Human (name,age){
    this.name = name
    this.age = age
}

Student (name,age,class){
    Human.call(this,name,age)
    this.class = class
}
```


##### 什么是 this

this 是在运行时进行绑定的，并不是在编写时绑定，它的上下文取决于函数调 用时的各种条件。this 的绑定和函数声明的位置没有任何关系，只取决于函数的调用方式。
当一个函数被调用时，会创建一个活动记录(有时候也称为执行上下文)。这个记录会包 含函数在哪里被调用(调用栈)、函数的调用方法、传入的参数等信息。this 就是记录的 其中一个属性，会在函数执行的过程中用到。

重要的一点是this 是在运行时进行绑定的 , 后面它的属性可以瞎改， 但是它是不能被重新赋值的。直接对this 赋值会错误。

##### 怎么来判断 this 的绑定对象

《你不知道的 Javascript 上》的总结很好的给出了答案了:

如果要判断一个运行中函数的 this 绑定，就需要找到这个函数的直接调用位置。找到之后
就可以顺序应用下面这四条规则来判断 this 的绑定对象。

- 1、由new调用?绑定到新创建的对象。
- 2、由call或者apply(或者bind)调用?绑定到指定的对象。
- 3、由上下文对象调用?绑定到那个上下文对象。
- 4、 默认:在严格模式下绑定到undefined，否则绑定到全局对象。

ES6 中的箭头函数并不会使用四条标准的绑定规则，而是根据当前的词法作用域来决定 this，具体来说，箭头函数会继承外层函数调用的 this 绑定(无论 this 绑定到什么)。这 其实和 ES6 之前代码中的 self = this 机制一样。


##### 分别来看看:

###### 1、 由new调用?绑定到新创建的对象。

在开头的《2、 对象生成》也描述， new , 把新建this 对象，并返回this 对象给创建的对象。

###### 2、 由call或者apply(或者bind)调用?绑定到指定的对象。

JavaScript内部提供了一种机制，让我们可以自行手动设置this的指向。它们就是call与apply。所有的函数都具有着两个方法。它们除了参数略有不同，其功能完全一样。它们的第一个参数都为this将要指向的对象。

而call与applay后面的参数，都是向将要执行的函数传递参数。其中call以一个一个的形式传递，apply以数组的形式传递。这是他们唯一的不同。

apply方法最多只有两个参数，第二个参数接收数组或者类数组，但是都会被转换成类数组传入func中，并且会被映射到func对应的参数上

``` javascript
function fn(num1, num2) {
    console.log(this.a + num1 + num2);
}
var obj = {
    a: 20
}

fn.call(obj, 100, 10); // 130
fn.apply(obj, [20, 10]); // 50
``` 

因为call/apply的存在，这让JavaScript变得十分灵活。因此就让call/apply拥有了很多有用处的场景。例如文章开头的例子1 和例子3。

调用内置对象的方法，实现特定的功能

对类数组操作
``` javascript
let arrObj = {
    0: 'a',
    1: 'b',
    2: 'c',
    3: 'd',
    length: 4
}

console.log(arrObj);
    //arrObj.push('e') //TypeError

Array.prototype.push.call(arrObj, 'e');
console.log(arrObj)

var arg2 = Array.prototype.slice.call(arrObj);
console.log(arg2)
```

查看值的类型
``` javascript
console.log(Object.prototype.toString.call("jerry"));//[object String]
console.log(Object.prototype.toString.call(12));//[object Number]
console.log(Object.prototype.toString.call(true));//[object Boolean]
console.log(Object.prototype.toString.call(undefined));//[object Undefined]
console.log(Object.prototype.toString.call(null));//[object Null]
console.log(Object.prototype.toString.call({name: "jerry"}));//[object Object]
console.log(Object.prototype.toString.call(function(){}));//[object Function]
console.log(Object.prototype.toString.call([]));//[object Array]
console.log(Object.prototype.toString.call(new Date));//[object Date]
console.log(Object.prototype.toString.call(/\d/));//[object RegExp]
function Person(){};
console.log(Object.prototype.toString.call(new Person));//[object Object]
```
合并两个数组

除了用 concat 也可以用push.apply
``` javascript
        var arr1 = [1, 2, 3]

        var arr2 =  [4, 5, 6]

        var arr = arr1.concat(arr2)

        arr1.push.apply(arr1,arr2);
```

###### 3、 由上下文对象调用?绑定到那个上下文对象。
###### 4、 默认:在严格模式下绑定到undefined，否则绑定到全局对象。

3和4，可以说的“谁调用它，this就指向谁”

- a. 如果没有对象调用的就指向全局对象
- b. 如果有对象调用的就指向对象
- c. 如果是对象对象的对象调用，指向最后一个对象

例子解释

``` javascript
var a = 0
var obj1 = {
    a: 1,
    obj2: {
        a: 2,
        obj3: {
            a: 3,
            obj3fn: function () {
                console.log(this.a);
            },
            obj3fn1: function () {
                function obj3fn2() {
                    console.log(this.a);
                }
                obj3fn2() //这里是直接调用，所以指向全局
            }
        }
    },
    obj1fn: function () {
        console.log(this.a);
    }
};

var f1 = obj1.obj1fn
f1() // 0 , 直接调用，所以指向全局
obj1.obj2.obj3.obj3fn1() // 0 , obj3fn2()这里是直接调用，所以指向全局 

obj1.obj1fn() // 1 , 被obj1调用， 所以obj1.a 被使用


obj1.obj2.obj3.obj3fn() // 3, // 1 , 对象obj1的obj2的obj3调用，最后一个是obj3， 所以obj3.a 被使用
```

###### 箭头函数

箭头函数的this有点像闭包里面的函数里面用了闭包的this, 所以如果直接调用箭头函数，this是指向闭包的。

``` javascript
function fn1 () {
    return () => {
        console.log(this)
    }
}
fn2 = fn1()
```

如这样，

``` javascript
function fn1 () {
    var _this = this
    return function () {
        console.log(_this)
    }
}
fn2 = fn1()
```
所以如果直接通过调用fn2 来修改 _this是不行的。

参考

- https://www.liaoxuefeng.com/wiki/1022910821149312/1031549578462080
- https://segmentfault.com/a/1190000012646488
- 《Javascript 高级程序设计（第 3 版）》
- 《你不知道的 Javascript》
