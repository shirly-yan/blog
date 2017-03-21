---
title: iOS 网络编程模式总结
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

转自：  http://blog.csdn.net/goohong/article/details/40505291
IOS 可以采用三类api 接口进行网络编程，根据抽象层次从低到高分别为socket方式、stream方式、url 方式。
一 、socket 方式
       IOS 提供的socket 方式的网络编程接口为CFSocket。CFSocket是BSD sockets的抽象和封装，CFSocket提供BSD sockets几乎所有的功能，并与run loop集成，用来实现多线程网络编程和网络事件监听。基于 CFSocket可以实现各种类型的 socket编程，包括stream-based 的sockets(如tcp)和packet-based 的sockets(如udp)。需要注意的是在iOS中CFSocket接口在需要时不自动激活设备的
 cellular modem或on-demand VPN。
      CFSocket包括以下编程接口，包括Socket的 创建、配置，以及根据创建和配置好的Socket 进行 远程通讯等接口。
1  Socket的 创建
> 1 .1、CFSocketCreate
> 创建一个特定协议和类型的 CFSocket对象
> 1.2、CFSocketCreateWithSocketSignature
> 该接口根据一个包含通讯协议和地址的CFSocketSignature结构来创建一个CFSocket对象
> 
> 
> 1.3、 CFSocketCreateConnectedToSocketSignature
> 该接口在创建一个CFSocket对象的同时还与一个远端主机进行连接。
> 
> 
> 1.4、CFSocketCreateWithNative
> 该接口通过封装一个存在的 BSD socket来创建一个CFSocket对象。

2   Socket的配置
> 2.1   CFSocketCopyAddress
> 功能： 返回一个 CFSocket对象的本地地址。
> 语法：
> SWIFT
> func CFSocketCopyAddress(_ s: CFSocket!) -> CFData!
> 2.2、CFSocketCopyPeerAddress
> 功能：返回与一个 CFSocket对象连接的远端地址。
> 语法：
> SWIFT
> func CFSocketCopyPeerAddress(_ s: CFSocket!) -> CFData!
> 2.3  CFSocketDisableCallBacks
> 功能：临时取消一个CFSocket对象创建时指定的某种类型的事件回调。
> 语法：
> SWIFT
> func CFSocketDisableCallBacks(_ s: CFSocket!,
>                             _ callBackTypes: CFOptionFlags)
> 2.4   CFSocketEnableCallBacks
> 功能：重新允许先前CFSocketDisableCallBacks函数取消的某种类型的事件回调。
> 语法：
> SWIFT
> func CFSocketEnableCallBacks(_ s: CFSocket!,
>                            _ callBackTypes: CFOptionFlags)
> 2.5 CFSocketGetContext
> 功能：返回一个CFSocket对象的上下文信息。
> 语法：
> SWIFT
> func CFSocketGetContext(_ s: CFSocket!,
>                       _ context: UnsafeMutablePointer<CFSocketContext>)
> 2.6 CFSocketGetNative
> 返回与一个CFSocket对象相关的本地 BSD socket。
> 语法：
> SWIFT
> func CFSocketGetNative(_ s: CFSocket!) -> CFSocketNativeHandle
> 2.7   CFSocketGetSocketFlags
> 功能：返回控制一个CFSocket对象的确定行为的 标志。
> 语法：
> SWIFT
> func CFSocketGetSocketFlags(_ s: CFSocket!) -> CFOptionFlags
> 
> 
> 2.8 CFSocketSetSocketFlags
> 功能：设置控制一个CFSocket对象的确定行为的 标志。
> 语法：
> SWIFT
> func CFSocketSetSocketFlags(_ s: CFSocket!,
>                           _ flags: CFOptionFlags)
> 2.9 CFSocketSetAddress
> 语法：
> SWIFT
> func CFSocketSetAddress(_ s: CFSocket!,
>                       _ address: CFData!) -> CFSocketError
> 功能：为一个CFSocket对象绑定一个本地地址并在本地socket支持的情况下对socket进行配置使其处于监听状态。该函数对应本地socket的 bind以及listen功能。一旦CFSocket对象绑定地址，依赖于socket的协议，其它进程和主机能连接到该CFSocket对象。


