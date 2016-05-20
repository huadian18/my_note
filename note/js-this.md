#学习this以及call,apply

[TOC]

###this

在《JavaScript设计模式与开发实践》关于this的描述中，this的核心要点：

    JavaScript的this总是指向一个对象

####this的调用方式
具体到实际应用中，this的指向又可以分为以下四种：

* 作为对象的方法调用
* 作为普通函数调用
* 构造器调用
* apply和call调用

接下来我们去剖析前3点，至于第4点的apply和call调用，会在call和apply部分详细讲解。

#####作为对象的方法调用
    说明：作为对象方法调用时，this指向该对象。

举例：
```javascript
/**
 * 1.作为对象的方法调用
 * 作为对象方法调用时，this指向该对象。
 */

var obj = {
  a: 1,
  getA: function {
    console.log(this === obj);
    console.log(this.a);
  }
};

obj.getA; // true , 1
```

#####作为普通函数调用
    说明：作为普通函数调用时，this总是指向全局对象(浏览器中是window)。

举例：
```javascript
/**
 * 2.作为普通函数调用
 * 不作为对象属性调用时,this必须指向一个对象。那就是全局对象。
 */

window.name = 'globalName';

var getName = function {
  console.log(this.name);
};

getName; // 'globalName'

var myObject = {
  name: "ObjectName",
  getName: function {
    console.log(this.name)
  }
};

myObject.getName; // 'ObjectName'

// 这里实质上是把function {console.log(this.name)}
// 这句话赋值给了theName。thisName在全局对象中调用，自然读取的是全局对象的name值
var theName = myObject.getName;

theName; // 'globalName'
```

#####构造器调用
    说明：作为构造器调用时，this指向返回的这个对象。

举例：
```javascript
/**
 * 3.作为构造器调用
 * 
 * 作为构造器调用时，this指向返回的这个对象。
 */

var myClass = function {
  this.name = "Lxxyx";
};

var obj = new myClass;

console.log(obj.name); // Lxxyx
console.log(obj) // myClass {name: "Lxxyx"}
```

但是如果构造函数中手动指定了return其它对象，那么this将不起作用。如果return的是别的数据类型，则没有问题。
```javascript
var myClass = function {
  this.name = "Lxxyx";
  // 加入return时，则返回的是别的对象。this不起作用。
  return {
    name:"ReturnOthers"
  }
};

var obj = new myClass;
console.log(obj.name); // ReturnOthers
```

###Call和Apply

Call和Apply的用途一样。都是用来指定函数体内this的指向。

####Call和Apply的区别

Call：第一个参数为this的指向，要传给函数的参数得一个一个的输入。Apply：第一个参数为this的指向，第二个参数为数组，一次性把所有参数传入。

    如果第一个参数为null,则this指向调用的本身。

#####改变this指向
    说明：这是call和apply最常用的用途了。用于改变函数体内this的指向。

举例：
```javascript
var name = "GlobalName"

var func = function {
  console.log(this.name)
};

func; // "GlobalName"

var obj = {
  name: "Lxxyx",
  getName: function {
    console.log(this.name)
  }
};

obj.getName.apply(window) // "GlobalName" 将this指向window
func.apply(obj) // "Lxxyx" 将this指向obj
```

#####借用其它对象的方法

这儿，我们先以一个立即执行匿名函数做开头：

```javascript
(function(a, b) {
  console.log(arguments) // 1,2
  // 调用Array的原型方法
  Array.prototype.push.call(arguments, 3);
  console.log(arguments) // 1,2,3
})(1,2)
```

    函数具有arguments属性，而arguments是一个类数组。

但是arguments是不能直接调用数组的方法的，所以我们要用call或者apply来调用Array对象的原型方法。

原理也很容易理解，比如刚才调用的是push方法，而push方法在谷歌的v8引擎中，源代码是这样的：

```javascript
function ArrayPush {
  var n = TO_UINT32(this.length); // 被push对象的长度
  var m = % _ArgumentsLength; // push的参数个数
  for (var i = 0; i < m; i++) {
    this[i + n] = % _Arguments(i); // 复制元素
  }
  this.length = n + m; //修正length属性
  return this.length;
}
```

它只与this有关，所以只要是类数组对象，都可以调用相关方法去处理。

###总结
1. 匿名函数，自执行函数里的this都是window 
2. 跟某一个元素绑定事件，当事件触发的时候，那个元素绑定的事件，this就是谁 
3. 函数执行的时候看前面的点，谁点就是谁 
4. 构造函数里的属性或方法，谁实例化this就是谁 
5. call和apply可以改变this关键字 改变的是Fuction原型上的关键字 