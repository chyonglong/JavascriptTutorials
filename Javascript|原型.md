#### Javascript 进阶|原型与原型链


从问题开始， 

- 1、 例子 1 ， 我们要创建两个人对象，可以直接用对象定义赋值； 但是这样带来的问题是， 假如我有 1 万，2 万个人， 那不是要定义很多对象
- 2、 为了解决例子 1 的问题，例子 2 中，我们可以创建一个专门建立人的对象的函数（这个专门建立对象的函数就是构造函数，其实也是函数的一种），而 javascript 中可以用 new 关键字创建对象 ，如例子 3（new 约等于隐藏了 函数开头生成 this 的对象 同时最后返回 this）
- 3、 从例子 2、3 中， 即使我们从创建了 两个对象， 它们的方法事实上还是指向了不同的地方。同样的，问题来了，如果是两个对象还好，但是我们如果创建 1 万，2 万个， 那内存空间就会占用很多， 事实上，在例子中所有的 walk 方法都是一样的，我们有没有办法只用一个 walk 呢？ 答案就是 原型
- 4、 例子 3 中， 事实上， 对象本身没有 walk 方法，  walk 方法 是在 该对象的原型上， 那它怎么可以调用得到 walk 方法呢？ 那就是通过 原型链

##### 1、 例子 1 ， 我们要创建两个人对象，可以直接用对象定义赋值； 但是这样带来的问题是， 假如我有 1 万，2 万个人， 那不是要定义很多对象
``` javascript

    console.log(" 对象 - 例子 1：");

    var xiaoa = { name: "xiaoa", walk: function () { console.log(this.name + " walk") } };
    var xiaob = { name: "xiaob", walk: function () { console.log(this.name + "walk") } };

    xiaoa.walk();
    xiaob.walk();

    console.log("xiaoa.walk == xiaob.walk?: ", xiaoa.walk == xiaob.walk);
    // false
    console.log("== 例子 1 end ==");

```

##### 2、 为了解决例子 1 的问题，例子 2 中，我们可以创建一个专门建立人的对象的函数（这个专门建立对象的函数就是构造函数，其实也是函数的一种），而 javascript 中可以用 new 关键字创建对象 ，如例子 3（new 约等于隐藏了 函数开头生成 this 的对象 同时最后返回 this）
``` javascript

    console.log(" 对象 - 例子 2：");
    var createObject = function (name) {
        var obj = {};
        obj.name = name;
        obj.walk = function () { console.log(this.name + " walk") };
        return obj;
    }

    var xiaoc = createObject('xiaoc')
    var xiaod = createObject('xiaod')

    xiaoc.walk();
    xiaod.walk();

    console.log("xiaoc.walk == xiaod.walk?: ", xiaoc.walk == xiaod.walk);
    // false

    console.log("== 例子 2 end ==");

```
``` javascript
    console.log(" 对象 - 例子 3：");
    var createObject = function (name) {
        this.name = name;
        this.walk = function () { console.log(this.name + " walk") };
    }

    // new 约等于隐藏了 函数开头生成 this 的对象 同时最后返回 this
    var xiaoe = new createObject('xiaoe')
    var xiaof = new createObject('xiaof')

    xiaoe.walk();
    xiaof.walk();

    console.log("xiaoe.walk == xiaof.walk?: ", xiaoe.walk == xiaof.walk);
    // false
    console.log("== 例子 3 end ==");

```
##### 3、 从例子 2、3 中， 即使我们从创建了 两个对象， 它们的方法事实上还是指向了不同的地方。同样的，问题来了，如果是两个对象还好，但是我们如果创建 1 万，2 万个， 那内存空间就会占用很多， 事实上，在例子中所有的 walk 方法都是一样的，我们有没有办法只用一个 walk 呢？ 答案就是 原型

实际上，声明一个构造函数 ，除了会申请保存函数的内存空间，还会额外申请一个内存空间，用于存储构造函数 A 的原型对象; 在原型对象中默认有一个 constructor 属性存储了构造函数的地址 (引用)(也就是 constructor 指向了构造函数)， 而 createObject.prototype 下面的函数就可以被调用并且只占一份内存！ 看例子 4：

