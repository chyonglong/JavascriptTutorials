#### Javascript 进阶|变量、执行环境、变量对象、作用域链



广义的定义：被调用的函数B，在函数A里面定义并使用了函数A的变量，无论函数B是在函数A外还是函数A内被调用，闭包会产生，在Chrome中，函数A是闭包，因为A的函数被B使用了。从这个定义的话，全局环境，只要有一个函数定义了，并使用了全局变量，那全局环境也算是一个闭包。

function foo1() { // bar1()执行时foo1是闭包
            var a = 2;
            function bar1() {
                console.log(a); // 2
            }
            bar1(); 
        }
        foo1();


        var bar
        function foo2() { // bar2()执行时foo2是闭包
            var a = 2;
            function bar2() {
                console.log(a); // 2
            }
            bar = bar2;
        }
        foo2();
        bar()


特定的定义：被调用的函数B，在函数A里面定义并使用了函数A的变量，但是函数B在函数A外执行，被调用的函数B是闭包. 《你不知道的Javascript》

function foo1() { 
            var a = 2;
            function bar1() {
                console.log(a); // a 的使用只是正常作用域的使用
            }
            bar1(); 
        }
        foo1();


        var bar
        function foo2() { 
            var a = 2;
            function bar2() {// 这个是闭包， bar2 是 foo2 外的bar()执行时被调用的
                console.log(a); 
            }
            bar = bar2;
        }
        foo2();
        bar()


当然，闭包产生的前提是， 函数A要先被调用了，要不然连函数B都没有被定义了。

知道了这个定义对我们的编程有帮助才是最重要的 , 后面我谈的闭包是用广义的定义，因为我觉得可以更好的利用它的特性。

闭包有几个特性的：
- 闭包（函数A）里面的变量， 外面的函数访问不了， 所以它可以很好的隐藏或保护
- 闭包里面的函数（函数B）访问了或改变了闭包（函数A）的的变量，
- 可以通过调用闭包（函数A）的函数（函数B）去访问或改变闭包（函数A）里面的变量
- 函数B使用到的闭包（函数A）变量 有可能被函数B外重新赋值而变化


为什么要知道闭包: 
1、分辨的出函数的变量是不是被闭包的其它的地方所改变
2、我们可以通过闭包间接访问该闭包的内部数据



1、分辨的出函数的变量是不是被闭包的其它的地方所改变

譬如
for (var i=1; i<=5; i++) {
    setTimeout( function timer() {
        console.log(i);
    }, i*1000 )}
}

输出是什么？12345?

答案是
6
6
6
6
6

所以用闭包的概念的话

console.log(i) 里面的i其实是闭包（全局变量）的i,而setTimeout 会在for 循环后执行，所以i的变化会直接影响到 函数 timer 里面的 i, for 循环后执行i的值是6，所有i都会输出6

假如我们要输出12345， 可以怎么操作呢? 

居然timer 里面的i被全局的i所污染，但是这个不是我们想要的， 那我们可以在setTimeout 外面建一个闭包， 而不使用全局的i，就可以解决问题了

for (var i=1; i<=5; i++) {
    (function (i){
    setTimeout( function timer() {
        console.log(i);
    }, i*1000 )}
    )(i)
}

另外一个例子

function createFunctions(){
    var result = new Array();
    for (var i=0; i < 10; i++){
        result[i] = function(){
        return i; };
    }
    return result;
}

let rtn = createFunctions()
for (var i=0; i < 10; i++){
        console.log(rtn[i]())
    }

  
result 返回出去后再调用，result元素 里面i 是闭包createFunctions的i,而result被调用肯定在for循环后的， 这个时候 i = 10, 所以，现在所有的结果都是10

同里的，我们不想让function 里面的i被闭包createFunctions 的 i所污染，， function 外面建一个闭包，就可以解决问题了， 返回的result 是在createFunctions定义的， 所以 createFunctions 也是闭包。

function createFunctions(){
    var result = new Array();
    for (var i=0; i < 10; i++){

        (function(i){
        result[i] = function(){
        return i; 
        }})
        (i)
}
    return result;
}

let rtn = createFunctions()
for (var i=0; i < 10; i++){
        console.log(rtn[i]())  
}
// 输出 0 1 2 3 4 5 6 7 8 9


2、我们可以通过闭包间接访问该闭包的内部数据

譬如，实现一个简单的任务清单，我们不想清单可以随便被赋值或改变，同时清单的增删可以被外面函数调用， 具体的实现就可以通过闭包实现， 通过闭包定义 doList , 在返回对该doList 的操作！

模块的实现就是通过这样实现的。模块里面的变量是独立的， 但是我们可以通过调用模块里面的功能实现一些通用功能

      
        function myDoList() {

            var doList = []
            function addTask(task) {
                let index = doList.indexOf(task)
                if (index == -1) {
                    doList.push(task)
                    console.log("doList:", doList)
                }
                else {
                    console.log("doList:", doList, " 已经存在 ", task)
                }
            }
            function finishTask(task) {
                let index = doList.indexOf(task)
                if (index == -1) {
                    console.log("doList:", doList, " 不存在 ", task)
                }
                else {
                    doList.splice(index, 1);
                    console.log("doList:", doList)
                }


            }

            return {
                addTask: addTask,
                finishTask: finishTask,
            }
        }
        var myDoListOperation = myDoList()
        myDoListOperation.addTask('a') //doList: ["a"]
        myDoListOperation.addTask('b') //doList: (2) ["a", "b"]
        myDoListOperation.finishTask('a') //doList: ["b"]



函数声明转换成函数表达式


先编译后执行

声明提升

函数优先


参考

https://segmentfault.com/a/1190000012646221