3、Sockets的使用
> 3.1 CFSocketConnectToAddress
> 功能：打开与一个远程socket的一个连接。
> 语法：
> SWIFT
> func CFSocketConnectToAddress(_ s: CFSocket!,
>                             _ address: CFData!,
>                             _ timeout: CFTimeInterval) -> CFSocketError
> 3.2 CFSocketCreateRunLoopSource
> 语法：
> SWIFT
> func CFSocketCreateRunLoopSource(_ allocator: CFAllocator!,
>                                _ s: CFSocket!,
>                                _ order: CFIndex) -> CFRunLoopSource!
> 功能：为一个CFSocket对象创建一个CFRunLoopSource对象。该创建的 CFRunLoopSource对象不自动添加到一个run loop。为了增加该run loop source到某个run loop，需要调用CFRunLoop对象 的CFRunLoopAddSource函数来为该CFRunLoop对象添加run loop source。
> 3.3 CFSocketGetTypeID
> 功能：返回CFSocket对象的 opaque类型对应的类型标示符。
> 语法：
> SWIFT
> func CFSocketGetTypeID() -> CFTypeID 
> 3.4  CFSocketInvalidate
> 功能：使一个CFSocket对象无效，使其停止接收和发送任何消息。
> 语法：
> SWIFT
> func CFSocketInvalidate(_ s: CFSocket!)
> 3.5 CFSocketIsValid
> 功能：返回一个指示一个CFSocket对象是否有效及是否能够发送和接收消息的布尔值。
> 语法：
> SWIFT
> func CFSocketIsValid(_ s: CFSocket!) -> Boolean
> 3.6 CFSocketSendData
> 功能：该函数用来通过一个CFSocket对象发送数据。
> 语法：
> SWIFT
> func CFSocketSendData(_ s: CFSocket!,
>                     _ address: CFData!,
>                     _ data: CFData!,
>                     _ timeout: CFTimeInterval) -> CFSocketError


二、stream编程模式
       stream编程模式提供了与 unix 的文件操作类似的模式。首先创建和设置流，接着打开流，然后读写流，在流存在时还可以通过查询流的相关属性来读取流的相关信息，在流使用完毕后关闭流。
       iOS 为stream编程模式提供的api编程接口包括两大类，一类是Core Foundation框架层用C语言实现的CFStream  API（包括CFStream、 CFReadStream 、CFWriteStream等）,一类是基于其上的在Foundation框架层用Objective-C语言实现的NSStream API（包括NSStream、NSInputStream NSOutputStream等）,两者提供相似的接口和行为，其中某些对象是toll-free
 bridged类型的，如CFStream 与NSStream，CFReadStream与NSInputStream，CFWriteStream与NSOutputStream之间，因此可以混合使用。
       开发人员可以根据自己的语言偏好选择使用。
       CFStream API的主要接口：
1、CFStream 创建接口
> 1.1  CFStreamCreatePairWithPeerSocketSignature
          功能：创建一对到一个socket的可读和可写流。
> 1.2 CFStreamCreatePairWithSocketToHost
> 功能：创建连接到一个特定主机的特定端口的一对可读写流。
> 1.3 CFStreamCreatePairWithSocket
> 功能：创建一对连接到一个socket的可读写流
> 1.4 CFStreamCreateBoundPair
> 功能：创建一对读写流。
> 其它可读写流创建接口：
>  1.5 CFReadStreamCreateForHTTPRequest
>    功能：为一个CFHTTP请求创建一个可读流。
> 1.6 CFReadStreamCreateForStreamedHTTPRequest
> 功能：为一个HTTP请求的body保持在内存的CFHTTP请求创建一个可读流。
> 1.7  CFReadStreamCreateWithFTPURL
> 功能：创建一个FTP可读流
> 1.8  CFWriteStreamCreateWithFTPURL
> 功能：创建一个FTP可读流
2. CFReadStream接口
> 2.1 流的打开和关闭
>       CFReadStreamOpen
       CFReadStreamClose
2.2  读取数据
      CFReadStreamRead
2.3.  调度一个可读流
   CFReadStreamScheduleWithRunLoop(_:_:_:) 
           CFReadStreamUnscheduleFromRunLoop(_:_:_:) 
2.4 检查可读流的属性  
> CFReadStreamCopyProperty(_:_:) 
> CFReadStreamGetBuffer(_:_:_:) 
> CFReadStreamCopyError(_:) 
> CFReadStreamGetError(_:) 
> CFReadStreamGetStatus(_:) 
> CFReadStreamHasBytesAvailable(_:) 
2.5 设置可读流的属性
> CFReadStreamSetClient(_:_:_:_:)
> CFReadStreamSetProperty(_:_:_:) 
 2.6 得到 CFReadStream的 Type ID
                    CFReadStreamGetTypeID()

