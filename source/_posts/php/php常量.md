---
title: php常量
date: 2016-10-12 16:20:51
categories: [php]
---
php常量的分类和定义
<!-- more -->
常量可以理解为值不变的量（如圆周率）；或者是常量值被定义后，在脚本的其他任何地方都不可以被改变。
PHP中的常量分为
+ 自定义常量
+ 系统常量
常量主要功效是可以避免重复定义，篡改变量值。在我们进行团队开发时，或者代码量很大的时候，对于一些第一次定义后不改变的量，如果我们使用变量，在不知情的情况下，使用同一变量名时，变量值就会被替换掉，从而会引发服务器执行错误的任务。

<h2>自定义常量</h2>
```php
define()函数的语法格式为：
bool define(string $constant_name, mixed $value[, $case_sensitive = true])
//参数
//“constant_name”  必选参数 常量名称
//即标志符，常量的命名规则与变量的一致，但是要注意哦，它可不带美元符号哦。
//“value”          必选参数 常量的值
//“case_sensitive” 可选参数 指定是否大小写敏感，
//设定为true表示不敏感，一般不指定第三个参数的情况下，默认第三个参数的值为false。
（注： string表示参数类型为字符串类型，mixed表示参数类型可以接受为多种不同的类型，case_sensitive = true表示默认为布尔类型TRUE）

<?php
define("PI",3.14);
echo PI;
echo "<br />";

$p = "PII";
define($p,3.14);
echo PII;
?>

结果
3.14
3.14
```

<h2>系统常量</h2>
系统常量是PHP已经定义好的常量，我们可以直接拿来使用，常见的系统常量有：
（1）__FILE__ :php程序文件名。它可以帮助我们获取当前文件在服务器的物理位置。
（2）__LINE__ :PHP程序文件行数。它可以告诉我们，当前代码在第几行。
（3）PHP_VERSION
:当前解析器的版本号。它可以告诉我们当前PHP解析器的版本号，我们可以提前知道我们的PHP代码是否可被该PHP解析器解析。
（4）PHP_OS
：执行当前PHP版本的操作系统名称。它可以告诉我们服务器所用的操作系统名称，我们可以根据该操作系统优化我们的代码。

```php
<?php
echo __FILE__;
echo "<br />";
echo __LINE__;
echo "<br />";
echo PHP_VERSION;
echo "<br />";
echo PHP_OS;
echo "<br />";
?>

结果
/Library/WebServer/Documents/test.php
13
5.5.36
Darwin
```

<h2>常量取值</h2>
+ 常量名直接获取值
+ 使用constant()函数
它和直接使用常量名输出的效果是一样的，但函数可以动态的输出不同的常量，在使用上要灵活、方便，其语法格式如下

```php
<?php
define("PI",3.14);
$r=1;
$area = PI*$r*$r; //计算圆的面积
?>

<?php 
$p="";
//定义圆周率的两种取值
define("PI1",3.14);
define("PI2",3.142);
//定义值的精度
$height = "中";
//根据精度返回常量名，将常量变成了一个可变的常量
if($height == "中"){
    $p = "PI1";
}else if($height == "低"){
	$p = "PI2";
}
$r=1;
$area= constant($p)*$r*$r;
echo $area;
?>
```

<h2>如何判定常量是否被定义</h2>
defined()函数判断一个常量是否已经定义

```php
其语法格式为：
bool defined(string constants_name)


<?php 
define("PI1",3.14);
$p = "PI1";
$is1 = defined($p);
$is2 = defined("PI2");
var_dump($is1);
var_dump($is2);
?>

结果
bool(true) bool(false)
```


<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