``` javascript
    console.log(" 对象 - 例子 4：");
    
    var createObject = function (name) {
        this.name = name;
    }
    
    // 声明一个构造函数 ，除了会申请保存函数的内存空间，还会额外申请一个内存空间，用于存储构造函数 A 的原型对象; 在原型对象中默认有一个 constructor 属性存储了构造函数的地址 (引用)(也就是 constructor 指向了构造函数)
    console.log("createObject.prototype.constructor == createObject ?: " , createObject.prototype.constructor == createObject ) 
    console.log("createObject prototype: " , createObject.prototype ) 

    createObject.prototype.walk = function () { console.log(this.name + " walk") };

    var xiaog = new createObject('xiaog')
    var xiaoh = new createObject('xiaoh')

    xiaog.walk();
    xiaoh.walk();

    console.log("xiaog.walk == xiaoh.walk?: ", xiaog.walk == xiaoh.walk);
    // true
    // 所以原型的作用: 共享属性，节省内存空间。

    console.log("== 例子 4 end ==");

```

原型的作用: 共享属性，节省内存空间。


##### 4、 例子 3 中， 事实上， 对象本身没有 walk 方法，  walk 方法 是在 该对象的原型上， 那它怎么可以调用得到 walk 方法呢？ 那就是通过 原型链

当调用对象里的方法时候， 如果该对象有此方法的时候， 它会调用自己的 walk; 而没有， 它才会调用 原型的 walk 方法。 那系统怎么知道要调用 原型的 walk 方法 呢？ 
同函数中的原型 (自动加了 prototype 对象) 类似， 而对象在创建时候，也会自动加了一个 `__proto__` 属性，用于指向它的原型; 同理的， 函数的 prototype 是一个对象， 所以它也有 `__proto__` 属性; 
所以可以看得到 新建对象 的  `__proto__` 是 构造函数的 prototype 对象 ; 构造函数的 prototype 对象 的 `__proto__` 最后是 Object.prototype ; Object.prototype.`__proto__` 的值为 null 。这样会形成由 `__proto__` 将对象和原型连起来的链条。这就是原型链。

看例子 5

``` javascript

    console.log(" 对象 - 例子 5：");
    var createObject = function (name) {
        this.name = name;
    }

    createObject.prototype.walk = function () { console.log(this.name + " walk") };

    var xiaog = new createObject('xiaog')
    var xiaoh = new createObject('xiaoh')

    // new 
    // var this = {}
    // this.

    xiaog.walk();//xiaog special walk
    xiaoh.walk();//xiaoh walk

    xiaog.walk = function () {
        console.log(this.name + " special walk")
    }

    xiaog.walk();//xiaog walk
    xiaoh.walk();//xiaoh walk
    console.log("xiaog.walk == xiaoh.walk?: ", xiaog.walk == xiaoh.walk);
    // false

    // 从例子看得到，当该对象如果有 walk 方法 的时候， 它会自己调用自己的 walk; 而没有 walk 方法 的时候 ， 它才会调用 原型的 walk 方法

    // 那系统怎么知道要调用 原型的 walk 方法 呢？ 
    // 同函数中的原型 (自动加了 prototype 对象) 类似， 而对象在创建时候，也会自动加了一个 __proto__ 属性，用于指向它的原型

    console.log("createObject.prototype == xiaog.__proto__: ", createObject.prototype == xiaog.__proto__);
    // true

    // 所以我们用 call 方法调用， 可以返回原型的 walk 方法
    xiaog.__proto__.walk.call(xiaog); //xiaog walk

    console.log("xiaog.prototype.walk == xiaoh.walk?: ", xiaog.__proto__.walk == xiaoh.walk);
    // true

    // 既然 对象在创建时候，也会自动加了一个 __proto__ 属性，用于指向它的原型 , 同理的， 函数的 prototype 是一个对象， 所以它也有 __proto__ 属性

    // 在这里 createObject.prototype 的 __proto__ 是 Object.prototype
    
    console.log("createObject.prototype.__proto__ == Object.prototype?: ", createObject.prototype.__proto__ == Object.prototype);
    // true


    // 所以可以看得到 xiaog 的  __proto__ 是 createObject.prototype ; createObject.prototype 的 __proto__ 是 Object.prototype ; Object.prototype.__proto__ 的值为 null 。这样会形成由 __proto__ 将对象和原型连起来的链条。这就是原型链


    console.log("== 例子 5 end ==");

```

一个小扩展是： 当我们用 变量 赋值 var arr = new Array(), 时， 其实我们用到了的函数，譬如 join, concat,find …… 事实上都是 Array.prototype 对象的函数，而如果调用 isPrototypeOf， 就是 Array.prototype.`__proto__` 的 Object.prototype 的 函数。 同理其他 内置对象 如 String Function, Nubmer Boolean …… 也一样。
