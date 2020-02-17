#### Javascript设计模式|策略模式

策略模式
策略模式：要实现某一个功能，有多种方案可以选择。我们定义一系列的算法，把它们一个个封装起来，并且使它们可以相互转换。

策略模式是一种常用且有效的设计模式。

策略模式可以有效避免多重条件选择语句。

策略模式提供了对开放-封装原则的完美支持，将方法封装在独立的strategy中，使得它们易于切换，易于理解，易于扩展。

复用性高。

当然，策略模式也有一些缺点

增加了许多策略类或者策略对象。

要使用策略模式，必须了解所有的strategy，违反了最少知识原则。

计算奖金的例子， 不好的实现方式：
var calculateBonus = function(performanceLevel, salary) {
  if (performanceLevel === 'S') {
    return salary * 4;
  }
  if (performanceLevel === 'A') {
    return salary * 3;
  }
  if (performanceLevel === 'B') {
    return salary * 2;
  }
};
console.log(calculateBonus('B', 20000)); // 输出:40000 
console.log(calculateBonus('S', 6000)); // 输出:24000

在JavaScript语言中，函数也是对象，所以简单和直接的做法是把strategy直接定义为函数：

var strategies = {
  S: function(salary) {
    return salary * 4;
  },
  A: function(salary) {
    return salary * 3;
  },
  B: function(salary) {
    return salary * 2;
  }
};

var calculateBonus = function(level, salary) {
  return strategies[level](salary);
};
console.log(calculateBonus('S', 20000)); // 输出:80000 
console.log(calculateBonus('A', 10000)); // 输出:30000


参考：
JavaScript设计模式与开发实践
---
> 桥智科技：科技赋能梦想！专注广州、深圳和惠州小程序定制开发、APP 应用定制开发、网站开发、区块链钱包开发！