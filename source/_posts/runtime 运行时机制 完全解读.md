---
title: runtime 运行时机制 完全解读
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

我们前面已经讲过一篇runtime 原理，现在这篇文章主要介绍的是runtime是什么以及怎么用！希望对读者有所帮助！
首先，第一个问题， 
1》runtime实现的机制是什么,怎么用，一般用于干嘛？ 
这个问题我就不跟大家绕弯子了，直接告诉大家， 
runtime是一套比较底层的纯C语言API, 属于1个C语言库, 包含了很多底层的C语言API。 
在我们平时编写的OC代码中, 程序运行过程时, 其实最终都是转成了runtime的C语言代码, runtime算是OC的幕后工作者 
比如说，下面一个创建对象的方法中， 
举例: 
OC : 
[[MJPerson alloc] init] 
runtime : 
objc_msgSend(objc_msgSend(“MJPerson” , “alloc”), “init”)
第二个问题 
runtime 用来干什么呢？？用在那些地方呢？怎么用呢？ 
runtime是属于OC的底层, 可以进行一些非常底层的操作(用OC是无法现实的, 不好实现)
- 在程序运行过程中, 动态创建一个类(比如KVO的底层实现)

- 在程序运行过程中, 动态地为某个类添加属性\方法, 修改属性值\方法

- 遍历一个类的所有成员变量(属性)\所有方法 
例如：我们需要对一个类的属性进行归档解档的时候属性特别的多，这时候，我们就会写很多对应的代码，但是如果使用了runtime就可以动态设置！ 
例如，PYPerson.h的文件如下所示
#import 


