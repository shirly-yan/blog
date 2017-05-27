---
title: php类
date: 2016-10-14 12:45:26
categories: [php]
---
php类
<!-- more -->

```php
<?php
//定义一个类
class Car {
    var $name = '汽车';
    function getName() {
        return $this->name;
    }
}

//实例化一个car对象
$car = new Car();
$car->name = '奥迪A6'; //设置对象的属性值
echo $car->getName();  //调用对象的方法 输出对象的名字
?>
```
<h2>属性</h2>
在类中定义的变量称之为属性，通常属性跟数据库中的字段有一定的关联，因此也可以称作“字段”。属性声明是由关键字 public，protected 或者 private 开头，后面跟一个普通的变量声明来组成。属性的变量可以设置初始化的默认值，默认值必须是常量。
访问控制的关键字代表的意义为：
+ public：公开的
+ protected：受保护的
+ private：私有的

```php
class Car {
    //定义公共属性
    public $name = '汽车';

    //定义受保护的属性
    protected $corlor = '白色';

    //定义私有属性
    private $price = '100000';
}
```
默认都为public，外部可以访问。一般通过->对象操作符来访问对象的属性或者方法，对于静态属性则使用::双冒号进行访问。当在类成员方法内部调用的时候，可以使用$this伪变量调用当前对象的属性。

```php
$car = new Car();
echo $car->name;   //调用对象的属性
echo $car->color;  //错误 受保护的属性不允许外部调用
echo $car->price;  //错误 私有属性不允许外部调用
```
受保护的属性与私有属性不允许外部调用，在类的成员方法内部是可以调用的。

```php
class Car{
    private $price = '1000';
    public function getPrice() {
        return $this->price; //内部访问私有属性
​    }
}
```
<h2>方法</h2>
方法就是在类中的function，很多时候我们分不清方法与函数有什么差别，在面向过程的程序设计中function叫做函数，在面向对象中function则被称之为方法。
同属性一样，类的方法也具有public，protected 以及 private 的访问控制。
访问控制的关键字代表的意义为：
public：公开的
protected：受保护的
private：私有的
我们可以这样定义方法：

```php
class Car {
    public function getName() {
        return '汽车';
    }
​}
$car = new Car();
echo $car->getName();
```
使用关键字static修饰的，称之为静态方法，静态方法不需要实例化对象，可以通过类名直接调用，操作符为双冒号::。

```php
class Car {
    public static function getName() {
        return '汽车';
    }
​}
echo Car::getName(); //结果为“汽车”
```
<h2>构造函数和析构函数</h2>
PHP5可以在类中使用__construct()定义一个构造函数，具有构造函数的类，会在每次对象创建的时候调用该函数，因此常用来在对象创建的时候进行一些初始化工作。

```php
class Car {
   function __construct() {
       print "构造函数被调用\n";
   }
}
$car = new Car(); //实例化的时候 会自动调用构造函数__construct，这里会输出一个字符串
```
在子类中如果定义了__construct则不会调用父类的__construct，如果需要同时调用父类的构造函数，需要使用parent::__construct()显式的调用。

```php
class Car {
   function __construct() {
       print "父类构造函数被调用\n";
   }
}
class Truck extends Car {
   function __construct() {
       print "子类构造函数被调用\n";
       parent::__construct();
   }
}
$car = new Truck();
```
同样，PHP5支持析构函数，使用__destruct()进行定义，析构函数指的是当某个对象的所有引用被删除，或者对象被显式的销毁时会执行的函数。

```php
class Car {
   function __construct() {
       print "构造函数被调用 \n";
   }
   function __destruct() {
       print "析构函数被调用 \n";
   }
}
$car = new Car(); //实例化时会调用构造函数
echo '使用后，准备销毁car对象 \n';
unset($car); //销毁时会调用析构函数
```
当PHP代码执行完毕以后，会自动回收与销毁对象，因此一般情况下不需要显式的去销毁对象。

<h2>重载</h2>
HP中的重载指的是动态的创建属性与方法，是通过魔术方法来实现的。属性的重载通过__set，__get，__isset，__unset来分别实现对不存在属性的赋值、读取、判断属性是否设置、销毁属性。

```php
class Car {
    private $ary = array();
    
    public function __set($key, $val) {
        $this->ary[$key] = $val;
    }
    
    public function __get($key) {
        if (isset($this->ary[$key])) {
            return $this->ary[$key];
        }
        return null;
    }
    
    public function __isset($key) {
        if (isset($this->ary[$key])) {
            return true;
        }
        return false;
    }
    
    public function __unset($key) {
        unset($this->ary[$key]);
    }
}
$car = new Car();
$car->name = '汽车';  //name属性动态创建并赋值
echo $car->name;
```

方法的重载通过__call来实现，当调用不存在的方法的时候，将会转为参数调用__call方法，当调用不存在的静态方法时会使用__callStatic重载。

```php
class Car {
    public $speed = 0;
    
    public function __call($name, $args) {
        if ($name == 'speedUp') {
            $this->speed += 10;
        }
    }
}
$car = new Car();
$car->speedUp(); //调用不存在的方法会使用重载
echo $car->speed;
```












<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