3.CFWriteStream 相关接口


>      3.1 CFWriteStreamClose(_:) 
>      3.2 CFWriteStreamOpen(_:) 
>      3.3 CFWriteStreamWrite(_:_:_:)
        3.4 CFWriteStreamScheduleWithRunLoop(_:_:_:)
  3.5 CFWriteStreamUnscheduleFromRunLoop(_:_:_:)
        3.6 CFWriteStreamCanAcceptBytes(_:)
>       3.7 CFWriteStreamCopyProperty(_:_:)
>       3.8 CFWriteStreamCopyError(_:)
>       3.9 CFWriteStreamGetError(_:)
>       3.10 CFWriteStreamGetStatus(_:) 
         3.11 CFWriteStreamSetClient(_:_:_:_:)
>   3.12 CFWriteStreamSetProperty(_:_:_:) 
>       3.13 CFWriteStreamGetTypeID()
CFStream API的使用步骤：
> 1） 利用流创建接口创建相关流；
> 2）、调用CFReadStreamSetClient （可读流）或CFWriteStreamSetClient （可写流）来登记要接收的流相关的事件；
> 3）、调用CFReadStreamScheduleWithRunLoop（可读流）或CFWriteStreamScheduleWithRunLoop（可写流）来使在流在一个run loop上进行调度以便接收相关事件；
> 4）、调用CFReadStreamOpen 或CFWriteStreamOpen 来打开已创建的流；
> 5）、在读取流的创建时登记的回调中，在接收到kCFStreamEventHasBytesAvailable事件时来读取数据， 在可写流已登记的回调中，在接收到kCFStreamEventCanAcceptBytes 事件时开始发送数据或请求；
> 6） 数据传输完成，关闭和释放打开和创建的相关流；
2、NSStream   API的使用>  在ios 中由于NSStream类不支持 与一个远程主机连接，而CFStream支持，因此为了使用 NSStream，你需要使用流创建函数CFStreamCreatePairWithSocketToHost或CFStreamCreatePairWithSocketToCFHost来打开一个与远程主机连接的socket并分配一对CFStream 对象（CFReadStream和CFWriteStream），并cast这些对象到NSStream
>  对象（对应NSInputStream 和 NSOutputStream）。从而可以使用NSStream类的相关接口进行相关网络编程。如设置接收网络事件的代理对象，调度到当前的run loop，然后打开它们进行相应处理。
代码片段如下：
**[objc]** [view
 plain](http://blog.csdn.net/goohong/article/details/40505291# "view plain")[copy](http://blog.csdn.net/goohong/article/details/40505291# "copy")[![在CODE上查看代码片](https://code.csdn.net/assets/CODE_ico.png)](https://code.csdn.net/snippets/498585 "在CODE上查看代码片")[![派生到我的代码片](https://code.csdn.net/assets/ico_fork.svg)](https://code.csdn.net/snippets/498585/fork "派生到我的代码片")1. {  
2.   
3.         NSURL *website = [NSURL URLWithString:urlStr];  
4.   
5.         if (!website) {  
6.   
7.             NSLog(@"%@ is not a valid URL");  
8.   
9.             return;  
10.   
11.         }  
12.   
13.         CFReadStreamRef readStream;  
14.   
15.         CFWriteStreamRef writeStream;  
16.   
17.         CFStreamCreatePairWithSocketToHost(NULL, (CFStringRef)[website host], 80, &readStream, &writeStream);  
18.   
19.         NSInputStream *inputStream = (__bridge_transfer NSInputStream *)readStream;  
20.   
21.         NSOutputStream *outputStream = (__bridge_transfer NSOutputStream *)writeStream;  
22.   
23.         [inputStream setDelegate:self];  
24.   
25.         [outputStream setDelegate:self];  
26.   
27.         [inputStream scheduleInRunLoop:[NSRunLoop currentRunLoop] forMode:NSDefaultRunLoopMode];  
28.   
29.         [outputStream scheduleInRunLoop:[NSRunLoop currentRunLoop] forMode:NSDefaultRunLoopMode];  
30.   
31.         [inputStream open];  
32.   
33.         [outputStream open];  
34.   
35.   
36.         /* Store a reference to the input and output streams so that 
37.  
38.            they don't go away.... */  
39.   
40.         ...  
41.   
42. }  


       在NSStream对象打开后，当接收到相关的stream-event网络消息，其代理对象中的handleEvent: 函数被调用，从而进行流相关的网络消息处理，  如发送相关协议的请求或接收应答等。以下为handleEvent: 函数进行事件处理的代码片段：
   
