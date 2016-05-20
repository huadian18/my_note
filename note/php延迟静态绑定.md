#php延迟静态绑定

    php5.3新添特性

[TOC]

```php
    class A  
    {  
        public static function echoClass()  
        {  
            echo __CLASS__;  
        }  
      
        public static function test()  
        {  
            self::echoClass();        
        }  
    }  
      
    class B extends A  
    {        
        public static function echoClass()  
        {  
             echo __CLASS__;  
        }  
    }  
      
    B::test(); //输出A  
```

    延迟静态绑定,就是把本来在定义阶段固定下来的表达式或变量,改在执行阶段才决定
比如当一个子类继承了父类的静态表达式的时候,它的值并不能被改变,有
时不希望看到这种情况
下面的例子说明了延迟静态绑定的作用:

```php
    class A  
    {  
        public static function echoClass()  
        {  
            echo __CLASS__;  
        }  
      
        public static function test()  
        {  
            static::echoClass();        
        }  
    }  
      
    class B extends A  
    {        
        public static function echoClass()  
        {  
             echo __CLASS__;  
        }  
    }  
      
    B::test(); //输出B  
```

实例：
```php
class A {
 protected static $def = '123456';
 
 public static function test() {
  echo get_class(new static);
 }
 
 public static function test2() {
  echo static::$def;
 }
}
 
class B extends A {
 protected static $def = '456789';
}
 
class C extends A {
 protected static $def = 'abcdef';
}
 
echo B::test();
echo '<br>';
echo C::test();
echo '<br>';
echo B::test2();
echo '<br>';
echo C::test2();
echo '<br>';
echo A::test();
echo '<br>';
echo A::test2();
echo '<br>';

```

// 输出结果:

    B
    C
    456789
    abcdef
    A
    123456

##get_called_class
php函数：>php5.3
`get_called_class`后期静态绑定（"Late Static Binding"）类的名称
**返回值**：返回类的名称，如果不是在类中调用则返回 FALSE 

