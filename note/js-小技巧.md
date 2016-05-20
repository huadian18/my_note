#使用更简单的类似indexOf的包含判断方式

原生的JavaScript没有contains方法。对检查字符串或字符串数组项中是否存在某值，你可以这样做：

    var someText = 'JavaScript rules';

    if (someText.indexOf('JavaScript') !== -1) {

    }

    // 或者

    if (someText.indexOf('JavaScript') >= 0) {

    }

但是我们再看看这些ExpressJs代码片段。

    examples/mvc/lib/boot.js

    for (var key in obj) {

    // "reserved" exports

    if (~['name', 'prefix', 'engine', 'before'].indexOf(key)) continue;

    lib/utils.js

    exports.normalizeType = function(type){

    return ~type.indexOf('/')

    ? acceptParams(type)

    : { value: mime.lookup(type), params: {} };

    };

    examples/web-service/index.js

    // key is invalid

    if (!~apiKeys.indexOf(key)) return next(error(401, 'invalid api key'));

问题是~位运算符。"运算符执行操作这样的二进制表达式，但他们返回标准的JavaScript的数值."

他们将-1转换为0，而0在JavaScript中又是false。

    var someText = 'text';

    !!~someText.indexOf('tex'); // someText 包含 "tex" - true

    !~someText.indexOf('tex'); // someText 不包含 "tex" - false

    ~someText.indexOf('asd'); // someText 不包含 "asd" - false

    ~someText.indexOf('ext'); // someText 包含 "ext" - true

    String.prototype.includes

在ES6(ES 2015)中介绍了includes方法可以用来确定是否一个字符串包含另一个字符串：

    'something'.includes('thing'); // true

在ECMAScript 2016 (ES7)中，甚至数组都可以这样操作，如indexOf：

    !!~[1, 2, 3].indexOf(1); // true

    [1, 2, 3].includes(1); // true

不幸的是，这只是在Chrome，Firefox，Safari 9或以上的浏览器中被支持。

#14 - arrow 函数

介绍下ES6里的新功能，arrow函数可能会是个很方便的工具，用更少行数写更多代码。他的名字来源于他的语法，=>和小箭头->比就像一个“胖胖的箭头”。可能有些人知道，这种函数类型和其他静态语言如lambda表达式的匿名函数。它被称为匿名，因为这些箭头函数没有一个描述性的函数名。

那么这样有什么好处呢？

语法：更少的LOC，不用一次次的键入函数关键字。

语义：从上下文中捕捉关键字this。

简单语法案例：

看看下面的两段代码片段，他们做的是一样的工作。你能很快的理解arrow函数的功能。

    // arrow函数的日常语法

    param => expression

    // 可能也会写在括号中

    // 括号是多参数要求

    (param1 [, param2]) => expression

    // 使用日常函数

    var arr = [5,3,2,9,1];

    var arrFunc = arr.map(function(x) {

    return x * x;

    });

    console.log(arr)

    // 使用arrow函数

    var arr = [5,3,2,9,1];

    var arrFunc = arr.map((x) => x*x);

    console.log(arr)

正如你所看到的，这个例子中的arrow函数可以节省你输入括号内参数和返回关键字的时间。建议把圆括号内的参数输入，如 (x,y) => x+y 。在不同的使用情况下，它只是用来应对遗忘的一种方式。但是上面的代码也会这样执行：x => x*x.目前看来，这些仅仅是导致更少的LOC和更好的可读性的句法改进。

this 绑定

还有一个更好的理由使用arrow函数。那就是在会出现this问题的背景下。使用arrow函数，你就不用担心.bind(this)和 that=this 了。因为arrow函数会从上下文中找到this。看下面的例子：

    // 全局定义this.i

    this.i = 100;

    var counterA = new CounterA;

    var counterB = new CounterB;

    var counterC = new CounterC;

    var counterD = new CounterD;

    // 不好的示例

    function CounterA {

    // CounterA's `this` 实例 (!! 忽略这里)

    this.i = 0;

    setInterval(function {

    // `this` 指全局对象，而不是 CounterA's `this`

    // 因此，开始计数与100，而不是0 (本地的 this.i)

    this.i++;

    document.getElementById("counterA").innerHTML = this.i;

    }, 500);

    }

    // 手动绑定 that = this

    function CounterB {

    this.i = 0;

    var that = this;

    setInterval(function {

    that.i++;

    document.getElementById("counterB").innerHTML = that.i;

    }, 500);

    }

    // 使用 .bind(this)

    function CounterC {

    this.i = 0;

    setInterval(function {

    this.i++;

    document.getElementById("counterC").innerHTML = this.i;

    }.bind(this), 500);

    }

    // 使用 arrow函数

    function CounterD {

    this.i = 0;

    setInterval( => {

    this.i++;

    document.getElementById("counterD").innerHTML = this.i;

    }, 500);

    }

关于arrow函数的进一步信息可以在MDN：https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions。查看不同的语法选请访问该站点：http://jsrocks.org/2014/10/arrow-functions-and-their-scope/。

#13 - 测量一个JavaScript代码块性能的技巧

快速测量一个JavaScript块的性能，我们可以使用控制台的功能像console.time(label)和console.timeEnd(label)

    console.time("Array initialize");

    var arr = new Array(100),

    len = arr.length,

    i;

    for (i = 0; i < len; i++) {

    arr[i] = new Object;

    };

    console.timeEnd("Array initialize"); // 输出: Array initialize: 0.711ms

更多信息Console object, JavaScript benchmarking

https://github.com/DeveloperToolsWG/console-object

https://mathiasbynens.be/notes/JavaScript-benchmarking

demo：jsfiddle-codepen (在浏览器控制台输出) http://codepen.io/anon/pen/JGJPoa

#12 - ES6中参数处理

在许多编程语言中，函数的参数是默认的，而开发人员必须显式定义一个参数是可选的。在JavaScript中的每个参数是可选的，但我们可以这一行为而不让一个函数利用ES6的默认值作为参数。http://exploringjs.com/es6/ch_parameter-handling.html#sec_parameter-default-values

    const _err = function( message ){

    throw new Error( message );

    }

    const getSum = (a = _err('a is not defined'), b = _err('b is not defined')) => a + b

    getSum( 10 ) // throws Error, b is not defined

    getSum( undefined, 10 ) // throws Error, a is not defined

_err是立即抛出一个错误的函数。如果没有一个参数作为值，默认值是会被使用，_err将被调用，将抛出错误。你可以在Mozilla开发者网络看到的更多默认参数的例子。https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/default_parameters

#11 - 提升

理解提升将帮助你组织你的function。只需要记住，变量声明和定义函数会被提升到顶部。变量的定义是不会的，即使你在同一行中声明和定义一个变量。此外，变量声明让系统知道变量存在，而定义是将其赋值给它。https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var#var_hoisting

    function doTheThing {

    // 错误: notDeclared is not defined

    console.log(notDeclared);

    // 输出: undefined

    console.log(definedLater);

    var definedLater;

    definedLater = 'I am defined!'

    // 输出: 'I am defined!'

    console.log(definedLater)

    // Outputs: undefined

    console.log(definedSimulateneously);

    var definedSimulateneously = 'I am defined!'

    // 输出: 'I am defined!'

    console.log(definedSimulateneously)

    // 输出: 'I did it!'

    doSomethingElse;

    function doSomethingElse{

    console.log('I did it!');

    }

    // 错误: undefined is not a function

    functionVar;

    var functionVar = function{

    console.log('I did it!');

    }

    }

为了使事情更容易阅读，在函数作用域内提升变量的声明将会让你明确该变量的声明是来自哪个作用域。在你需要使用变量之前定义它们。在作用域底部定义函数，确保代码清晰规范。

#10 - 检查一个对象是否有属性

当你要检查一个对象是否存在某个属性时，你可能会这样做：https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_Objects

    var myObject = {

    name: '@tips_js'

    };

    if (myObject.name) { ... }

这是可以的，但你必须知道这个还有两原生的方式，in operator和object.hasownproperty，每个对象是对象，既可用方法。每个object都继承自Object，这两个方法都可用。

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/in

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty

一些不同点

    var myObject = {

    name: '@tips_js'

    };

    myObject.hasOwnProperty('name'); // true

    'name' in myObject; // true

    myObject.hasOwnProperty('valueOf'); // false, valueOf 是从原型链继承的

    'valueOf' in myObject; // true

他们之间的不同在于检查的性质，换句话说，当该对象本身有查找的属性时hasOwnProperty返回yrue，然而，in operator不区分属性创建的对象和属性继承的原型链。

这里有另外一个例子：

    var myFunc = function {

    this.name = '@tips_js';

    };

    myFunc.prototype.age = '10 days';

    var user = new myFunc;

    user.hasOwnProperty('name'); // true

    user.hasOwnProperty('age'); // false, 因为age是原型链上的

在这里看例子：https://jsbin.com/tecoqa/edit?js,console

同时，建议在检查对象的属性存在时，阅读这些有关的常见错误。https://github.com/loverajoel/jstips/issues/62

#09 - 模板字符串

截至ES6，JS已经有模板字符串作为替代经典的结束引用的字符串。

案例：普通字符串

    var firstName = 'Jake';

    var lastName = 'Rawr';

    console.log('My name is ' + firstName + ' ' + lastName);

    // My name is Jake Rawr

    模板字符串：

    var firstName = 'Jake';

    var lastName = 'Rawr';

    console.log(`My name is ${firstName} ${lastName}`);

    // My name is Jake Rawr

在模板字符串中${}中，你可以写不用写/n或者简单逻辑来实现多行字符串。

您还可以使用函数来修改模板字符串的输出，它们被称为模板字符串的标记。https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/template_strings#Tagged_template_strings

你可能还想读到更多的理解模板字符串相关信息。https://hacks.mozilla.org/2015/05/es6-in-depth-template-strings-2

#08 - 将节点列表转换为数组

querySelectorAll 方法返回一个和数组类似的节点列表对象。这些数据结构类似数组，因为经常以数组形式出现，但是又不能用数组的方法，比如map和foreach。这里是一个快速、安全、可重用的方式将一个节点列表到一个DOM元素数组：

    const nodelist = document.querySelectorAll('div');

    const nodelistToArray = Array.apply(, nodelist);

    //later on ..

    nodelistToArray.forEach(...);

    nodelistToArray.map(...);

    nodelistToArray.slice(...);

    //etc...

apply方法是将一系列数组格式的参数传递给一个给定this的函数。MDN指出，apply将会调用类似数组的对象，而这正是querySelectorAll所返回的。因为我们不需要在函数的上下文中指定this，所以我们传入或0。返回的结果是一组能使用数组方法的DOM元素数组。https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply

如果你使用的是es2015可以利用...(spread operator)https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator

const nodelist = [...document.querySelectorAll('div')]; // 返回的是个真实的数组

    //later on ..

    nodelist.forEach(...);

    nodelist.map(...);

    nodelist.slice(...);

    //etc...

#07 - "use strict" 和懒惰

严格模式的JavaScript让开发人员更加安全的编写JavaScript。

默认情况下，JavaScript允许开发者懒惰，例如，我们在第一次声明变量的时候可以不用var，虽然这可能看起来像一个没有经验的开发人员，同时这也是很多错误的根源，变量名拼写错误或意外地将它提到了外部作用域。

程序员喜欢让电脑为我们做些无聊的事，检查一些我们工作的错误。"use strict"指令我们做这些，将我们的错误转换成JavaScript的错误。

我们把这个指令可以通过添加在一个js文件的顶部：

    // 整个script文件都将是严格模式语法

    "use strict";

    var v = "Hi! I'm a strict mode script!";

或者在函数内：

    function f

    {

    // 函数范围内的严格模式语法

    'use strict';

    function nested { return "And so am I!"; }

    return "Hi! I'm a strict mode function! " + nested;

    }

    function f2 { return "I'm not strict."; }

在包含这个指令的JavaScript文件或者函数内，我们将一些较大的JavaScript项目中的不良行为直接在JavaScript引擎执行中禁止了。在其他情况中，严格模式改变以下的行为：

· 变量只有在前面 var 声明了才能用

· 试图写入只读属性产生的误差

· 必须用 new 关键字调用构造函数

· this 不会默认指向全局对象

· 非常有限的使用eval

· 保护保留字符或未来保留字符不被作为变量名使用

严格模式在新项目中是很有好处的，但是在大多数地方没使用到它的老项目里使用它是非常具有挑战性的。当你把多个文件合并到一个文件时，它也是个问题，就像可能导致整个文件都在严格模式下执行。

它不是一个声明，只是一个字面量，早期版本的浏览器会忽略它。严格模式支持：

· IE 10+

· FF 4+

· Chrome 13+

· Safari 5.1+

· Opera 12+

参阅MDN对于严格模式的描述。https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode

#06 - 处理一个数组或单个元素作为参数的方法

相比于写个单独的方法去分别操作一个数组和一个元素作为参数的函数，更好的是写一个通用的函数，这样就都可以操作。这类似于一些jQuery的方法(css匹配将修改所有的选择器)。

你仅需要先将一切放进数组，Array.concat会接收数组或单一的对象：

    function printUpperCase(words) {

    var elements = .concat(words);

    for (var i = 0; i < elements.length; i++) {

    console.log(elements[i].toUpperCase);

    }

    }

    printUpperCase现在可以接收无论单一的元素作为参数还是一个数组：

    printUpperCase("cactus");

    // => CACTUS

    printUpperCase(["cactus", "bear", "potato"]);

    // => CACTUS

    // BEAR

    // POTATO

#05 - undefined 和 的不同

· undefined指的是一个变量未被声明，或者一个变量被声明但未赋值

· 是指一个特定的值，即"没有值"

. JavaScript给未赋值的变量默认定义为undefined

· JavaScript不会给未赋值的变量设置值，它被程序员用来表示一个无价值的值

· undefined在json格式数据中是无效的，而有效

· undefined 类型是 undefined

· 类似是object.为什么呢？http://www.2ality.com/2013/10/typeof-.html

· 两者都是原始值

· 两者都被认为false(Boolean(undefined) // false, Boolean // false)。

· 辨认变量是不是undefined

typeof variable === "undefined"

· 检查变量是不是

variable === ""

从值考虑他们是相等的，但是从类型和值共同考虑他们是不相等的

== undefined // true

=== undefined // false

#04 - 以非ASCII字符形式来排序字符串

JavaScript有个原生的方法对字符串格式的数组进行排序，做一个简单的array.sort将会把字符串们按首字母的数序排列。当然，也可以提供自定义排序功能。

['Shanghai', 'New York', 'Mumbai', 'Buenos Aires'].sort;

// ["Buenos Aires", "Mumbai", "New York", "Shanghai"]

当你试图用非ASCII字符，如 ['é', 'a', 'ú', 'c']这样的进行排序，你会得到一个奇怪的结果['c', 'e', 'á', 'ú']，因为只有用英语的语言才能排序，所以发生这种情况。

看一个简单的例子：

    // Spanish

    ['único','árbol', 'cosas', 'fútbol'].sort;

    // ["cosas", "fútbol", "árbol", "único"] // 错误的排序

    // German

    ['Woche', 'wöchentlich', 'wäre', 'Wann'].sort;

    // ["Wann", "Woche", "wäre", "wöchentlich"] // 错误的排序

幸运的是，有两种方法来避免这种行为，ECMAScript国际化的API提供了localecompare和and Intl.Collator。

这两种方法都有自己的自定义参数，以便将其配置来充分的完成功能。

使用 localeCompare

    ['único','árbol', 'cosas', 'fútbol'].sort(function (a, b) {

    return a.localeCompare(b);

    });

    // ["árbol", "cosas", "fútbol", "único"]

    ['Woche', 'wöchentlich', 'wäre', 'Wann'].sort(function (a, b) {

    return a.localeCompare(b);

    });

    // ["Wann", "wäre", "Woche", "wöchentlich"]

    使用 intl.collator

    ['único','árbol', 'cosas', 'fútbol'].sort(Intl.Collator.compare);

    // ["árbol", "cosas", "fútbol", "único"]

    ['Woche', 'wöchentlich', 'wäre', 'Wann'].sort(Intl.Collator.compare);

    // ["Wann", "wäre", "Woche", "wöchentlich"]

· 每个方法都可以自定义位置

· 在FF浏览器，intl.collator会更快，当比较的是较大的数值或字符串

因此，当你在用英语以外的语言来赋值给字符串数组时，记得要使用此方法来避免意外的排序。

#03 - 改善嵌套条件

我们怎样才能改善并且使JavaScript中的if语句做出更有效的嵌套。

    if (color) {

    if (color === 'black') {

    printBlackBackground;

    } else if (color === 'red') {

    printRedBackground;

    } else if (color === 'blue') {

    printBlueBackground;

    } else if (color === 'green') {

    printGreenBackground;

    } else {

    printYellowBackground;

    }

    }

一个改善的办法是用switch语句代替嵌套的if语句。虽然它是更加简洁，更加有序，但不推荐这样做，因为很难debug。这里指出原因：https://toddmotto.com/deprecating-the-switch-statement-for-object-literals/

    switch(color) {

    case 'black':

    printBlackBackground;

    break;

    case 'red':

    printRedBackground;

    break;

    case 'blue':

    printBlueBackground;

    break;

    case 'green':

    printGreenBackground;

    break;

    default:

    printYellowBackground;

    }

但是，当我们有多个判断条件的情况下呢？在这种情况下，如果我们想让它更加简洁，更加有序，我们可以使用switch。如果我们将true的作为一个参数传递给该switch语句，它可以让我们在每一个情况下放置一个条件。

    switch(true) {

    case (typeof color === 'string' && color === 'black'):

    printBlackBackground;

    break;

    case (typeof color === 'string' && color === 'red'):

    printRedBackground;

    break;

    case (typeof color === 'string' && color === 'blue'):

    printBlueBackground;

    break;

    case (typeof color === 'string' && color === 'green'):

    printGreenBackground;

    break;

    case (typeof color === 'string' && color === 'yellow'):

    printYellowBackground;

    break;

    }

但我们必须避免在每一个条件下进行多次检查，尽量避免使用switch。我们也必须考虑到最有效的方法是通过一个object。

    var colorObj = {

    'black': printBlackBackground,

    'red': printRedBackground,

    'blue': printBlueBackground,

    'green': printGreenBackground,

    'yellow': printYellowBackground

    };

    if (color in colorObj) {

    colorObj[color];

    }

这里有更多相关的信息。http://www.nicoespeon.com/en/2015/01/oop-revisited-switch-in-js/

#02 - ReactJs 子级构造的keys是很重要的

keys是代表你需要传递给动态数组的所有组件的一个属性。这是一个独特的和指定的ID，react用它来标识每个DOM组件以用来知道这是个不同的组件或者是同一个组件。使用keys来确保子组件是可保存的并且不是再次创造的，并且防止怪异事情的产生。

· 使用已存在的一个独立的对象值

· 定义父组件中的键，而不是子组件

    //不好的

    ...

    render {

    <div key={{item.key}}>{{item.name}}</div>

    }

    ...

    //好的

    <MyComponent key={{item.key}}/>

· 使用数组所以不是个好习惯 https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318#.76co046o9

    · random从不会执行

    //不好的

    <MyComponent key={{Math.random}}/>

· 你可以创建你的唯一id，请确保该方法是快速的并已经附加到对象上的

· 当子级的数量是庞大的或包含复杂的组件，使用keys来提高性能

· 你必须为所有的子级ReactCSSTransitionGroup提供key属性http://docs.reactjs-china.com/react/docs/animation.html

#01 - AngularJs： $digest vs $apply

AngularJs最让人欣赏的特点是双向数据绑定。为了是它工作，AngularJs评估模型的变化和视图的循环($digest)。你需要了解这个概念，以便了解框架是如何在引擎中工作的。

Angular评估每时的每个事件的变化。这就是$digest循环。有时你必须手动运行一个新的循环，你必须有正确的选择，因为这个阶段是性能方面表现出最具影响力的。

$apply

这个核心方法让你来启动一个$digest循环。这意味着所以的watch列表中的对象都将被检查，整个应用程序启动了$digest循环。在内部，执行可选的函数参数之后，调用$rootScope.$digest;

$digest

在这种情况下，$digest方法在当前作用域和它的子作用域执行，你应该注意到，父级的作用域将不被检查，并没有受到影响。

建议：

· 只在浏览器DOM事件在Angular之外被触发的时候使用$apply或者$digest

· 给$apply传递函数表达式，这有一个错误处理机制，允许在消化周期中整合变化。

    $scope.$apply( => {

    $scope.tip = 'Javascript Tip';

    });

· 如果你仅仅想更新当前作用域或者他的子作用域，用$digest，并且防止整个应用程序的$digest。性能不言而喻咯。

· 当$apply有很多东西绑定时，这对机器来说是个艰难的过程，可能会导致性能问题。

· 如果你使用的是Angular 1.2.x以上的，使用$evalAsync。这是一个在当前循环或下一次循环的期间或对表达式做出评估的核心方法，这可以提高你的应用程序的性能。

#00 - 在数组插入一个项

将一个项插入到现有数组中，是一个日常常见的任务，你可以使用push在数组的末尾添加元素，使用unshift在开始的位置，或者在中间使用splice。

这些都是已知的方法，但这并不意味着没有一个更高性能的途径。我们来看一看。

在数组的末尾添加一个元素很容易与push，但还有一个更高性能的途径。

    var arr = [1,2,3,4,5];

    arr.push(6);

    arr[arr.length] = 6; //在Chrome 47.0.2526.106 (Mac OS X 10.11.1)上提高了 43% 的速度

这两种方法都修改了数组，不相信我？看这个jsperf http://jsperf.com/push-item-inside-an-array

现在，如果我们正在尝试将一个项目添加到数组的开头：

    var arr = [1,2,3,4,5];

    arr.unshift(0);

    [0].concat(arr); //在Chrome 47.0.2526.106 (Mac OS X 10.11.1)上提高了 98% 的速度

这里更详细一点：unshift编辑原有的数组，concat返回一个新数组。jsperf http://jsperf.com/unshift-item-inside-an-array

添加在阵列中的物品很容易使用splice，它是做它的最高效的方式。

    var items = ['one', 'two', 'three', 'four'];

    items.splice(items.length / 2, 0, 'hello');

我试着在不同的浏览器和操作系统中运行这些测试，结果是相似的。我希望这些建议对你有用，鼓励你自己做测试

英文原文地址：https://github.com/loverajoel/jstips