**[objc]** [view
 plain](http://blog.csdn.net/goohong/article/details/40505291# "view plain")[copy](http://blog.csdn.net/goohong/article/details/40505291# "copy")[![在CODE上查看代码片](https://code.csdn.net/assets/CODE_ico.png)](https://code.csdn.net/snippets/498585 "在CODE上查看代码片")[![派生到我的代码片](https://code.csdn.net/assets/ico_fork.svg)](https://code.csdn.net/snippets/498585/fork "派生到我的代码片")1. - (void)stream:(NSStream *)stream handleEvent:(NSStreamEvent)eventCode {  
2.   
3.     NSLog(@"stream:handleEvent: is invoked...");  
4.   
5.    
6.   
7.     switch(eventCode) {  
8.   
9.         case NSStreamEventHasSpaceAvailable:  
10.   
11.         {  
12.   
13.             if (stream == oStream) {  
14.   
15.                 NSString * str = [NSString stringWithFormat:  
16.   
17.                     @"GET / HTTP/1.0\r\n\r\n"];  
18.   
19.                 const uint8_t * rawstring =  
20.   
21.                     (const uint8_t *)[str UTF8String];  
22.   
23.                 [oStream write:rawstring maxLength:strlen(rawstring)];  
24.   
25.                 [oStream close];  
26.   
27.             }  
28.   
29.             break;  
30.   
31.         }  
32.   
33.         // continued ...  
34.   
35.     }  
36.   
37. }  



三、url 编程模式
         url 编程模式通过URL 的方式来实现网络编程，任何要存取的网络资源（包括局域网和广域网）都可以用一个URL来表示和存取，并支持设备间的资源共享。url 编程模式系统提供http, https, file, ftp, data等五种协议支持，并允许用户自己开发和登记相关类来支持另外的应用层网络协议，进行协议的扩展。
        url 编程模式在IOS系统可以使用两种编程接口：NSURLSession 和NSURLConnection。
        对于iOS 7 以后的最新系统推荐使用NSURLSession API，对于老版本由于不支持NSURLSession，因此必须使用NSURLConnection API。
NSURLSession编程模式是对相关的连接请求通过一个会话来完成，应用通过创建一系列sessions来实现网络通讯，每一个session协调一组相关数据的传输任务。在每一个session内，应用添加一系列任务，每一个任务表现一个特定URL 请求。
        NSURLSession相比NSURLConnection的优点是支持在应用挂起、停止或crashed时能够在后台继续下载数据，即支持任务的取消、重启（恢复）、挂起，以及支持从已挂起、取消或失败的下载中重新恢复下载的能力。
         对于简单的请求，还可以直接通过一个简单的NSURL对象来发出请求，并使用一个NSData内存对象或者一个文件的方式来引出NSURL指向的内容。而NSURLConnection API只能通过构造一个NSURLRequest对象或其子类来发出URL请求来请求下载或上传URL数据。 使用一个NSURLRequest请求对象封装一个URL请求，例如HTTP协议方法，除了可以封装一些协议特定的属性外，还可以规定任意本地cached数据的使用策略。
         对于NSURLRequest请求对象的应答包括两部分：描述内容的元数据metadata及内容数据本身。两种API对于使用NSURLRequest请求接收的元数据metadata都由NSURLResponse类来封装，其中包含MIME类型、内容长度、编码及提供应答的URL等内容。NSURLResponse协议特定的子类还能提供额外的元数据，如NSHTTPURLResponse提供协议头和WEB服务器返回的状态码
 等信息。
         NSURLSession API的使用：

         NSURLSession类支持三种会话类型（默认会话类型、临时会话、后台会话）以及三种类型的任务（数据任务、下载任务、上传任务）。
         数据任务使用NSData 对象来发送和接收内存数据，不存储数据到一个文件，因此不支持后台会话。
         下载任务以一个文件的形式引出数据，并支持在应用没有运行时的后台下载。
         上传任务用来上传数据（文件），也能够支持应用没有运行时的后台上传。
          默认会话和后台会话的区别是后台会话使用一个分离的进程处理所有的数据传输任务，并带有一些限制：后台会话必须使用特定应用代理来提供事件提交，并仅支持HTTP和HTTPS 协议，不支持其它定制协议，并仅支持上传和下载任务，不支持数据任务。
         临时会话不存储任何数据到磁盘，所有接收的内容都保存到与会话关联的RAM中，当会话无效时，RAM中接收的内容自动被清除。
   NSURLSession API的使用步骤：
   1 、创建一个NSURLSessionConfiguration配置对象
