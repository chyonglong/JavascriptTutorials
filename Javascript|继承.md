#### Javascript 进阶|继承

面向对象编程很重要的一个方面，就是对象的继承。A 对象通过继承 B 对象，就能直接拥有 B 对象的所有属性和方法。这对于代码的复用是非常有用的。

大部分面向对象的编程语言，都是通过 “类”（class）实现对象的继承。传统上，JavaScript 语言的继承不通过 class(ES6 引入了 class 语法)，而是通过 “原型对象”（prototype）实现。

直接借助例子完成继承
- 1、例子 1 ， 很明显的 Student 是继承自 Human ， 但是这里同样的代码重复了写了， 那如何实现继承呢？
- 2、实现继承前的知识点
  - 2.1 call 函数
  - 2.2 new
  - 2.3 object.create()
- 3、例子 2， 实现继承

##### 1、 例子 1 ， 很明显的 Student 是继承自 Human ， 但是这里同样的代码重复了写了， 那如何实现继承呢？
``` javascript

console.log(" 对象继承 - 例子 1：");

var Human = function (name) { 
    this.name = name;
}

Human.prototype.walk = function () { console.log(this.name + " walk") };

var Student = function (name,  classNo) {
    this.name = name; // 属性可以来自父类 Human
    this.classNo = classNo
}

Student.prototype.walk = function () { console.log(this.name + " walk") }; // 方法可以来自父类 Human
Student.prototype.goToSchool = function () { console.log(this.name + " go to school class " + this.classNo) };

var studentA = new Student('studentA', 'classA')
studentA.walk();
studentA.goToSchool();


console.log(" 对象继承 - 例子 1 end");

```

##### 2、实现继承前的知识点
###### 2.1 call 函数

**call 函数 ： 调用所有者对象作为参数的方法**。直接看例子：

``` javascript

console.log(" 对象继承 - 例子 2：");

var Human = function (name) { 
    this.name = name;
}

var xianA = {name:"xianA"}
Human.call(xianA,'xianAChanged') // 通过调用 ， Human 的 this 会是 xianA
console.log(xianA) //{name: "xianAChanged"}

console.log(" 对象继承 - 例子 2：");

```


###### 2.2 new

**new 约等于在构造函数添加一个临时对象 ,  并产生 `__proto__` 和用该函数的原型赋值， 再返回临时对象**

``` javascript
// function fn(属性){
//   var 临时对象 = {};
//   临时对象 .__proto__ = fn.prototype;
//   临时对象 . 属性
//   return 临时对象;
// }
```

###### 2.3 object.create()

**Object.create 新建一个 构造函数， 并把参数的对象赋值它的原型， 通过 new 的方式返回对象实例出来。**再结合 new 里面的逻辑 `临时对象.__proto__ = fn.prototype;` , 返回出来的实例的 `__proto__` 其实就是 fn.prototype , 也就是传过来的 obj, 也就是**`实例.__proto__ = fn.prototype = obj`**!

``` javascript
// Object.create(obj){
// var fn = function() { }
// fn.prototype = obj 
// return new fn()  // new 的 临时对象 .__proto__ = fn.prototype = obj
// }
```

##### 3、例子 2， 实现继承

**属性继承**：如 2.1 说的， 我们可以通过 call 指定所有者对象 , 所以同理，我们直接用 Human.call(this,name) 就可以实现继承 Human 的属性了

**方法继承**：Student.prototype 怎么样继承 Human.prototype? 

在文章 Javascript 进阶|原型与原型链 中， 我们提到过原型链 , 对象在创建时候，会自动加了一个 `__proto__` 属性，用于指向它的原型; 函数的 prototype 是一个对象， 所以它也有 `__proto__` 属性; 函数的调用是， 当本对象不存在调用函数时候，就会找到它的 `__proto__` 的 prototype 对象 ，如果有就调用， 没有再找; 直到找到或 "not a function"!

所以 Student.prototype 继承 Human.prototype, 事实上我们要实现的是 Student.prototype.`__proto__` = Human.prototype

结合 2.2 和 2.3 , 只要通过 Student.prototype = Object.create(Human.prototype) 就简单完成了， 因为`实例.__proto__ = fn.prototype = obj`(实例是 Student.prototype; obj 是 Human.prototype), 所以 **Student.prototype.`__proto__`= Human.prototype, 也就是实现了继承了**。

最好， Student.prototype 还是没有自己的 constructor, 它要找的话还是向自己的原型对象上找最后还是找到 Human.prototype, constructor 还是 Human , 所以要给 Student.prototype 写自己的 constructor, Student.prototype.constructor = Student 到此， 继承完成！
 

``` javascript

console.log(" 对象继承 - 例子 1：");

var Human = function (name) { 
    this.name = name;
}

Human.prototype.walk = function () { console.log(this.name + " walk") };

var Student = function (name,  classNo) {
    // this.name = name; // 属性可以来自父类 Human
    Human.call(this,name) // 完成 Human 继承
    this.classNo = classNo
}

// Student.prototype.walk = function () { console.log(this.name + " walk") }; // 方法可以来自父类 Human
Student.prototype = Object.create(Human.prototype);
Student.prototype.goToSchool = function () { console.log(this.name + " go to school class " + this.classNo) };
Student.prototype.constructor = Student;
//Student.prototype 还是没有自己的 constructor, 它要找的话还是向自己的原型对象上找最后还是找到 Human.prototype, constructor 还是 Human , 所以要给 Student.prototype 写自己的 constructor:

var studentA = new Student('studentA', 'classA');
studentA.walk();
studentA.goToSchool();

console.log(" 对象继承 - 例子 1 end");

```

##### 参考

https://www.cnblogs.com/BluceLee/p/10360885.html