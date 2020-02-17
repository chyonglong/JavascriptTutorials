

#### Javascript 进阶|正则表达式

正则表达式用于对字符串模式匹配及检索替换，是对字符串执行模式匹配的强大工具。

这样说比较模糊， 来个具体问题：

- a. 要找出字符串 `ee123ad32def1aa0`的所有连着数字 , 怎么样做？ 
- b. 要替换字符串 `ee123ad32def1aa0`的连着的字符到 x, 怎么样做？
- c. 要替换字符串 `ee123ad32def1aa0@xxx.xxx` 是不是合法邮箱 , 怎么样做？

当然的，我们可以编程把字符串里的每个字符做验证， 但是这样就显得有点繁琐 ! 所以正则表达式的左右就要来了！它是字符串的预定好的规则，从而来匹配字符串而达到查找，替换或者验证的功能！


下面分几部分介绍正则表达式同时在例子中实现开头的几个问题：
- 正则表达式查找字符串
  - 1、 直接字符的获取
  - 2、 修饰符
  - 3、 转义字符
  - 4、 量词
  - 5、 范围
- 正则表达式用于替换
- 正则表达式用于验证

##### 正则表达式查找字符串

接下来 ， 我用例子来带出正则表达式

首先， 定义正则有两种方式， 通过构造函数 和 perl 的编程格式， 通常， 我们的会有 perl 的编程格式 , 后面的定义也会直接用 perl 的编程格式。
``` javascript 
var re = new RegExp("a"); // 构造函数
var re = /a/; // perl 的编程格式
``` 

为了方便打印， 我自己先写了一个打印函数， 用于打印结果 , 后面的例子会直接调用。
``` javascript 
var printLog = function (orginalStr, patt, str) {
    document.write(orginalStr,'    ----   ' , patt.toString(), " -> ", str, '<br>');
}
```



###### 1. 直接字符的获取

开始之前介绍一个 **String.match(RegExg)** 方法 , match() 方法可在字符串内检索指定的值，或找到一个或多个正则表达式的匹配。 

简单的， 我要获取 `ee123ad32def1aa0` 的 1 , 返回 1； 而如果我要找 4， 由于字符串不存在 4, 所以返回 null

``` javascript 
var str = `ee123ad32def1aa0`

var patt = /1/

printLog(str, patt, str.match(patt))
// 输出： ee123ad32def1aa0 ---- /1/ -> 1

var patt = /4/

printLog(str, patt, str.match(patt))
// 输出： ee123ad32def1aa0 ---- /4/ -> null
```

###### 2. 修饰符

上例子中， 我们要找 `ee123ad32def1aa0` 的 1，  `patt = /1/` 返回了 1 个 1, 但是字符串里面是有 2 个 1 的， 我想做是把所有的 1 都选出来。 

在正则中， 通过在表达式后面加一个字符 g `patt = /1/g` ， 用于代表搜索字符串全部， 如例子 , 输出 1,1


``` javascript 
var str = `ee123ad32def1aa0`

var patt = /1/g

printLog(str, patt, str.match(patt))

// 输出： ee123ad32def1aa0 ---- /1/g -> 1,1
```

表达式后面加一个字符 g, 被称作修饰符。 g	代表：执行全局匹配 还有 i 代表：执行对大小写不敏感的匹配。


###### 3. 转义字符

上例子中， 我们直接查找 1， 但是我们可以查找把所有的数字找出来吗 ? 当然，是有的，**正则中用可以用`\d`代表所有的数字，**

``` javascript 
var str = `ee123ad32def1aa0`

var patt = /\d/g

printLog(str, patt, str.match(patt))
// 输出：ee123ad32def1aa0 ---- /\d/g -> 1,2,3,3,2,1,0
```

\d 就是转义字符。如下是常用的转义字符， 也可以在参考文档中查看更多的转义字符
- \w	查找单词字符。单词字符包括：a-z、A-Z、0-9，以及下划线 , 包含 _ (下划线) 字符。
- \W	查找非单词字符。
- \d	查找数字。
- \D	查找非数字字符。
- \s	查找空白字符。
- \S	查找非空白字符。


###### 4. 量词

例子 3 中， 输出的 1,2,3,3,2,1,0 数字都被拆成一个一个的元素， 那我可以限定 2 个数字吗？

尝试用加一个`\d` - `/\d\d/g`输出， 例子如下

``` javascript 
var str = `ee123ad32def1aa0`

var patt = /\d\d/g

printLog(str, patt, str.match(patt))
// 输出：ee123ad32def1aa0 ---- /\d\d/g -> 12,32
```

发觉 12,32 的确两个数字作为一个元素了。但是如果我 3 个甚至 9 个呢？ **正则用大括号{}表示元素的个数**， 如下例


``` javascript 
var str = `ee123ad32def1aa0`

var patt = /\d{3}/g // 如果要 4 个，把 3 改成 4 就好

printLog(str, patt, str.match(patt))
// 输出：ee123ad32def1aa0 ---- /\d{3}/g -> 123
```