[@interface](http://my.oschina.net/u/996807) PYPerson : NSObject 
@property (nonatomic, assign) int age; 
@property (nonatomic, assign) int height; 
@property (nonatomic, copy) NSString *name; 
@property (nonatomic, assign) int age2; 
@property (nonatomic, assign) int height2; 
@property (nonatomic, assign) int age3; 
@property (nonatomic, assign) int height3; 
@property (nonatomic, assign) int age4; 
@property (nonatomic, assign) int height4;
[@end](http://my.oschina.net/u/567204)
而PYPerson.m实现文件的内容如下
	*<!-- lang: cpp -->*
	#import "PYPerson.h"
	

#import 
@implementation PYPerson
- (void)encodeWithCoder:(NSCoder )encoder 
{ 
unsigned int count = 0; 
Ivar ivars = class_copyIvarList([PYPerson class], &count);
for (int i = 0; i<count; i++) {
	*// 取出i位置对应的成员变量*
	Ivar ivar = ivars[i];
	
	*// 查看成员变量*
	const char *name = ivar_getName(ivar);
	
	*// 归档*
	NSString *key = [NSString stringWithUTF8String:name];
	id value = [self valueForKey:key];
	[encoder encodeObject:value forKey:key];
	

}
free(ivars); 
}

- (id)initWithCoder:(NSCoder *)decoder 
{ 
if (self = [super init]) {
	unsigned int count = 0;
	Ivar *ivars = class_copyIvarList([PYPerson class], &count);
	
	for (int i = 0; i<count; i++) {
	    *// 取出i位置对应的成员变量*
	    Ivar ivar = ivars[i];
	
	    *// 查看成员变量*
	    const char *name = ivar_getName(ivar);
	
	    *// 归档*
	    NSString *key = [NSString stringWithUTF8String:name];
	    id value = [decoder decodeObjectForKey:key];
	
	    *// 设置到成员变量身上*
	    [self setValue:value forKey:key];
	}
	
	free(ivars);
	

} 
return self; 
}


[@end](http://my.oschina.net/u/567204)
这样我们可以看到归档和解档的案例其实是runtime写下的
学习，runtime机制首先要了解下面几个问题 
1相关的头文件和函数 
1> 头文件
- 
利用头文件，我们可以查看到runtime中的各个方法！ 

2> 相关应用
- NSCoding(归档和解档, 利用runtime遍历模型对象的所有属性)
- 字典 –> 模型 (利用runtime遍历模型对象的所有属性, 根据属性名从字典中取出对应的值, 设置到模型的属性上)
- KVO(利用runtime动态产生一个类)
- 用于封装框架(想怎么改就怎么改) 
这就是我们runtime机制的只要运用方向

3> 相关函数
- objc_msgSend : 给对象发送消息
- class_copyMethodList : 遍历某个类所有的方法
- class_copyIvarList : 遍历某个类所有的成员变量
- class_….. 
这是我们学习runtime必须知道的函数！

4.必备常识 
1> Ivar : 成员变量 
2> Method : 成员方法 
从上面例子中我们看到我们定义的成员变量，如果要是动态创建方法，可以使用Method，
也许，看到这里，你是否对runtime有了更深入的了解呢？在这里，希望我们大家相互交流！有什么错误之处，还请指正 

RunTime简称运行时。就是系统在运行的时候的一些机制，其中最主要的是消息机制。对于C语言，函数的调用在编译的时候会决定调用哪个函数（ C语言的函数调用请看这里 ）。编译完成之后直接顺序执行，无任何二义性。OC的函数调用成为消息发送。属于动态调用过程。在编译的时候并不能决定真正调用哪个函数（事实证明，在编 译阶段，OC可以调用任何函数，即使这个函数并未实现，只要申明过就不会报错。而C语言在编译阶段就会报错）。只有在真正运行的时候才会根据函数的名称找 到对应的函数来调用。
那OC是怎么实现动态调用的呢？下面我们来看看OC通过发送消息来达到动态调用的秘密。假如在OC中写了这样的一个代码：
1`[obj makeText];`其中obj是一个对象，makeText是一个函数名称。对于这样一个简单的调用。在编译时RunTime会将上述代码转化成
1`objc_msgSend(obj,@selector(makeText));`首先我们来看看obj这个对象，iOS中的obj都继承于NSObject。
123`@interface NSObject <nsobject> {``    ``Class isa  OBJC_ISA_AVAILABILITY;``}</nsobject>`在NSObjcet中存在一个Class的isa指针。然后我们看看Class：
1234567891011121314`typedef struct objc_class *Class;``struct objc_class {``  ``Class isa; ``// 指向metaclass``  ` `  ``Class super_class ; ``// 指向其父类``  ``const char *name ; ``// 类名``  ``long version ; ``// 类的版本信息，初始化默认为0，可以通过runtime函数class_setVersion和class_getVersion进行修改、读取``  ``long info; ``// 一些标识信息,如CLS_CLASS (0x1L) 表示该类为普通 class ，其中包含对象方法和成员变量;CLS_META (0x2L) 表示该类为 metaclass，其中包含类方法;``  ``long instance_size ; ``// 该类的实例变量大小(包括从父类继承下来的实例变量);``  ``struct objc_ivar_list *ivars; ``// 用于存储每个成员变量的地址``  ``struct objc_method_list **methodLists ; ``// 与 info 的一些标志位有关,如CLS_CLASS (0x1L),则存储对象方法，如CLS_META (0x2L)，则存储类方法;``  ``struct objc_cache *cache; ``// 指向最近使用的方法的指针，用于提升效率；``  ``struct objc_protocol_list *protocols; ``// 存储该类遵守的协议``    ``}`

我们可以看到，对于一个Class类中，存在很多东西，下面我来一一解释一下：
Class isa：指向metaclass，也就是静态的Class。一般一个Obj对象中的isa会指向普通的Class，这个Class中存储普通成员变量和对 象方法（“-”开头的方法），普通Class中的isa指针指向静态Class，静态Class中存储static类型成员变量和类方法（“+”开头的方 法）。
Class super_class:指向父类，如果这个类是根类，则为NULL。
下面一张图片很好的描述了类和对象的继承关系：
![iuqQFnm.png](cid:f919a9e0f1d396baac9ed7f8fdc4a3be "1413628797629491.png")
**注意**：所有metaclass中isa指针都指向跟metaclass。而跟metaclass则指向自身。Root metaclass是通过继承Root class产生的。与root class结构体成员一致，也就是前面提到的结构。不同的是Root metaclass的isa指针指向自身。
Class类中其他的成员这里就先不做过多解释了，下面我们来看看：
**@selector (makeText)**：这是一个SEL方法选择器。SEL其主要作用是快速的通过方法名字（makeText）查找到对应方法的函数指针，然后调用其函 数。SEL其本身是一个Int类型的一个地址，地址中存放着方法的名字。对于一个类中。每一个方法对应着一个SEL。所以iOS类中不能存在2个名称相同 的方法，即使参数类型不同，因为SEL是根据方法名字生成的，相同的方法名称只能对应一个SEL。
下面我们就来看看具体消息发送之后是怎么来动态查找对应的方法的。

首先，编译器将代码[obj makeText];转化为objc_msgSend(obj, @selector (makeText));，在objc_msgSend函数中。首先通过obj的isa指针找到obj对应的class。在Class中先去cache中 通过SEL查找对应函数method（猜测cache中method列表是以SEL为key通过hash表来存储的，这样能提高函数查找速度），若 cache中未找到。再去methodList中查找，若methodlist中未找到，则取superClass中查找。若能找到，则将method加
 入到cache中，以方便下次查找，并通过method中的函数指针跳转到对应的函数中去执行。 

