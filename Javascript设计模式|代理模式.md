#### Javascript设计模式|单例模式

单例模式简述
概念：
单例模式思想在于保证一个特定类仅有一个实例，意味着当你第二次使用同一个类创建信对象时，应得到和第一次创建对象完全相同。

特点： 

1. 可以来划分命名空间，从而清除全局变量所带来的风险。
2. 可以把代码组织的更为一体，便于阅读和维护。
3. 可以被实例化，且实例化一次。


在javascript中用闭包的形式实现单例模式比较简单， 变量在其作用域中， 用函数访问，实现单个变量。

单例模式的简单实现


``` javascript
        var Singleton = function(name){
            this.name = name;
        };
        Singleton.prototype.getName = function(){
          return this.name;
        }
        // 获取实例对象
        var getInstance = (function() {
            var instance = null;
            return function(name) {
                if(!instance) {
                    instance = new Singleton(name);
                }
                return instance;
            }
        })();
        // 测试单例模式的实例
        var a = getInstance("aa");
        var b = getInstance("bb");
        console.log(b.getName()); // "aa"
        console.log(a === b);     // true
``` 

如上代码，实现一个单例模式，无非就是使用一个变量来标识该类是否被实例化，如果未被实例化的话，那么我们可以实例化一次，否则的话，直接返回已经被实例化的对象。

单例模式的运用

日常工作中，我们经常需要实现一个遮罩层，来防止用户中断页面操作。所谓的遮罩层，就是一个大小跟窗口一致的半透明div层。我们要求页面最多只能存在一个遮罩层，此时运用单例模式就再合适不过了。

以下是代码实现~~~
``` javascript
        var createMask = (function(){
            var div;
            return function(){
                if(!div) {
                    div = document.createElement("div");
                    div.innerHTML = "遮罩层";
                    div.style.display = 'none';
                    document.body.appendChild(div);
                }
                return div;
            }
        })();
        document.querySelector("body").onclick = function(){
            var win = createMask();
            win.style.display = "block";
        }
``` 
如上代码，虽然可以实现需求，但是不通用。如果业务又需要我们实现单例模式创建弹窗效果，势必需要copy一份代码，所以我们需要对单例模式进行封装。

单例模式的封装

var getInstance = function(fn) {
    var result;
    return function(){
        return result || (result = fn.call(this,arguments));
    }
};


如上代码：我们使用一个参数fn传递进去，如果有result这个实例的话，直接返回，否则的话，会去执行fn函数，并将结果保存到result中。

现在，不管我们需要实例化多少个对象，都使用getInstance来实现。

以下是代码示例~~~
``` javascript
        var createMask = function(){
            var div = document.createElement("div");
            div.innerHTML = "遮罩层";
            div.style.display = 'none';
            document.body.appendChild(div);
            return div;
        };
        // 创建iframe
        var createIframe = function(){
            var iframe = document.createElement("iframe");
            document.body.appendChild(iframe);
            return iframe;
        };
        // 获取实例的封装代码
        var getInstance = function(fn) {
            var result;
            return function(){
                return result || (result = fn.call(this,arguments));
            }
        };
        // 测试创建遮罩层
        var createSingleMask = getInstance(createMask);
        document.querySelector("body").onclick = function(){
            var win = createSingleMask();
            win.style.display = "block";
        };
        // 测试创建iframe
        var createSingleIframe = getInstance(createIframe);
        document.querySelector("body").onclick = function(){
            var win = createSingleIframe();
            win.src = "https://jiangxia.github.io/";
        };
``` 

单例模式在我们平时的应用中用的比较多的，相当于把我们的代码封装在一个起来，只是暴露一个入口，从而避免全部变量的污染。

参考：
https://juejin.im/post/5c071f2ff265da6115109302

---
> 桥智科技：科技赋能梦想！专注广州、深圳和惠州小程序定制开发、APP 应用定制开发、网站开发、区块链钱包开发！