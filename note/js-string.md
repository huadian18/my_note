

# Javascript复习笔记---字符串
String构造器可以使用new调用，也可以不使用，但是，这两种调用的结果也是完全不一样的。用new调用的时候，String作为构造器函数，创建字符串对象。不使用new的时候，String用作一个常规函数，将参数强制转为原始类型的字符串并且返回。字符串对象和字符串原始类型是不同的。你可以访问一个原始类型字符串的属性和方法，就像他是一个对象一样，但是你不可以往一个原始类型上面添加属性，虽然JavaScript引擎并不会报错，但是，你那样做是徒劳的，因为添加到原始类型上的属性是瞬间的，也就是说你添加了之后是获取不到的，永远都是undefined。实际上使用new来创建字符串对象是没有什么必要的，在项目中基本没有使用过。下面是一些字符串创建的例子：

[TOC]

####字符串
* 使用new创建字符串对象
```javascript
var s1 = new String("hello");
console.log(typeof s1); //"object"
```
* 没有new，创建原始类型的字符串
```javascript
var s2 = String("hello");
console.log(typeof s2); //"string"
```
* 最好是直接使用字面量的方法创建原始类型的字符串：
```javascript
var s3 = "hello";
console.log(typeof s3); //"string"
```
* 可以调用原始类型的方法和属性：
```javascript
console.log(s3.length); //5
```
* 但是不可以往原始类型上面添加属性，即使不提示错误
```javascript
s3.name = "fuck";
console.log(s3.name); //undefined
```

***

String.prototye原型上具有很多有用的方法，下面分别介绍一下：

####字符串的基本操作方法：

```javascript
var s = "JavaScript";
console.log(s.length); //10
console.log(s.indexOf("a")); //1
console.log(s.lastIndexOf("a")); //3
console.log(s.charAt(0)); //"J"
console.log(s.charCodeAt(0)); //74
console.log(s.toLowerCase()); //"javascript"
console.log(s.toLocaleLowerCase());
console.log(s.toUpperCase()); //"JAVASCRIPT"
console.log(s.toLocaleUpperCase()); //"JAVASCRIPT" 返回一个字符串，其中所有的字母字符都被转换为大写，同时适应宿主环境的当前区域设置。
s.concat(" rulz", "!"); //"JavaScript rulz!"
```
#####提取字符串：

* slice(begin, end);
* substring(begin, end);
* substr(begin, length);

slice和substring，他们用法相同，都是接受两个参数，分别是开始索引和结束索引。substr是非标准，它接受开始索引和截取长度。

```javascript
var s = "JavaScript";
s.slice(4, 7); //'Scr'
s.substring(4, 7); //'Scr'
s.substr(4, 3); //'Scr'
```

#####字符串比较：

使用localCompare，如果两个参数相等，返回0；如果参数较小，返回整数（这意味着参数将会排序在最初的字符串之前）；否则返回一个负数：

console.log("JavaScript".localeCompare("Java")); //1
console.log("JavaScript".localeCompare("JavaScriptz")); //-1
console.log("JavaScript".localeCompare("JavaScript")); //0

#####字符串转换转换成数组:

split根据一个分隔符字符串，将字符串分隔到一个数组里面，并且返回：

```javascript
var s = "a,b,c";
console.log(s.split(",")); //["a", "b", "c"]
```

他还允许分隔符是一个正则表达式：

```javascript
var s = "JavaScript";
console.log(s.split(/a/)); //["J", "v", "Script"]
```

正则表达式可解析随意分隔的CSV（comma-separated value，逗号隔开的值）数据。

如下例子中，因为逗号前后会有空格，但是我们不想要空格，如果我们直接使用逗号分隔的话，空格也会保留下来的，

```javascript
var s = "a ,b ,c ";
console.log(s.split(",")); //["a ", "b ", "c "]
```

这时候，正则表达式就用上场了：

```javascript
var s = "a ,b ,c ";
console.log(s.split(/\s*,\s*/)) //["a", "b", "c"]
```

#####字符串的搜索：

search方法接受一个参数，可以是字符串，也可以是正则表达式。如果，找到匹配结果，那么返回结果的索引（这点与indexOf有所不同），如果找不到，则返回-1：

```javascript
var s = "JavaScript";
console.log(s.search(/ava/)); //1
console.log(s.search("Java")); //0
console.log(s.search("JavaEE")); //-1
console.log(s.search(/Script/)); //4
```

#####字符串替换：

######replace：
首先，replace接受需要查找的一个正则表达式，如下，我们需要把所有的a替换成@，这要用到g修饰符修饰正则表达式

```javascript
var s = "JavaScript";
console.log(s.replace(/a/g, "@")); //"j@v@script"
```

如果，传递的是一个字符串，其内容会用作一个正则表达式的模式。但是，由于这个例子中，我们无法设置正则表达式模式的修饰符（如g,i,m），因此，只有一次会被替换：

```javascript
var s = "JavaScript";
console.log(s.replace("a", "@")); //"j@vaScript"
```

这是常见的错误原因之一，即使你只想替换第一次出现，也总是使用正则表达式来搜索，这是好习惯。
replace方法是不可以接受数组，当需要替换多个字符串的时候，可以连缀调用，如要把所有的a替换成@，所有s替换成$：

```javascript
var s = "JavaScript";
console.log(s.replace(/a/g, "@").replace(/s/gi, "$")); //J@v@$cript
```

replace强大之处在于，可以接受一个回调函数，而不是一个简单的字符串字面量作为替换。该回调函数有3个参数，分别是匹配、匹配的索引，以及最初输入的字符串，并且函数可以根据匹配进行有意义的或者带条件的替换，如下会使用html的实体代码来替换小写字母：
```javascript
var ents = "JavaScript".replace(/[a-z]/g, function(match, index, input) {
//match是a,然后是v，接着是a，以此类推
//index是match的索引，1, 2, 3, 4, 5, 6...
//input是"JavaScript"
return "&#".concat(match.charCodeAt(0), ";");
});
 
console.log(ents); //J&#97;&#118;&#97;S&#99;&#114;&#105;&#112;&#116;
```

######match：

用来匹配一个正则表达式模式，如下：

```javascript
var s = "JavaScript";
console.log(s.match(/[A-Z]/)); //["J"]
console.log(s.match(/[A-Z]/g)) //["J", "S"]
```

当该正则表达式没有使用g修饰符的时候，该方法和正则表达式的exec方法工作方式相同：

```javascript
console.log("string".match(/[a-z]/)); //["s"]
console.log(/[a-z]/.exec("string")); //["s"]
```

注意，当找不到匹配结果时，将会返回null，而不是空数组：

```javascript
"string".match(/[0-9]/); //null
/[0-9]/.exec("string"); //null
```
   