> NSURLSessionConfiguration配置对象提供广泛的配置选项，包括：
> 1）、特定于单个会话的私有数据存储，包括caches, cookies, credentials, 和protocols；
> 2）、与一个特定请求或一个会话关联的Authentication；
> 3）、与一个主机的最大连接数；
> 4）、与一个资源关联的超时；
> 5）、最小和最大TLS版本支持；
> 6）、定制的代理词典；
> 7）、cookie策略的控制；
> 8）、HTTP pipelining行为的控制
2、根据配置创建相应的NSURLSession；
       如下代码片段展示了根据不同的配置对象创建不同类型的NSURLSession会话对象。
         
**[objc]** [view
 plain](http://blog.csdn.net/goohong/article/details/40505291# "view plain")[copy](http://blog.csdn.net/goohong/article/details/40505291# "copy")[![在CODE上查看代码片](https://code.csdn.net/assets/CODE_ico.png)](https://code.csdn.net/snippets/498585 "在CODE上查看代码片")[![派生到我的代码片](https://code.csdn.net/assets/ico_fork.svg)](https://code.csdn.net/snippets/498585/fork "派生到我的代码片")1. /* Create a session for each configurations. */  
2.     self.defaultSession = [NSURLSession sessionWithConfiguration: defaultConfigObject delegate: self delegateQueue: [NSOperationQueue mainQueue]];  
3.     self.backgroundSession = [NSURLSession sessionWithConfiguration: backgroundConfigObject delegate: self delegateQueue: [NSOperationQueue mainQueue]];  
4.     self.ephemeralSession = [NSURLSession sessionWithConfiguration: ephemeralConfigObject delegate: self delegateQueue: [NSOperationQueue mainQueue]];  



       NSURLSession API通过代理来实现异步URL内容存取，代理可以是系统提供的代理，还可以是应用提供的特定代理对象。任务对象当从服务器接收到数据或传输完成时调用这些代理对象的方法。
     在创建会话指定相应的代理对象。
3、为会话添加任务；
使用如下方法来添加数据任务到一个会话。

> dataTaskWithURL(_:) 
> dataTaskWithURL(_:completionHandler:) 
> dataTaskWithRequest(_:) 
> dataTaskWithRequest(_:completionHandler:) 

使用如下方法来添加下载任务到一个会话。
> downloadTaskWithURL(_:) 
> downloadTaskWithURL(_:completionHandler:) 
> downloadTaskWithRequest(_:) 
> downloadTaskWithRequest(_:completionHandler:) 
> downloadTaskWithResumeData(_:) 
> downloadTaskWithResumeData(_:completionHandler:) 


使用如下方法来添加上传任务到一个会话
> uploadTaskWithRequest(_:fromData:) 
> uploadTaskWithRequest(_:fromData:completionHandler:) 
> uploadTaskWithRequest(_:fromFile:) 
> uploadTaskWithRequest(_:fromFile:completionHandler:) 
> uploadTaskWithStreamedRequest(_:) 
         如下是数据任务创建代码片段：     
**[objc]** [view
 plain](http://blog.csdn.net/goohong/article/details/40505291# "view plain")[copy](http://blog.csdn.net/goohong/article/details/40505291# "copy")[![在CODE上查看代码片](https://code.csdn.net/assets/CODE_ico.png)](https://code.csdn.net/snippets/498585 "在CODE上查看代码片")[![派生到我的代码片](https://code.csdn.net/assets/ico_fork.svg)](https://code.csdn.net/snippets/498585/fork "派生到我的代码片")1. NSURL *url = [NSURL URLWithString: @"http://www.example.com/"];  
2.   
3. NSURLSessionDataTask *dataTask = [self.defaultSession dataTaskWithURL: url];  
4.   
5. [dataTask resume];  

       下面是下载任务创建代码片段：         
**[objc]** [view
 plain](http://blog.csdn.net/goohong/article/details/40505291# "view plain")[copy](http://blog.csdn.net/goohong/article/details/40505291# "copy")[![在CODE上查看代码片](https://code.csdn.net/assets/CODE_ico.png)](https://code.csdn.net/snippets/498585 "在CODE上查看代码片")[![派生到我的代码片](https://code.csdn.net/assets/ico_fork.svg)](https://code.csdn.net/snippets/498585/fork "派生到我的代码片")1. NSURL *url = [NSURL URLWithString: @"https://developer.apple.com/library/ios/documentation/Cocoa/Reference/"  
2.   
3.               "Foundation/ObjC_classic/FoundationObjC.pdf"];  
4.   
5. NSURLSessionDownloadTask *downloadTask = [self.backgroundSession downloadTaskWithURL: url];  
6.   
7. [downloadTask resume];  

4、使用代理方法接收数据及状态信息


> 会话的数据任务在使用应用特定代理接收数据时必须实现如下两个代理方法：
> URLSession:dataTask:didReceiveData: 
> 一次一片的提供请求的数据给会话任务。
> URLSession:task:didCompleteWithError:
> 指示请求数据已经全部接收。
> 
> 
> 会话的下载任务在下载文件时应该实现如下代理方法：
> URLSession:downloadTask:didFinishDownloadingToURL:
> 下载内容存储到一个URL指定的一个临时文件，在该方法返回之前，必须把临时文件的内容移到一个永久位置，而临时文件被删除。
> URLSession:downloadTask:didWriteData:totalBytesWritten:totalBytesExpectedToWrite: 
> 为应用提供关于当前下载进度的状态信息
> URLSession:downloadTask:didResumeAtOffset:expectedTotalBytes:
> 告诉应用已经从从先前的失败下载中恢复。
> URLSession:task:didCompleteWithError: 
> 告诉应用下载已经失败。
> 
> 
> 下载失败恢复处理：
> 在应用使用cancelByProducingResumeData: 方法取消下载任务时，可以使用downloadTaskWithResumeData: 或 downloadTaskWithResumeData:completionHandler:方法重新创建一个新下载任务并传送cancelByProducingResumeData:产生的恢复数据从而接着继续下载。
> 在传输失败时，如果任务可恢复，则调用URLSession:task:didCompleteWithError: 方法。在传送给URLSession:task:didCompleteWithError: 方法的参数NSError中的 userInfo 词典中包含键值为NSURLSessionDownloadTaskResumeData的恢复数据，因此可以使用downloadTaskWithResumeData:
>  或 downloadTaskWithResumeData:completionHandler:方法重新创建一个新下载任务来接着恢复数据继续下载。
> 
> 
> 系统代理仅能支持基本的URL资源存取任务，不支持认证和后台下载，并且还必须提供一个completion handler block来把返回的URL数据提交到应用。如下是一个使用系统代理的代码例子：         
**[objc]** [view
 plain](http://blog.csdn.net/goohong/article/details/40505291# "view plain")[copy](http://blog.csdn.net/goohong/article/details/40505291# "copy")[![在CODE上查看代码片](https://code.csdn.net/assets/CODE_ico.png)](https://code.csdn.net/snippets/498585 "在CODE上查看代码片")[![派生到我的代码片](https://code.csdn.net/assets/ico_fork.svg)](https://code.csdn.net/snippets/498585/fork "派生到我的代码片")1. NSURLSession *delegateFreeSession = [NSURLSession sessionWithConfiguration: defaultConfigObject delegate: nil delegateQueue: [NSOperationQueue mainQueue]];  
2.   
3. [delegateFreeSession dataTaskWithURL: [NSURL URLWithString: @"http://www.example.com/"]  
4.   
5.                   completionHandler:^(NSData *data, NSURLResponse *response,  
6.   
7.                                       NSError *error) {  
8.   
9.                       NSLog(@"Got response %@ with error %@.\n", response, error);  
10.   
11.                       NSLog(@"DATA:\n%@\nEND DATA\n",  
12.   
13.                             [[NSString alloc] initWithData: data  
14.   
15.                                     encoding: NSUTF8StringEncoding]);  
16.   
17.                   }] resume];  

          会话的上传任务的任务创建和相关代理方法：
          会话的上传任务使用HTTP  POST方法来上传数据。可以以一个NSData对象、一个文件或使用一个流为HTTP POST请求的body提供内容。
          在以NSData对象提供上传数据时，应用调用uploadTaskWithRequest:fromData: 或uploadTaskWithRequest:fromData:completionHandler: 方法来创建上传任务。
> 在以文件形式提供上传数据时，应用调用uploadTaskWithRequest:fromFile: 或 uploadTaskWithRequest:fromFile:completionHandler:方法来创建上传任务
> 在以流方式提供上传数据时，应用调用uploadTaskWithStreamedRequest:方法来创建上传任务。
> 应用特定代理可以通过实现URLSession:task:didSendBodyData:totalBytesSent:totalBytesExpectedToSend:方法来获得上传进度信息。
