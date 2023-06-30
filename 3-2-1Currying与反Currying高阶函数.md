# 高阶函数
1.  函数可以作为参数被传递；
2.  函数可以作为返回值输出。

## 其他高阶函数应用

### 函数柯里化

::: 函数传值不会立马计算，而是会通过闭包把值存储，并可以继续传值，待到函数被真正求值才会吧所有参数一次性求值

1. 应用场景：假设我们要编写一个计算每月开销的函数。在每天结束之前，我们都要记录今天花掉了多少
钱。

```javascript
  var currying = function(fn) {
      var args = [];
      return function() {
          if (arguments.length === 0) {
              return fn.apply(this, args);
          } else {
              [].push.apply(args, arguments);
              return arguments.callee;
          }
      }
  };
  var cost = (function() {
      var money = 0;
      return function() {
          for (var i = 0, l = arguments.length; i < l; i++) {
              money += arguments[i];
          }
          return money;
      }
  })();
  var cost = currying(cost); // 转化成 currying 函数
  cost(100); // 未真正求值
  cost(200); // 未真正求值 
  cost(300); // 未真正求值
  alert(cost()); // 求值并输出：600
```

### uncurrying

::: 泛化 this 的过程提取出来

```javascript
  Function.prototype.uncurrying = function() {
      var self = this; // self 此时是 Array.prototype.push 
      return function() {
          var obj = Array.prototype.shift.call(arguments);
          // obj 是{ 
          // "length": 1, 
          // "0": 1 
          // } 
          // arguments 对象的第一个元素被截去，剩下[2] 
          return self.apply(obj, arguments);
          // 相当于 Array.prototype.push.apply( obj, 2 ) 
      };
  };
  // 使用
  var push = Array.prototype.push.uncurrying();
  var obj = {
      "length": 1,
      "0": 1
  };
  push(obj, 2);
  console.log(obj); // 输出：{0: 1, 1: 2, length: 2}

  // 另一种反柯里化实现方案
  
  ```
