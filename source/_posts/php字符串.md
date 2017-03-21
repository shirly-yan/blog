---
title: php字符串
date: 2016-10-14 17:19:35
categories: php
---
php字符串
<!-- more -->

<h2>字符串的定义</h2>
一个字符串 通过下面的3种方法来定义：
1、单引号
2、双引号
3、heredoc语法结构

```php
//单引号定义的字符串
$hello = 'hello world';
//双引号定义的字符串
$hello = "hello world";
//heredoc语法结构定义的字符串
$hello = <<<TAG
hello world
TAG;
```

PHP允许我们在双引号串中直接包含字串变量。
而单引号串中的内容总被认为是普通字符。
比如：

```php
$str='hello';
echo "str is $str"; //运行结果: str is hello
echo 'str is $str'; //运行结果: str is $str
```

<h2>字符串的连接</h2>
PHP中用英文的点号.来连接两个字符串。

```php
$hello='hello';

$world=' world';

$hi = $hello.$world;

echo $hi;//我们可以用echo函数输出一下这个字符串连接。
```

<h2>去除字符串首尾的空格</h2>
PHP中有三个函数可以去掉字符串的空格
+ trim去除一个字符串两端空格。
+ rtrim是去除一个字符串右部空格，其中的r是right的缩写。
+ ltrim是去除一个字符串左部空格，其中的l是left的缩写。
 
例子如下：

```php
echo trim(" 空格 ")."<br>";
echo rtrim(" 空格 ")."<br>";
echo ltrim(" 空格 ")."<br>";
```

<h2>获取字符串的长度</h2>
函数strlen()

```php
$str = 'hello';
$len = strlen($str);
echo $len;//输出结果是5
```
strlen函数对于计算英文字符是非常的擅长，但是如果有中文汉字，要计算长度该怎么办？
可以使用mb_strlen()函数获取字符串中中文长度。
例子如下：

```php
$str = "我爱你";
echo mb_strlen($str,"UTF8");//结果：3，此处的UTF8表示中文编码是UTF8格式，中文一般采用UTF8编码
```

<h2>字符串的截取</h2>
+ 英文字符串的截取函数substr()
+ 中文字符串的截取函数mb_substr()

英文字符串的截取函数substr()
函数说明：substr(字符串变量,开始截取的位置,截取个数）
例如：

```php
$str='i love you';
//截取love这几个字母
echo substr($str, 2, 4);
//为什么开始位置是2呢，因为substr函数计算字符串位置是从0开始的，也就是0的位置是i,1的位置是空格，l的位置是2。从位置2开始取4个字符，就是love。
```
中文字符串的截取函数mb_substr()
函数说明：mb_substr(字符串变量,开始截取的位置，截取个数, 网页编码）
例如：

```php
$str='我爱你，中国';
//截取中国两个字
echo mb_substr($str, 4, 2, 'utf8');
//为什么开始位置是4呢，和上一个例子一样，因为mb_substr函数计算汉字位置是从0开始的，也就是0的位置是我,1的位置是爱，4的位置是中。从位置4开始取2个汉字，就是中国。中文编码一般是utf8格式
```

<h2>查找字符串</h2>
查找字符串函数strpos();
函数说明：strpos(要处理的字符串, 要定位的字符串, 定位的起始位置[可选])
例子：

```php
$str = 'I want to study at imooc';
$pos = strpos($str, 'imooc');
echo $pos;//结果显示19，表示从位置0开始，imooc在第19个位置开始出现
```

<h2>替换字符串</h2>
替换函数str_replace()
函数说明：str_replace(要查找的字符串, 要替换的字符串, 被搜索的字符串, 替换进行计数[可选])
例子：

```php
$str = 'I want to learn js';
$replace = str_replace('js', 'php', $str);
echo $replace;//结果显示I want to learn php
```

<h2>格式化字符串</h2>
格式化字符串函数sprintf()
函数说明：sprintf(格式, 要转化的字符串)
返回：格式化好的字符串
例子：

```php
$str = '99.9';
$result = sprintf('%01.2f', $str);
echo $result;//结果显示99.90
```

<h2>字符串的合并</h2>
字符串合并函数implode()
函数说明：implode(分隔符[可选], 数组)
返回值：把数组元素组合为一个字符串
例子：

```php
$arr = array('Hello', 'World!');
$result = implode('', $arr);
print_r($result);//结果显示Hello World!
```
<h2>字符串的分隔</h2>
字符串分隔函数explode()
函数说明：explode(分隔符[可选], 字符串)
返回值：函数返回由字符串组成的数组
例子：

```php
$str = 'apple,banana';
$result = explode(',', $str);
print_r($result);//结果显示array('apple','banana')
```

<h2>字符串的转义</h2>
字符串转义函数addslashes()
函数说明：用于对特殊字符加上转义字符，返回一个字符串
返回值：一个经过转义后的字符串
例子：

```php
$str = "what's your name?";
echo addslashes($str);//输出：what\'s your name?
```









<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