可是， 往往我们现实生活中，一般不是固定的数字元素的， 可能是 1 个或者 1 个以上， **大括号{}可以加第二个元素， 表示 大于等于第一个数字， 小于等于第二个数字，** 譬如`{1,9}`, 找出 1 个或 2-9 个连着的数字。看例子输出`123,32,1,0`就被输出了。


``` javascript
var str = `ee123ad32def1aa0`

var patt = /\d{1,9}/g // 如果要 4 个，把 3 改成 4 就好

printLog(str, patt, str.match(patt))
// 输出：ee123ad32def1aa0 ---- /\d{1,9}/g -> 123,32,1,0
```

那如果我们实现 1 个或无数个呢 ? 只要把 大括号{}的第二个元素为空，但是逗号保留就可以了`/\d{1,}/g`, 而事实上系统定义了一个常用量词`+`(`/\d+/g`) 也代表了 1 个或无数个 , 下例子中， 它们的输出是一样的。

**这里也回答了文章开头的第一个问题 a**

*a. 要找出字符串 `ee123ad32def1aa0`的所有连着数字 , 怎么样做？* 

``` javascript
var str = `ee123ad32def1aa0`

var patt = /\d{1,}/g

printLog(str, patt, str.match(patt))
// 输出：ee123ad32def1aa0 ---- /\d{1,}/g -> 123,32,1,0

var patt = /\d+/g

printLog(str, patt, str.match(patt))
// 输出：ee123ad32def1aa0 ---- /\d+/g -> 123,32,1,0
```

更多量词介绍:


| 量词   | 描述                                                                                                                                                                                                                                                                 |
| ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| n+     | 匹配任何包含至少一个 n 的字符串。 例如，/a+/ 匹配 "candy" 中的 "a"，"caaaaaaandy" 中所有的 "a"。                                                                                                                                                                     |
| n*     | 匹配任何包含零个或多个 n 的字符串。例如，/bo*/ 匹配 "A ghost booooed" 中的 "boooo"，"A bird warbled" 中的 "b"，但是不匹配 "A goat grunted"。                                                                                                                         |
| n?     | 匹配任何包含零个或一个 n 的字符串。例如，/e?le?/ 匹配 "angel" 中的 "el"，"angle" 中的 "le"。                                                                                                                                                                         |
| n{X}   | 匹配包含 X 个 n 的序列的字符串。例如，/a{2}/ 不匹配 "candy," 中的 "a"，但是匹配 "caandy," 中的两个 "a"，且匹配 "caaandy." 中的前两个 "a"。                                                                                                                           |
| n{X,}  | X 是一个正整数。前面的模式 n 连续出现至少 X 次时匹配。例如，/a{2,}/ 不匹配 "candy" 中的 "a"，但是匹配 "caandy" 和 "caaaaaaandy." 中所有的 "a"。                                                                                                                      |
| n{X,Y} | X 和 Y 为正整数。前面的模式 n 连续出现至少 X 次，至多 Y 次时匹配。例如，/a{1,3}/ 不匹配 "cndy"，匹配 "candy," 中的 "a"，"caandy," 中的两个 "a"，匹配 "caaaaaaandy" 中的前面三个 "a"。注意，当匹配 "caaaaaaandy" 时，即使原始字符串拥有更多的 "a"，匹配项也是 "aaa"。 |
| n$     | 匹配任何结尾为 n 的字符串。                                                                                                                                                                                                                                          |
| ^n     | 匹配任何开头为 n 的字符串。                                                                                                                                                                                                                                          |
| ?=n    | 匹配任何其后紧接指定字符串 n 的字符串。                                                                                                                                                                                                                              |
| ?!n    | 匹配任何其后没有紧接指定字符串 n 的字符串。                                                                                                                                                                                                                          |

###### 5. 范围

`\d` 的转义字符，包括了 0-9 的所有数字， 那如果我们只想要 1-9 呢？ **正则用方括号查找某个范围内的字符**， 譬如 1-9 用`/[1-9]+/g` , 如下例， 0 数字就不包括了

``` javascript
var str = `ee123ad32def1aa0`

var patt = /[1-9]+/g

printLog(str, patt, str.match(patt))

// 输出：ee123ad32def1aa0 ---- /[1-9]+/g -> 123,32,1
```

它也可以专门**挑选一些值**，譬如，挑选 1 和 2: `/[12]+/g`, 它也**等同于用括号加|符号的形式**`/(1|2)+/g`，
``` javascript
var str = `ee123ad32def1aa0`

var patt = /[12]+/g

printLog(str, patt, str.match(patt))
// 输出：ee123ad32def1aa0 ---- /[12]+/g -> 12,2,1

var patt = /(1|2)+/g

printLog(str, patt, str.match(patt))
// 输出：ee123ad32def1aa0 ---- /(1|2)+/g -> 12,2,1
```

