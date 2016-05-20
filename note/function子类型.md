学生在前面给大家介绍过，ECMAScript中的function有三种用法：作为对象使用、处理业务以及创建object类型的实例对象，跟这三种用法相对应的有三种子类型：对象的属性、变量和创建出来的object实例对象的属性，这三种子类型是相互独立的，而且也很容易区分，不过有些读者可能会因为不了解这一点，所以经常会将他们混淆，混淆之后就会带来不必要的错误，所以一定要将他们区分清楚，本节学生先来分别给大家介绍一下这三种子类型。

function作为对象来使用

这种情况下function对象的子类型就是对象自己的属性，这时通过点操作符“.”（或者方括号）来使用，比如下面的例子

    function book(){}

    book.price = 161.0;

    book.getPrice = function () {

    return this.price;

    }

    console.log(book.getPrice()); //161

在这种情况下function是作为object类型的对象来使用的，上面的例子中首先定义了function类型的book对象，然后给他添加了price属性和getPrice方法属性，这时就可以直接使用点操作符来对其进行操作了。

function用于处理业务

这种情况下function的子类型就是自己定义的局部变量（包括参数），这时的变量是在方法被调用时通过变量作用域链来管理的，变量作用域的相关内容我们在前面已经讲过了，如果您对变量作用域链的印象不深可以返回去查阅相关章节。

function用于创建对象

这种情况下对应的子类型是使用function所创建出来的对象实例的属性，主要包括在function中通过this添加的属性，以及创建完成之后实例对象自己添加的属性，另外还可以调用function的prototype属性对象中所包含的属性，比如我们前面用过的Car的例子

    function Car(color, displacement){

    this.color = color;

    this.displacement = displacement;

    }

    Car.prototype.logMessage = function(){

    console.log(this.color+","+this.displacement);

    }

    var car = new Car("black", "2.4T");

这时候创建出来的car对象就包含有color和displacement两个属性，而且还可以调用Car.prototype的logMessage方法属性。当然，创建完之后还可以使用点操作符给创建出来的car对象添加或者修改属性，也可以使用delete删除其中的属性，比如下面的例子

    function Car(color, displacement){

    this.color = color;

    this.displacement = displacement;

    }

    Car.prototype.logMessage = function(){

    console.log(this.color+","+this.displacement);

    }

    var car = new Car("black", "2.4T");

    car.logColor = function () {

    console.log(this.color);

    }

    car.logColor(); // black

    car.color = "red";

    car.logColor(); // red

    delete car.color;

    car.logColor(); //undefined

这里我们在创建完car对象后又给他添加了logColor方法，可以打印出car的color属性。添加完logColor方法后直接调用就可以打印出car原来的color属性值（black），然后我们将其修改为red之后再打印就打印出了red，最后我们使用delete删除掉car的color属性，这时再调用logColor方法时就会打印出undefined。

三种子类型的关系

相互独立

function的三种子类型是相互独立的，他们只能在自己所对应的环境中来使用而不可以相互调用，这一点大家一定要记住，我们来看个例子

    function log(msg){

    console.log(msg);

    }

    function Bird(){

    var name = "kitty";

    this.type = "pigeon";

    this.getName = function () {

    return this.name;

    }

    }

    Bird.color="white";

    Bird.getType = function () {

    return this.type;

    }

    Bird.prototype.getColor = function () {

    return this.color;

    }

    var bird = new Bird();

    log(bird.getColor()); // undefined

    log(bird.getName()); // undefined

    log(Bird.getType()); // undefined

这个例子中的最后三条语句都会打印出undefined，下面学生来给大家分析其中的原因。

Bird作为对象时会包含color和getType两个属性，作为处理业务的函数时包含一个name的局部变量，创建出来的实例对象bird具有type和getName两个属性，而且还可以调用Bird.prototype中的getColor属性，getColor也可以看做bird的属性，如下表所示

表4-2
用法  子类型
对象  color、getType
处理业务    name
创建对象    type、getName、（getColor）

