---
title: php数组
date: 2016-10-14 11:20:49
categories: [php]
---
php数组
<!-- more -->
PHP有两种数组：
+ 索引数组
+ 关联数组
索引和关联两个词都是针对数组的键而言的。
先介绍下索引数组，索引数组是指数组的键是整数的数组，并且键的整数顺序是从0开始，依次类推。

<h2>定义</h2>
```php
$arr = array();
//表示创建一个空数组，并把创建的空数组赋值给变量$arr。
```

<h2>索引数组</h2>
<h3>索引数组初始化</h3>
```php
<?php
//创建一个索引数组，索引数组的键是“0”，值是“苹果”
$fruit = array("苹果","香蕉","菠萝");
print_r($fruit);
?>

结果
Array
( 
	[0] => 苹果 
	[1] => 香蕉 
	[2] => 菠萝 
)
```
<h3>索引数组赋值</h3>
索引数组赋值有三种方式:
+ 第一种：用数组变量的名字后面跟一个中括号的方式赋值，当然，索引数组中，中括号内的键一定是整数。比如，$arr[0]='苹果';
+ 第二种：用array()创建一个空数组，使用=>符号来分隔键和值，左侧表示键，右侧表示值。当然，索引数组中，键一定是整数。比如，array('0'=>'苹果');
+ 第三种：用array()创建一个空数组，直接在数组里用英文的单引号'或者英文的双引号"赋值，数组会默认建立从0开始的整数的键。比如array('苹果');这个数组相当于array('0'=>'苹果');

<h3>索引数组访问</h3>
```php
<?php
//从数组变量$arr中，读取键为0的值
$arr = array('苹果','香蕉');
$arr0 = $arr[0];
if( isset($arr0) ) {print_r($arr0);}
?>

<?php
$fruit=array('苹果','香蕉','菠萝');
for($index=0; $index<3; $index++){
    echo '<br>数组第'.$index.'值是：'.$fruit[$index];
}
?>

<?php
$fruit=array('苹果','香蕉','菠萝');
foreach($fruit as $key=>$value){
    echo '<br>第'.$key.'值是：'.$value;
}
?>
```


<h2>关联数组</h2>
<h3>关联数组初始化</h3>
```php
<?php
//创建一个关联数组，关联数组的键“orange”，值是“橘子”
$fruit = array(
    'apple'=>"苹果",
    "banana"=>"香蕉",
    "pineapple"=>"菠萝",
    );
    print_r($fruit);
?>
```
<h3>关联数组赋值</h3>
+ 第一种：用数组变量的名字后面跟一个中括号的方式赋值，当然，关联数组中，中括号内的键一定是字符串。比如，$arr['apple']='苹果';
 
+ 第二种：用array()创建一个空数组，使用=>符号来分隔键和值，左侧表示键，右侧表示值。当然，关联数组中，键一定是字符串。比如，array('apple'=>'苹果');

<h3>关联数组访问</h3>
用数组变量的名字后跟中括号+键的方式来访问数组中的值，键使用单引号或者双引号括起来。

```php
<?php
//从数组变量$arr中，读取键为apple的值
$arr = array('apple'=>"苹果",'banana'=>"香蕉",'pineapple'=>"菠萝");
$arr0 = $arr['apple'];
if( isset($arr0) ) {print_r($arr0);}
?>
```
foreach循环可以将数组里的所有值都访问到，下面我们展示下，用foreach循环访问关联数组里的值。

```php
<?php
$fruit=array('apple'=>"苹果",'banana'=>"香蕉",'pineapple'=>"菠萝");
foreach($fruit as $key=>$value){
    echo '<br>键是：'.$key.'，对应的值是：'.$value;
}

?>
```

















<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