**方括号里也可以做到非操作**，譬如查找不是数字的所有字符
``` javascript
var str = `ee123ad32def1aa0`

var patt = /[^0-9]+/g

printLog(str, patt, str.match(patt))
// 输出：ee123ad32def1aa0 ---- /[^0-9]+/g -> ee,ad,def,aa
```

更多方括号用法：

| 表达式             | 描述                               |
| ------------------ | ---------------------------------- |
| [abc]              | 查找方括号之间的任何字符。         |
| [^abc]             | 查找任何不在方括号之间的字符。     |
| [0-9]              | 查找任何从 0 至 9 的数字。         |
| [a-z]              | 查找任何从小写 a 到小写 z 的字符。 |
| [A-Z]              | 查找任何从大写 A 到大写 Z 的字符。 |
| [A-z]              | 查找任何从大写 A 到小写 z 的字符。 |
| [adgk]             | 查找给定集合内的任何字符。         |
| [^adgk]            | 查找给定集合外的任何字符。         |
| (red\|blue\|green) | 查找任何指定的选项。               |


##### 正则表达式用于替换

查找功能介绍完了， 大部分的搜索举一反三就好！** 那我们查找出字符后，可以通过 replace 替换字符串！**

例如把所有非数字替换成 x,  ee123ad32def1aa0 替换后就是 x123x32x1x0
``` javascript
var str = `ee123ad32def1aa0`

var patt = /[^0-9]+/g

printLog(str, patt, str.match(patt))
// 输出：ee123ad32def1aa0 ---- /[^0-9]+/g -> ee,ad,def,aa

var str2 = str.replace(patt,'x')
printLog(str, patt, str2);
// 输出： ee123ad32def1aa0 ---- /[^0-9]+/g -> x123x32x1x0
```

**这里也回答了文章开头的第二个问题 b**

*b. 要替换字符串 `ee123ad32def1aa0`的连着的字符到 x, 怎么样做？* 

##### 正则表达式用于验证

正则表达式用在验证某个字段是非常有效的， 

这里用到了**正则的 test() 方法**

> RegExpObject.test(string)
>> test() 方法用于检测一个字符串是否匹配某个模式 .
>>
>> 如果字符串中有匹配的值返回 true ，否则返回 false。

譬如我测试 `ee123ad32def1aa0`是不是含有 d(输出 true) 和含有 9(输出 false)

  
``` javascript
var str = `ee123ad32def1aa0`

var patt = /d/g
document.write(patt.test(str)+'<br>')
// 输出： true
var patt = /9/g
document.write(patt.test(str)+'<br>')
// 输出： false
```

**这里也来回答文章开头的第三个问题 c**

*c. 要替换字符串 `ee123ad32def1aa0@xxx.xxx` 是不是合法邮箱 , 怎么样做？* 

``AAA@BBB.CCC``， 首先分析邮箱的正则：

1. 整体， 其实是不分大小写的  ，所以是需要加 i 的修饰符 如 //i
2. AAA 部分， 1 个或多个 a-z、A-Z、0-9，或下划线 ，也就是 通配符 \w+
3. @ ， 直接字符
4. BBB 部分， 1 个或多个 a-z、A-Z、0-9, 可以写成 [a-z0-9]+
5. . ， 应该它是系统字符，所以需要转义，写成\.
6. CCC 部分， 1 个或多个 a-z、A-Z, 可以写成 [a-z]+
7. 最后需要注意的是，因为 test 是只要找到字符串中存在这个模式就会是 true , 所以头尾也要符合的格式， 所以需要加头 ^ 和 尾 $ 
   
最后得出来的正则是 /^\w+@[a-z0-9]+\.[a-z]+$/i

例子测试 , 输出正确
``` javascript
var patt = /^\w+@[a-z0-9]+\.[a-z]+$/i
var str1 = '！xx1@aa.com'
document.write(patt.test(str1)+'<br>')
// 输出： false
var str1 = 'xx1@aa.com!'
var patt = /^\w+@[a-z0-9]+\.[a-z]+$/i
document.write(patt.test(str1)+'<br>')
// 输出： false
var str1 = 'xx1aa.com!'
var patt = /^\w+@[a-z0-9]+\.[a-z]+$/i
document.write(patt.test(str1)+'<br>')
// 输出： false
var str1 = 'xx1@aacom!'
var patt = /^\w+@[a-z0-9]+\.[a-z]+$/i
document.write(patt.test(str1)+'<br>')
// 输出： false
var str1 = 'xx1aacom!'
var patt = /^\w+@[a-z0-9]+\.[a-z]+$/i
document.write(patt.test(str1)+'<br>')
// 输出： false
var str1 = 'xx1@aa.com'
var patt = /^\w+@[a-z0-9]+\.[a-z]+$/i
document.write(patt.test(str1)+'<br>')
// 输出： true
```


到此，正则介绍完了！不了解正则的好像是天书，看不懂， 但是了解了它的定义模式后， 它定义规则也是才几个的： **直接字符的获取、 修饰符、 转义字符、 量词和范围**，而懂得了就可以在未来多多使用了！

##### 参考

https://www.runoob.com/jsref/jsref-obj-regexp.html