# sort
sort()方法：用于排序，默认把所有元素先转换为String再排序，所以不能直接对数字排序

但是sort()方法可以接受函数，可以利用函数对数组进行排序
```
按数字从小到大排序

var arr = [10, 20, 1, 2];
arr.sort(function (x, y) {
    if (x < y) {
        return -1;
    }
    if (x > y) {
        return 1;
    }
    return 0;
});
console.log(arr);   // 结果：[1, 2, 10, 20]
```
sort()方法会直接对Array进行修改，它返回的结果仍是当前Array：

```
var a1 = ['B', 'A', 'C'];
var a2 = a1.sort();
a1; // ['A', 'B', 'C']
a2; // ['A', 'B', 'C']
a1 === a2; // true, a1和a2是同一对象
```

## 参考
[JS高级函数--------filter、sort](https://blog.csdn.net/baidu_36065997/article/details/79081643)


