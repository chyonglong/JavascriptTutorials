# JSON的key值为数字时如何使用

JJSON(JavaScript Object Notation) 是一种轻量级的数据交换格式。易于人阅读和编写。同时也易于机器解析和生成。它基于JavaScript（Standard ECMA-262 3rd Edition - December 1999）的一个子集。 JSON采用完全独立于语言的文本格式，但是也使用了类似于C语言家族的习惯（包括C, C++, C#, Java, JavaScript, Perl, Python等）。这些特性使JSON成为理想的数据交换语言。 
        

        比较标准的写法： 
Javascript代码  收藏代码
var json = '{"a":"1", "b":"2"}';    
var data = eval('('+ json +')');    
alert(data.a);    
alert(data['a']);    

       这样两种方式都可以取到json中的值。 

        但是当key的值为数字时，只能使用类似数组下表的访问方式取值。 
Javascript代码  收藏代码
var json = '{"0":"a", "1":"b", "length":2}';    
var data = eval('(' + json + ')');    
//alert(data.0);    //报错，此方式不可用    
alert(data['0']);    
alert(data[0]);    //注意此写法与数组用下标访问是相同的    
alert(data.length)  //貌似数组的长度   


        1.使用json时，通常都使用第一种方式，且key一般应使用合法的变量名(字母或下划线开头的包括字母、下划线和数字的字符串) 

        2.对象的两种访问方式：data.key和data[’key’]各自有自己的应用场景，一般情况使用data.key即可，也比较直观(它符合其它高级语言中访问对象中属性的方式)； 

        当key为一个变量时，并且使用在循环中，用data['key']这种方式。 
Javascript代码  收藏代码
for(var i=0; i < 10; i++) {    
  s += data['key' + i];  //循环调用，可简化代码    
}  

        3.第三种采用数字做key的方式，虽然不推荐，但也是有其应用价值的； 

        如当建立一个与数据库中id一一对应的map对象的时候， 

        可直接用id的数值做key，虽然你可以给它加上一个字母前缀来让它符合合法的变量名的标准， 

        并让它的数据能通过data.key的方式访问， 

        但如果数据量非常大的话， 

        为每个id都加一个前缀，＋字符连接运算也是要消耗性能的， 

        特别是在很少需要采用data.key方式去访问属性的情况下， 

        那么可以抛弃此调用方式，直接用数字做key也未尝不可， 

        除了key名称不符合合法变量名的标准之外，似乎并没有其它损失； 
## 参考
[JSON的key值为数字时如何使用](https://yuelangyc.iteye.com/blog/1416424)
