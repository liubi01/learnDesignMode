# this

::: 具体指向哪个对象是在“运行时”基于函数的”执行环境“动态绑定的，而非函数被声明时的环境

## 1. 作为对象的方法调用。

::: 指向对象

```javascript
   var obj = {
       a: 1,
       getA: function() {
           alert(this === obj); // 输出：true 
           alert(this.a); // 输出: 1 
       }
   };
   obj.getA();
```

## 2. 作为普通函数调用

::: 指向全局window

```javascript
 window.name = 'globalName';
 var getName = function() {
     return this.name;
 };
 console.log(getName()); // 输出：globalName 
 //或者：
 window.name = 'globalName';
 var myObject = {
     name: 'sven',
     getName: function() {
         return this.name;
     }
 };
 var getName = myObject.getName;
 console.log(getName()); // globalName
 // 
 function func() {
     "use strict"
     alert(this); // 输出：undefined 
 }
 func();
```

## 3. 构造器调用

```javascript
 // 默认方式
 var MyClass = function() {
     this.name = 'sven';
 };
 var obj = new MyClass();
 alert(obj.name); // 输出：sven

 // 如果返回是一个对象的话,obj指向返回值
 var MyClass = function() {
     this.name = 'sven';
     return { // 显式地返回一个对象
         name: 'anne'
     }
 };
 var obj = new MyClass();
 alert(obj.name); // 输出：anne

 // 如果返回是一个非对象的话,和默认一样this
 var MyClass = function() {
     this.name = 'sven'
     return 'anne'; // 返回 string 类型
 };
 var obj = new MyClass();
 alert(obj.name); // 输出：sven 
```

## 4. Function.prototype.call 或 Function.prototype.apply 调用

::: 用 Function.prototype.call 或 Function.prototype.apply 可以动态地
改变传入函数的 this：

```javascript
   var obj1 = {
       name: 'sven',
       getName: function() {
           return this.name;
       }
   };
   var obj2 = {
       name: 'anne'
   };
   console.log(obj1.getName()); // 输出: sven 
   console.log(obj1.getName.call(obj2)); // 输出：anne 
```

## 丢失的this

::：判断执行环境,对象属性调用,普通函数调用
```javascript
  var obj = {
      myName: 'sven',
      getName: function() {
          return this.myName;
      }
  };
  console.log(obj.getName()); // 输出：'sven' 
  var getName2 = obj.getName;
  console.log(getName2()); // 输出：undefine
```
