---
title: Core Data
date: 2016-10-08 11:39:50
categories: objective-c
---
Core Data是iOS5之后才出现的一个框架，它提供了对象-关系映射(ORM)的功能，即能够将OC对象转化成数据，保存在SQLite数据库文件中，也能够将保存在数据库中的数据还原成OC对象。
<!-- more -->
在此数据操作期间，我们不需要编写任何SQL语句，这个有点类似于著名的Hibernate持久化框架，不过功能肯定是没有Hibernate强大的。简单地用下图描述下它的作用：
![abc](http://img.my.csdn.net/uploads/201302/01/1359705997_4313.png)
左边是关系模型，即数据库，数据库里面有张person表，person表里面有id、name、age三个字段，而且有2条记录；
右边是对象模型，可以看到，有2个OC对象；
利用Core Data框架，我们就可以轻松地将数据库里面的2条记录转换成2个OC对象，也可以轻松地将2个OC对象保存到数据库中，变成2条表记录，而且不用写一条SQL语句。

#[]()模型文件
  在Core Data，需要进行映射的对象称为实体(entity)，而且需要使用Core
 Data的模型文件来描述app中的所有实体和实体属性。这里以Person(人)和Card(身份证)2个实体为例子，先看看实体属性和实体之间的关联关系：
![abc](http://img.my.csdn.net/uploads/201302/01/1359707024_5895.png)
Person实体中有：name（姓名）、age（年龄）、card（身份证）三个属性
Card实体中有：no（号码）、person（人）两个属性

接下来看看创建模型文件的过程：
1.选择模板
![abc](http://img.my.csdn.net/uploads/201302/01/1359707426_5763.png)![abc](http://img.my.csdn.net/uploads/201302/01/1359707501_8695.png)

2.添加实体
![](http://img.my.csdn.net/uploads/201302/01/1359707563_9302.png)

3.添加Person的2个基本属性
![](http://img.my.csdn.net/uploads/201302/01/1359707773_7614.png)

4.添加Card的1个基本属性
![](http://img.my.csdn.net/uploads/201302/01/1359707796_4561.png)


5.建立Card和Person的关联关系![](http://img.my.csdn.net/uploads/201302/01/1359708105_6064.png)        ![](http://img.my.csdn.net/uploads/201302/01/1359708115_5772.png)
右图中的![](http://img.my.csdn.net/uploads/201302/01/1359708186_1349.png)表示Card中有个Person类型的person属性，目的就是建立Card跟Person之间的一对一关联关系(建议补上这一项)，在Person中加上Inverse属性后，你会发现Card中Inverse属性也自动补上了
![](http://img.my.csdn.net/uploads/201302/01/1359708436_2378.png)



****
#[]()了解NSManagedObject

1.通过Core Data从数据库取出的对象，默认情况下都是NSManagedObject对象
![](http://img.my.csdn.net/uploads/201302/01/1359708744_9527.png)  ![](http://img.my.csdn.net/uploads/201302/01/1359708756_9809.png)

2.NSManagedObject的工作模式有点类似于NSDictionary对象，通过键-值对来存取所有的实体属性1> setValue:forKey:存储属性值(属性名为key)
2> valueForKey:获取属性值(属性名为key)


****#[]()CoreData中的核心对象

![](http://img.my.csdn.net/uploads/201302/01/1359708878_8041.png)
注：黑色表示类名，红色表示类里面的一个属性
开发步骤总结：
1.初始化NSManagedObjectModel对象，加载模型文件，读取app中的所有实体信息
2.初始化NSPersistentStoreCoordinator对象，添加持久化库(这里采取SQLite数据库)
3.初始化NSManagedObjectContext对象，拿到这个上下文对象操作实体，进行CRUD操作

****#[]()代码实现
先添加CoreData.framework和导入主头文件<CoreData/CoreData.h>
![](http://img.my.csdn.net/uploads/201302/01/1359710937_3208.png)

1.搭建上下文环境
**[java]** [view
 plain](http://blog.csdn.net/q199109106q/article/details/8563438/# "view plain")[copy](http://blog.csdn.net/q199109106q/article/details/8563438/# "copy")1. // 从应用程序包中加载模型文件  
2. NSManagedObjectModel *model = [NSManagedObjectModel mergedModelFromBundles:nil];  
3. // 传入模型对象，初始化NSPersistentStoreCoordinator  
4. NSPersistentStoreCoordinator *psc = [[[NSPersistentStoreCoordinator alloc] initWithManagedObjectModel:model] autorelease];  
5. // 构建SQLite数据库文件的路径  
6. NSString *docs = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) lastObject];  
7. NSURL *url = [NSURL fileURLWithPath:[docs stringByAppendingPathComponent:@"person.data"]];  
8. // 添加持久化存储库，这里使用SQLite作为存储库  
9. NSError *error = nil;  
10. NSPersistentStore *store = [psc addPersistentStoreWithType:NSSQLiteStoreType configuration:nil URL:url options:nil error:&error];  
11. if (store == nil) { // 直接抛异常  
12.     [NSException raise:@"添加数据库错误" format:@"%@", [error localizedDescription]];  
13. }  
14. // 初始化上下文，设置persistentStoreCoordinator属性  
15. NSManagedObjectContext *context = [[NSManagedObjectContext alloc] init];  
16. context.persistentStoreCoordinator = psc;  
17. // 用完之后，记得要[context release];  


2.添加数据到数据库
**[java]** [view
 plain](http://blog.csdn.net/q199109106q/article/details/8563438/# "view plain")[copy](http://blog.csdn.net/q199109106q/article/details/8563438/# "copy")1. // 传入上下文，创建一个Person实体对象  
2. NSManagedObject *person = [NSEntityDescription insertNewObjectForEntityForName:@"Person" inManagedObjectContext:context];  
3. // 设置Person的简单属性  
4. [person setValue:@"MJ" forKey:@"name"];  
5. [person setValue:[NSNumber numberWithInt:27] forKey:@"age"];  
6. // 传入上下文，创建一个Card实体对象  
7. NSManagedObject *card = [NSEntityDescription insertNewObjectForEntityForName:@"Card" inManagedObjectContext:context];  
8. [card setValue:@"4414241933432" forKey:@"no"];  
9. // 设置Person和Card之间的关联关系  
10. [person setValue:card forKey:@"card"];  
11. // 利用上下文对象，将数据同步到持久化存储库  
12. NSError *error = nil;  
13. BOOL success = [context save:&error];  
14. if (!success) {  
15.     [NSException raise:@"访问数据库错误" format:@"%@", [error localizedDescription]];  
16. }  
17. // 如果是想做更新操作：只要在更改了实体对象的属性后调用[context save:&error]，就能将更改的数据同步到数据库  


3.从数据库中查询数据
**[java]** [view
 plain](http://blog.csdn.net/q199109106q/article/details/8563438/# "view plain")[copy](http://blog.csdn.net/q199109106q/article/details/8563438/# "copy")1. // 初始化一个查询请求  
2. NSFetchRequest *request = [[[NSFetchRequest alloc] init] autorelease];  
3. // 设置要查询的实体  
4. request.entity = [NSEntityDescription entityForName:@"Person" inManagedObjectContext:context];  
5. // 设置排序（按照age降序）  
6. NSSortDescriptor *sort = [NSSortDescriptor sortDescriptorWithKey:@"age" ascending:NO];  
7. request.sortDescriptors = [NSArray arrayWithObject:sort];  
8. // 设置条件过滤(搜索name中包含字符串"Itcast-1"的记录，注意：设置条件过滤时，数据库SQL语句中的%要用*来代替，所以%Itcast-1%应该写成*Itcast-1*)  
9. NSPredicate *predicate = [NSPredicate predicateWithFormat:@"name like %@", @"*Itcast-1*"];  
10. request.predicate = predicate;  
11. // 执行请求  
12. NSError *error = nil;  
13. NSArray *objs = [context executeFetchRequest:request error:&error];  
14. if (error) {  
15.     [NSException raise:@"查询错误" format:@"%@", [error localizedDescription]];  
16. }  
17. // 遍历数据  
18. for (NSManagedObject *obj in objs) {  
19.     NSLog(@"name=%@", [obj valueForKey:@"name"]  
20. }  

注：Core Data不会根据实体中的关联关系立即获取相应的关联对象，比如通过Core Data取出Person实体时，并不会立即查询相关联的Card实体；当应用真的需要使用Card时，才会再次查询数据库，加载Card实体的信息。这个就是Core Data的延迟加载机制

4.删除数据库中的数据
**[java]** [view
 plain](http://blog.csdn.net/q199109106q/article/details/8563438/# "view plain")[copy](http://blog.csdn.net/q199109106q/article/details/8563438/# "copy")1. // 传入需要删除的实体对象  
2. [context deleteObject:managedObject];  
3. // 将结果同步到数据库  
4. NSError *error = nil;  
5. [context save:&error];  
6. if (error) {  
7.     [NSException raise:@"删除错误" format:@"%@", [error localizedDescription]];  
8. }  


********#[]()打开CoreData的SQL语句输出开关
1.打开Product，点击EditScheme...
2.点击Arguments，在ArgumentsPassed On Launch中添加2项
1> -com.apple.CoreData.SQLDebug
2> 1
![](http://img.my.csdn.net/uploads/201302/01/1359711942_1857.png)![](http://img.my.csdn.net/uploads/201302/01/1359711964_1550.png)


********#[]()创建NSManagedObject的子类
默认情况下，利用Core Data取出的实体都是NSManagedObject类型的，能够利用键-值对来存取数据。但是一般情况下，实体在存取数据的基础上，有时还需要添加一些业务方法来完成一些其他任务，那么就必须创建NSManagedObject的子类
![](http://img.my.csdn.net/uploads/201302/01/1359712054_3978.png)

选择模型文件 
![](http://img.my.csdn.net/uploads/201302/01/1359712079_5045.png)

选择需要创建子类的实体 
![](http://img.my.csdn.net/uploads/201302/01/1359712094_5888.png)

创建完毕后，多了2个子类 
![](http://img.my.csdn.net/uploads/201302/01/1359712116_3772.png)

文件内容展示：
Person.h
**[java]** [view
 plain](http://blog.csdn.net/q199109106q/article/details/8563438/# "view plain")[copy](http://blog.csdn.net/q199109106q/article/details/8563438/# "copy")1. #import <Foundation/Foundation.h>  
2. #import <CoreData/CoreData.h>  
3.   
4. @class Card;  
5.   
6. @interface Person : NSManagedObject  
7.   
8. @property (nonatomic, retain) NSString * name;  
9. @property (nonatomic, retain) NSNumber * age;  
10. @property (nonatomic, retain) Card *card;  
11.   
12. @end  


Person.m
**[java]** [view
 plain](http://blog.csdn.net/q199109106q/article/details/8563438/# "view plain")[copy](http://blog.csdn.net/q199109106q/article/details/8563438/# "copy")1. #import "Person.h"  
2.   
3. @implementation Person  
4.   
5. @dynamic name;  
6. @dynamic age;  
7. @dynamic card;  
8.   
9. @end  


Card.h
**[java]** [view
 plain](http://blog.csdn.net/q199109106q/article/details/8563438/# "view plain")[copy](http://blog.csdn.net/q199109106q/article/details/8563438/# "copy")1. #import <Foundation/Foundation.h>  
2. #import <CoreData/CoreData.h>  
3.   
4. @class Person;  
5.   
6. @interface Card : NSManagedObject  
7.   
8. @property (nonatomic, retain) NSString * no;  
9. @property (nonatomic, retain) Person *person;  
10.   
11. @end  


Card.m
**[java]** [view
 plain](http://blog.csdn.net/q199109106q/article/details/8563438/# "view plain")[copy](http://blog.csdn.net/q199109106q/article/details/8563438/# "copy")1. #import "Card.h"  
2. #import "Person.h"  
3.   
4. @implementation Card  
5.   
6. @dynamic no;  
7. @dynamic person;  
8.   
9. @end  


那么往数据库中添加数据的时候就应该写了：
**[java]** [view
 plain](http://blog.csdn.net/q199109106q/article/details/8563438/# "view plain")[copy](http://blog.csdn.net/q199109106q/article/details/8563438/# "copy")1. Person *person = [NSEntityDescription insertNewObjectForEntityForName:@"Person" inManagedObjectContext:context];  
2. person.name = @"MJ";  
3. person.age = [NSNumber numberWithInt:27];  
4.   
5. Card *card = [NSEntityDescription insertNewObjectForEntityForName:@”Card" inManagedObjectContext:context];  
6. card.no = @”4414245465656";  
7. person.card = card;  
8. // 最后调用[context save&error];保存数据  



说到这里，整个Core Data框架的入门就结束了，其实Core Data还远不止这些功能，它还支持自动撤销机制，一对多关联等，这里就不一一介绍了