# push、pop、unshift、shift、splice、concat、 join 的用法


1、数组添加删除 头部或尾部（ push()、pop()、unshift()、shift() ）

数组尾部添加 push()方法可向数组的末尾添加一个或多个元素，并返回新的长度 
语法：arrayObject.push(newelement1,newelement2,….,newelementX)


数组尾部删除 pop（）方法用于删除并返回数组的最后一个元素 
语法：arrayObject.pop()

数组头部添加 unshift() 方法可向数组的开头添加一个或更多元素，并返回新的长度 
语法：arrayObject.unshift(newelement1,newelement2,….,newelementX)

数组头部删除 shift() 方法用于把数组的第一个元素从其中删除，并返回第一个元素的值 
语法：arrayObject.shift()

2、对数组删除\添加、替换的用法 splice()的用法 
数组删除 splice() –可以删除任意数量的项，只需要指定2个参数：要删除的第一项的位置和要删除项的项数 
语法: arr.splice(起点,长度) 【如 arr.splice(0,2) 会删除数组中的前两项。】
数组添加 splice() –可以向指定位置插入任意数量的项，只需要提供3个参数：插入起始位置、0（要删除的项数）和要插入的项。 如果要插入多个项，可以再传入第四、第五，一直任意多个项。 
语法：arr.splice(起点,长度为0,需要添加的元素) 【如 arr.splice(2,0,”a”,”b”)会从位置2开始插入字符串“a”和”b”】
数组的替换 splice()–即删除和插入数量相等项数的综合应用，可以指向指定位置插入任意数量的项，且同时删除任意数量的项，只需要指定3个指定参数：起始位置、要删除的项数和要插入的任意数量项。 插入的项数是不必与删除的项数相等。 
语法：arr.splice(起点,长度为要替换的个数,替换后的元素) 【如splice(2,2,”a”,”b”) 会删除当前数组位置2的项，然后再从位置2开始插入字符串“a”和“b”。】 
3、数组连接、分割（concat()、join()的用法）

数组连接 concat（） 方法用于连接两个或多个字符串。该方法没有改变原有字符串，但是会返回连接两个或多个字符串新字符串 

``` Javascript
btn[9].onclick = function(){
    var a = [1,2,3] 
    var b = [4,5,6]
    var arr = a.concat(b) //concat()方法用于连接两个或多个数组
    alert(arr) //1,2,3,4,5,6
}

```
数组分隔 join（）方法用于把数组中的所有元素放入一个字符串。 
语法：arrayObject.join(separator)

``` Javascript
btn[10].onclick = function(){
    var a = [1,2,3,4,5,6]
    a.join('-')//使用分隔符来分隔数组中的元素
    alert(a.join('-'))//1-2-3-4-5-6
}
```

## 参考
[push、pop、unshift、shift、splice、concat、 join 的用法](https://blog.csdn.net/lily2016n/article/details/76974910)


