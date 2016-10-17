---
title: php静态关键字static
date: 2016-10-14 16:33:33
categories: php
---

<!-- more -->
静态属性与方法可以在不实例化类的情况下调用，直接使用类名::方法名的方式进行调用。静态属性不允许对象使用->操作符调用。

```php
class Car {
    private static $speed = 10;
    
    public static function getSpeed() {
        return self::$speed;
    }
}
echo Car::getSpeed();  //调用静态方法
```
静态方法也可以通过变量来进行动态调用

```php
$func = 'getSpeed';
$className = 'Car';
echo $className::$func();  //动态调用静态方法
```
静态方法中，$this伪变量不允许使用。可以使用self，parent，static在内部调用静态方法与属性。

```php
class Car {
    private static $speed = 10;
    
    public static function getSpeed() {
        return self::$speed;
    }
    
    public static function speedUp() {
        return self::$speed+=10;
    }
}
class BigCar extends Car {
    public static function start() {
        parent::speedUp();
    }
}

BigCar::start();
echo BigCar::getSpeed();
```
如果构造函数定义成了私有方法，则不允许直接实例化对象了，这时候一般通过静态方法进行实例化，在设计模式中会经常使用这样的方法来控制对象的创建，比如单例模式只允许有一个全局唯一的对象。

```php
class Car {
    private function __construct() {
        echo 'object create';
    }

    private static $_object = null;
    public static function getInstance() {
        if (empty(self::$_object)) {
            self::$_object = new Car(); //内部方法可以调用私有方法，因此这里可以创建对象
        }
        return self::$_object;
    }
}
//$car = new Car(); //这里不允许直接实例化对象
$car = Car::getInstance(); //通过静态方法来获得一个实例
```









<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
