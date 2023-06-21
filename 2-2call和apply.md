# call和apply的区别

::: call 是包装在 apply 上面的“一颗语法糖”，如果我们明确地知道函数接受多少个参数，而且想
一目了然地表达形参和实参的对应关系，那么也可以用 call 来传送参数。

1. 如果我们传入的第一个参数为 null，函数体内的 this 会指向默认的宿主对象，在浏览器中则是 window：-严格模式为null

## 1. call、apply修正this

```javascript
  document.getElementById = (function(func) {
      return function() {
          return func.apply(document, arguments);
      }
  })(document.getElementById);
  var getId = document.getElementById;
  var div = getId('div1');
  alert(div.id); // 输出： div1 
```

## 2. Function.prototype.bind

```javascript
Function.prototype.bind = function(context) {
    var self = this; // 保存原函数
    return function() { // 返回一个新的函数
        return self.apply(context, arguments); // 执行新的函数的时候，会把之前传入的 context 
        // 当作新函数体内的 this 
    }
};
var obj = {
    name: 'sven'
};
var func = function() {
    alert(this.name); // 输出：sven 
}.bind(obj);
func();

// 或者 
Function.prototype.bind = function() {
    var self = this, // 保存原函数
        context = [].shift.call(arguments), // 需要绑定的 this 上下文
        args = [].slice.call(arguments); // 剩余的参数转成数组
    return function() { // 返回一个新的函数
        return self.apply(context, [].concat.call(args, [].slice.call(arguments)));
        // 执行新的函数的时候，会把之前传入的 context 当作新函数体内的 this 
        // 并且组合两次分别传入的参数，作为新函数的参数
    }
};
var obj = {
    name: 'sven'
};
var func = function(a, b, c, d) {
    alert(this.name); // 输出：sven 
    alert([a, b, c, d]) // 输出：[ 1, 2, 3, 4 ] 
}.bind(obj, 1, 2);
func(3, 4);
```

## 3. 借用其他对象的方法

```javascript
  var A = function(name) {
      this.name = name;
  };
  var B = function() {
      A.apply(this, arguments);
  };
  B.prototype.getName = function() {
      return this.name;
  };
  var b = new B('sven');
  console.log(b.getName()); // 输出： 'sven'
```
