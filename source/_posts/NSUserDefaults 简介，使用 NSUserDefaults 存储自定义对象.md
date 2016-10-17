---
title: NSUserDefaults 简介，使用 NSUserDefaults 存储自定义对象
date: 2016-10-08 11:39:44
categories: objective-c
---
NSUserDefaults适合存储轻量级的本地数据，一些简单的数据（NSString类型的）例如密码，网址等，NSUserDefaults肯定是首选，但是如果我们自定义了一个对象，对象保存的是一些信息，这时候就不能直接存储到NSUserDefaults了
<!-- more -->
一、了解NSUserDefaults以及它可以直接存储的类型


      NSUserDefaults是一个单例，在整个程序中只有一个实例对象，他可以用于数据的永久保存，而且简单实用，这是它可以让数据自由传递的一个前提，也是大家喜欢用它保存简单数据的一个主要原因。     

      使用 NSUserDefaults 存储自定义对象的最初，我们必须认识NSUserDefaults可以存储哪一些类型的数据，下面一一列出：

NSUserDefaults支持的数据类型有：NSNumber（NSInteger、float、double），NSString，NSDate，NSArray，NSDictionary，BOOL.
如果想要将上述数据类型的数据永久保存到NSUserDefaults中去，只需要简单的操作(一个Value 一个Key ),例如，想要保存一个NSString的对象，代码实现为：



    //将NSString 对象存储到 NSUserDefaults 中
    NSString *passWord = @"1234567";
    NSUserDefaults *user = [NSUserDefaults standardUserDefaults];
    [user setObject:passWord forKey:@"userPassWord"];


    将数据取出也很简单，只需要取出key 对应的值就好了，代码如下：

    NSUserDefaults *user = [NSUserDefaults standardUserDefaults];
    NSString *passWord = [ user objectForKey:@"userPassWord"];


注意：对相同的Key赋值约等于一次覆盖，要保证每一个Key的唯一性

值得注意的是：

        NSUserDefaults 存储的对象全是不可变的（这一点非常关键，弄错的话程序会出bug），例如，如果我想要存储一个 NSMutableArray 对象，我必须先创建一个不可变数组（NSArray）再将它存入NSUserDefaults中去，代码如下：



    NSMutableArray *mutableArray = [NSMutableArray arrayWithObjects:@"123",@"234", nil];
    NSArray * array = [NSArray arrayWithArray:mutableArray];
    
    NSUserDefaults *user = [NSUserDefaults standardUserDefaults];
    [user setObject:array forKey:@"记住存放的一定是不可变的"];


取出数据是一样的，想要用NSUserDefaults中的数据给可变数组赋值

先给出一个错误的写法：

```objc
    /*-------------------------错误的赋值方法-------------------*/
    NSUserDefaults *user = [NSUserDefaults standardUserDefaults];
    
    //这样写后，mutableArray 就变成了不可变数组了，如果你要在数组中添加或删除数据就会出现bug
    NSMutableArray *mutableArray = [user objectForKey:@"记住存放的一定是不可变的"];



正确的写法：

    /*-------------------------正确的赋值方法-------------------*/
    NSUserDefaults *user = [NSUserDefaults standardUserDefaults];
    
    //可以用alloc 方法代替
    NSMutableArray *mutableArray = [NSMutableArray arrayWithArray:[user objectForKey:@"记住存放的一定是不可变的"]];


二、使用 NSUserDefaults 存储自定义对象

1、将自定义类型转换为NSData类型

      当数据重复而且多的时候（例如想存储全班同学的学号，姓名，性别（这个数据量可能太大了 ）），如果不用SQLite 存储 （多数据最好还是用这个），你可以选择使用归档，再将文件写入本地，但是这种方式和 NSUserDefaults 比起来麻烦多了（因为NSFileManage 本来就挺复杂） ，但是问题是，NSUserDefaults 本身不支持自定义对象的存储，不过它支持NSData的类型，下面举一个例子来介绍。



我们先建立一个叫Student 的类，这个类里有三个属性（学号，姓名，性别）,如图：



我们要做的就是将Student类型变成NSData类型 ，那么就必须实现归档：

这里要实现 在.h 文件中申明 NSCoding 协议，再 在 .m 中实现 encodeWithCoder 方法 和 

initWithCoder 方法就可以了 ： 

.h 中修改文件如图 ：



.m中加入代码 ：



这样做就可以将自定义类型转变为NSData类型了



2、将自定义类型数据存入 NSUserDefaults 中

    如果要存储全班同学的信息，我们可以建一个NSMutableArray 来存放全班同学的信息（里面存储的全是NSData对象）在需要存储的地方加入代码：

//首先，要建立一个可变数组来存储 NSDate对象

     Student *student = [[Student alloc] ini];
     
    //下面进行的是对student对象的 name ， studentNumber ，sex 的赋值
    student.name = @"lady-奕奕";
    student.studentNumber = @"3100104006";
    student.sex = @"女";
    
    //这是一个存放全班同学的数组
    NSMutableArray * dataArray = [NSMutableArray arrayWithCapacity:50];
    
    //将student类型变为NSData类型
    NSData *data = [NSKeyedArchiver archivedDataWithRootObject:student];
    
    //存放数据的数组将data加入进去
    [dataArray addObject:data];


如果你只想存一个人的信息，你可以直接将NSData存入NSUserDefaults中 :

    NSData *data = [NSKeyedArchiver archivedDataWithRootObject:student];   
    
    NSUserDefaults *user = [NSUserDefaults standardUserDefaults];
    [user setObject:data forKey:@"oneStudent"];


如果你想存储全班同学的信息，你还要用一个for循环将data 放入 dataArray中，这里具体的操作就不实现了，只给出存放的代码：

   //记住要转换成不可变数组类型
    NSArray * array = [NSArray arrayWithArray:dataArray];
    
    NSUserDefaults *user = [NSUserDefaults standardUserDefaults];
    [user setObject:array forKey:@"allStudent"];


从NSUserDefaults中取出数据在还原也很简单

例如还原一个学生的数据：

NSUserDefaults *user = [NSUserDefaults standardUserDefaults];
 
 NSdData *data = [user objectForKey:@"oneStudent"];
    
 Student *student = [NSKeyedUnarchiver unarchiveObjectWithData:data];


总之，NSUserDefaults 在我们编写代码中是最常用的一个永久保存数据的方法，也是最简单的。
```


