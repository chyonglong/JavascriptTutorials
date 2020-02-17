#### 位置和宽度：clientWidth、offsetWidth、scrollWidth等区别

立即执行函数（IIFE），也叫做自执行函数，就是不需要调用就立马执行的函数。

在解释立即函数的时候，我们先了解一下三个函数相关的知识：

函数声明：

function fun() {

}
函数表达式：
var fun = function () {

}
匿名函数：
function() {

}

立即函数有两种常见格式：

(function() {
console.log(999)
}())

(function() {
console.log(999)
})()

这两种格式都能保证函数立马执行，这也是立即函数的基础常见的格式，()运算符加上匿名函数，还有另外几种格式也能立即执行：

!function() {
console.log(999)
}()

+function() {
console.log(999)
}()

-function() {
console.log(999)
}()

=function() {
console.log(999)
}()

Var fun = function() {
console.log(999)
}()

运算符!、+、-、=和函数表达式都能打到立即执行。上面的方法，是匿名函数加上运算符，其实把匿名函数都换成函数声明也是一样的，也能变成立即执行函数：

!function fun() {
console.log(999)
}()

那么我们为什么要使用立即函数呢？我们都知道JavaScript没有块级作用域，只要函数作用域，立即函数最大的用途就是创建一个函数作用域，也就是创建一个私有的空间。

我们都知道jQuery就是一个匿名函数，看源码可以看见jQuery所有内容都包含在匿名函数里面

( function( global, factory ) {

}

创建一个函数作用域是所有JS插件必须要有的功能，以确保各JS插件创建的变量不能和其他JS插件的变量还有引入使用程序的变量发生冲突。

参考
https://blog.csdn.net/wade3po/article/details/86749348

---
> 桥智科技：科技赋能梦想！专注广州、深圳和惠州小程序定制开发、APP 应用定制开发、网站开发、区块链钱包开发！