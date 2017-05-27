---
title: php图形操作
date: 2016-10-17 13:58:32
categories: [php]
---
php图形操作
<!-- more -->
GD指的是Graphic Device，PHP的GD库是用来处理图形的扩展库，通过GD库提供的一系列API，可以对图像进行处理或者直接生成新的图片。
PHP除了能进行文本处理以外，通过GD库，可以对JPG、PNG、GIF、SWF等图片进行处理。GD库常用在图片加水印，验证码生成等方面。
PHP默认已经集成了GD库，只需要在安装的时候开启就行。

```php
header("content-type: image/png");
$img=imagecreatetruecolor(100, 100);
$red=imagecolorallocate($img, 0xFF, 0x00, 0x00);
imagefill($img, 0, 0, $red);
imagepng($img);
imagedestroy($img);
```

<h2>绘制线条</h2>
要对图形进行操作，首先要新建一个画布，通过imagecreatetruecolor函数可以创建一个真彩色的空白图片：

```php
$img = imagecreatetruecolor(100, 100);
```
GD库中对于画笔所用的颜色，需要通过imagecolorallocate函数进行分配，通过参数设定RGB的颜色值来确定画笔的颜色：

```php
$red = imagecolorallocate($img, 0xFF, 0x00, 0x00);
```
然后我们通过调用绘制线段函数imageline进行线条的绘制，通过指定起点跟终点来最终得到线条。

```php
imageline($img, 0, 0, 100, 100, $red);
```
线条绘制好以后，通过header与imagepng进行图像的输出。

```php
header("content-type: image/png");
imagepng($img);
```
最后可以调用imagedestroy释放该图片占用的内存。

```php
imagedestroy($img);
```
通过上面的步骤，可以发现PHP绘制图形非常的简单，但很多时候我们不只是需要输出图片，可能我们还需要得到一个图片文件，可以通过imagepng函数指定文件名将绘制后的图像保存到文件中。

```php
imagepng($img, 'img.png');
```
<h2>生成图像验证码</h2>
```php
<?php
$img = imagecreatetruecolor(100, 40);
$black = imagecolorallocate($img, 0x00, 0x00, 0x00);
$green = imagecolorallocate($img, 0x00, 0xFF, 0x00);
$white = imagecolorallocate($img, 0xFF, 0xFF, 0xFF);
imagefill($img,0,0,$white);
//生成随机的验证码
$code = '';
for($i = 0; $i < 4; $i++) {
    $code .= rand(0, 9);
}
imagestring($img, 5, 10, 10, $code, $black);
//加入噪点干扰
for($i=0;$i<50;$i++) {
  imagesetpixel($img, rand(0, 100) , rand(0, 100) , $black); 
  imagesetpixel($img, rand(0, 100) , rand(0, 100) , $green);
}
//输出验证码
header("content-type: image/png");
imagepng($img);
imagedestroy($img);
?>
```



<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