每种用法中所定义的方法只能调用相应用法所对应的属性，而不能相互调用，从表中的对应关系可以看出getName、getColor 和getType 三个方法都获取不到对应的值，所以他们都会输出undefined。

另外，getName和getColor是bird的属性方法，getType是Bird的属性方法，如果使用Bird调用getName或getColor方法或者使用bird调用getType方法都会抛出找不到方法的错误。

对象的属性没有继承关系

除了三种子类型不可以相互调用之外还有一种情况也非常容易被误解，那就是对象的属性没有继承关系，我们看下面的例子

    function obj(){}

    obj.v = 1;

    obj.func = {

    logV : function(){

    console.log(this.v);

    }

    };

    obj.func.logV();

这里的obj是作为对象使用的，obj有一个属性v和一个对象属性func，func对象中又有一个logV方法，logV方法用于打印对象的v属性。logV方法打印的是func对象的v属性，但是func对象并没有v属性，所以最后会打印出undefined。这里虽然obj对象中包含v属性，但是由于属性不可以继承，所以obj的func属性对象中的方法不可以使用obj中的属性v，这一点大家一定要记住而不要和prototype的继承相混淆。

多知道点

JavaScript中的“公有属性”、“私有属性”以及“静态属性”等

在有些资料上可能会看到类似“公有属性”、“私有属性”以及“静态属性”等的名称，其实这些是基于类的语言（比如Java、C++等）中的一些概念，JavaScript并不是基于类的而是基于对象的语言，所以本身并没有这些概念，所谓的“公有属性”一般指的是使用function对象创建出来的object类型对象所拥有的属性，“私有属性”一般指function的内部变量，“静态属性”一般指function对象自己的属性。

当然，我们学习JavaScript的目的是为了使用他来实现我们的功能，而不是为了做理论上的研究，所以如果大家觉得使用那种叫法更容易理解也可以那么去叫。

关联三种子类型

ECMAScript中的三种子类本来是相互独立、各有各的使用环境的，但是有一些情况下需要操作不属于自己所对应环境的子类型，这时就需要使用一些技巧来实现了。

为了描述方便，我们将function作为对象使用记作O（Object），作为函数使用记作F（Function），创建出来的对象实例记作I（Instance），他们所对应的子类型分别记作op（object property）、v（variables）和ip（instance property），他们之间的调用方法如下表所示

表4-3

    op  v   ip
O   直接调用    在函数中关联到对象属性 不可调用
F   使用function对象调用  直接调用    不可调用
I   使用function对象调用  在函数中关联到实例属性 直接调用

这里的纵向表头表示function对象不同的用法，横向表头表示各种子类型，表格的主体表示在function相应用法中调用各种子类型的方法。因为function创建出来的实例对象在创建之前还不存在，所以function作为方法和作为对象使用时是无法调用function创建出来的实例对象的属性（ip）的。调用参数可以在函数中将变量关联到相应属性，调用function作为对象时的属性可以直接使用function对象来调用，我们看下面的例子

    function log(msg){

    console.log(msg);

    }

    function Bird(){

    var name = "kitty";

    var type = "pigeon";

    //将局部变量name关联到创建出来对象的getName、setName属性方法

    this.getName = function () {

    return name;

    }

    this.setName = function (n) {

    name = n;

    }

    //将局部变量type关联到Bird对象的getType属性方法

    Bird.getType = function () {

    return type;

    }

    //在业务处理中调用Bird对象的color属性

    log(Bird.color); //white，F调用op

    }

    Bird.color="white";

    //在创建出的实例对象中调用Bird对象的color属性

    Bird.prototype.getColor = function () {

    return Bird.color;

    }

    var bird = new Bird();

    log(bird.getColor()); //white，I调用op

    log(bird.getName()); //kitty，I调用v

    log(Bird.getType()); //pigeon，O调用v

    bird.setName("petter"); //I调用v

    log(bird.getName()); //petter，I调用v

例子中以及做了详细的注释，学生就不再详细解释了，通过这个例子我们就清楚了三种子类型在不同环境（用法）中相互调用的方式了。