# filter
filter()方法：用于把Array中的某些元素过滤掉，然后返回剩下的元素

filter()也接受一个函数，把传入的函数依次作用于每个元素，然后根据返回值是true还是false决定保留还是丢弃该元素。
```
arr.filter(function(ele,index,arr){ });
```
filter()接收的回调函数，其实可以有多个参数。通常我们仅使用第一个参数ele，表示Array的某个元素。回调函数还可以接收另外两个参数，表示元素的位置index和数组本身arr

例子1：
把一个数组arr = [‘A’, ”, ‘B’, null, undefined, ‘C’, ’ ‘];中的空字符串删掉：(学习trim())
```
var arr = ['A', '', 'B', null, undefined, 'C', '  '];
var r = arr.filter(function (s) {
    return s && s.trim(); // 注意：IE9以下的版本没有trim()方法
});
r;  // 结果:['A', 'B', 'C']
```
例子2：
去除Array的重复元素
```
var r,arr = ['apple', 'strawberry', 'banana', 'pear', 'apple','orange', 'orange', 'strawberry'];

r = arr.filter(function (element, index, self) {
    return self.indexOf(element) === index;  //indexOf总是返回第一个元素的位置
});

console.log(r.toString());
```
## 参考
[JS高级函数--------filter、sort](https://blog.csdn.net/baidu_36065997/article/details/79081643)


