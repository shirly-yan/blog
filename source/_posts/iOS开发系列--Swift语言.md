---
title: iOS开发系列--Swift语言
date: 2016-10-08 11:39:43
categories: objective-c
---
Swift是苹果2014年推出的全新的编程语言，它继承了C语言、ObjC的特性，且克服了C语言的兼容性问题。
<!-- more -->
来自：http://blog.csdn.net/jianxin160/article/details/47753245
概述
Swift是苹果2014年推出的全新的编程语言，它继承了C语言、ObjC的特性，且克服了C语言的兼容性问题。Swift发展过程中不仅保留了ObjC很多语法特性，它也借鉴了多种现代化语言的特点，在其中你可以看到C#、Java、Javascript、Python等多种语言的影子。同时在2015年的WWDC上苹果还宣布Swift的新版本Swift2.0，并宣布稍后Swift即将开源，除了支持iOS、OS X之外还将支持linux。
本文将继续iOS开发系列教程，假设读者已经有了其他语言基础（强烈建议初学者从本系列第一章开始阅读，如果您希望从Swift学起，那么推荐你首先阅读苹果官方电子书[《the swift
 programming language》](https://itunes.apple.com/us/book/swift-programming-language/id881256329?mt=11)），不会从零基础一点点剖析这门语言的语法，旨在帮助大家快速从ObjC快速过度到Swift开发中。即便如此，要尽可能全面的介绍Swift的语法特点也不是一件容易的事情，因此本文将采用较长的篇幅进行介绍。
1. [基础部分](http://blog.csdn.net/jianxin160/article/details/47753245#basic)1. [第一个Swift程序](http://blog.csdn.net/jianxin160/article/details/47753245#firstSwift)
2. [数据类型](http://blog.csdn.net/jianxin160/article/details/47753245#dataType)1. [基础类型](http://blog.csdn.net/jianxin160/article/details/47753245#basicType)
2. [集合类型](http://blog.csdn.net/jianxin160/article/details/47753245#collectionType)
3. [元组](http://blog.csdn.net/jianxin160/article/details/47753245#tuple)
4. [可选类型](http://blog.csdn.net/jianxin160/article/details/47753245#optional)


5. [运算符](http://blog.csdn.net/jianxin160/article/details/47753245#operator)
6. [控制流](http://blog.csdn.net/jianxin160/article/details/47753245#controlFlow)


7. [函数和闭包](http://blog.csdn.net/jianxin160/article/details/47753245#functionAndClosures)1. [函数](http://blog.csdn.net/jianxin160/article/details/47753245#function)
2. [闭包](http://blog.csdn.net/jianxin160/article/details/47753245#closures)


3. [类](http://blog.csdn.net/jianxin160/article/details/47753245#class)1. [属性](http://blog.csdn.net/jianxin160/article/details/47753245#property)
2. [方法](http://blog.csdn.net/jianxin160/article/details/47753245#method)
3. [下标脚本](http://blog.csdn.net/jianxin160/article/details/47753245#subscript)
4. [继承](http://blog.csdn.net/jianxin160/article/details/47753245#inheritance)


5. [协议](http://blog.csdn.net/jianxin160/article/details/47753245#protocol)
6. [扩展](http://blog.csdn.net/jianxin160/article/details/47753245#extension)
7. [枚举和结构体](http://blog.csdn.net/jianxin160/article/details/47753245#enumAndStruct)1. [结构体](http://blog.csdn.net/jianxin160/article/details/47753245#struct)
2. [枚举](http://blog.csdn.net/jianxin160/article/details/47753245#enum)


3. [泛型](http://blog.csdn.net/jianxin160/article/details/47753245#generic)

#[]()基础部分
##[]()第一个Swift程序
创建一个命令行程序如下：

```swift
import Foundation

/**
*  Swift没有main函数，默认从top level code的上方开始自上而下执行（因此不能有多个top level代码）
*/
println("Hello, World!")

```
从上面的代码可以看出：
1. Swift没有main函数，从top level code的上方开始往下执行（就是第一个非声明语句开始执行[表达式或者控制结构，类、结构体、枚举和方法等属于声明语句]），不能存在多个top level code文件(否则编译器无法确定执行入口，事实上swift隐含一个main函数，这个main函数会设置并调用全局 “C_ARGC C_ARGV”并调用由top level code构成的top_level_code()函数)；
2. Swift通过import引入其他类库（和Java比较像）；
3. Swift语句不需要双引号结尾（尽管加上也不报错），除非一行包含多条语句（和Python有点类似）；

##[]()数据类型
Swift包含了C和ObjC语言中的所有基础类型，Int整形，Float和Double浮点型，Bool布尔型，Character字符型，String字符串类型；当然还包括enum枚举、struct结构体构造类型；Array数组、Set集合、Dictionary字典集合类型；不仅如此还增加了高阶数据类型元组（Tuple），可选类型（Optinal）。
###[]()基础类型
Xcode 从6.0开始加入了Playground代码测试，可以实时查看代码执行结果，下面使用Playground简单演示一下Swift的基础内容，对Swift有个简单的认识：

```swift
import Foundation

var a:Int=1 //通过var定义一个变量

//下面变量b虽然没有声明类型，但是会自动进行类型推断，这里b推断为Int类型
var b=2

var c:UInt=3
let d=a+b //通过let定义一个变量

//下面通过"\()"实现了字符串和变量相加(字符串插值)，等价于println("d="+String(d))
println("d=\(d)") //结果：d=3

//注意由于Swift是强类型语言，a是Int类型而c是UInt类型，二者不能运算，下面的语句报错;但是注意如果是类似于：let a=1+2.0是不会报错的，因为两个都是字面量，Swift会首先计算出结果再推断a的类型
//let e=a+c

//Int.max是Int类型的最大值，类似还有Int.min、Int32.max、Int32.min等
let e=Int.max //结果：9223372036854775807

var f:Float=1.0
var g=2.0 //浮点型自动推断为Double类型

var h:String="hello "

//emoj表情也可以作为变量或者常量，事实上所有Unicode字符都是可以的
var 
```
