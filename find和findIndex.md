# ES6中Array.find()和findIndex()函数的用法详解
find()函数用来查找目标元素，找到就返回该元素，找不到返回undefined。

findIndex()函数也是查找目标元素，找到就返回元素的位置，找不到就返回-1。

他们的都是一个查找回调函数。

``` Javascript
[1, 2, 3, 4].find((value, index, arr) => {
})
```

查找函数有三个参数。

value：每一次迭代查找的数组元素。

index：每一次迭代查找的数组元素索引。

arr：被查找的数组。

例：

1.查找元素，返回找到的值，找不到返回undefined。

``` Javascript
const arr1 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]
var ret1 = arr1.find((value, index, arr) => {
 return value > 4
})
var ret2 = arr1.find((value, index, arr) => {
 return value > 14
})
console.log('%s', ret1)
console.log('%s', ret2)
```
结果：

undefined

2.查找元素，返回找到的index，找不到返回-1。

``` Javascript
var ret3 = arr1.findIndex((value, index, arr) => {
 return value > 4
})
 
var ret4 = arr1.findIndex((value, index, arr) => {
 return value > 14
})
console.log('%s', ret3)
console.log('%s', ret4)
```

结果：

4
-1

3.查找NaN。
``` Javascript
const arr2 = [1, 2, NaN, 4, 5, 6, 7, 8, 9, 10, 11]
var ret5 = arr2.find((value, index, arr) => {
 return Object.is(NaN, value)
})
var ret6 = arr2.findIndex((value, index, arr) => {
 return Object.is(NaN, value)
})
console.log('%s', ret5)
console.log('%s', ret6)
```
结果：

NaN 
2    

总结

以上所述是小编给大家介绍的ES6中Array.find()和findIndex()函数的用法详解，


## 参考
[ES6中Array.find()和findIndex()函数的用法详解](https://www.jb51.net/article/123813.htm)


