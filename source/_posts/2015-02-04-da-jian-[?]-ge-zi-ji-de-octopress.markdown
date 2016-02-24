---
layout: post
title: "iOS面试题目"
date: 2015-03-03 22:35:24 +0800
comments: true
thumbnail: /img/2015/06/3.jpg
categories: 
---
面试的一些经典问题，基础内容，看自己学了多少忘记了多少<!--more-->
###1.#import和#include的区别，@class的作用是什么？  
	    #import与#include在引用一个类的时候会包含这个类的所有信息包括变量方法等，但是这样做会对编译效率造成影响。比如由100个类都#import了ClassA，那么在编译的时候这100个类都会对ClassA处理，又比如A被B引用，B被C引用，C被D引用...此时如果A被修改，那么后面的B，C，D。。。都需要重新进行编译。还有一个用法会引起编译错误的就是在ClassA中#import ClassB在ClassB中#import ClassA那么在编译的时候也会出现未知错误。    
	    所以一般来说，在interface中引用一个类，就用@class，它会把这个类作为一个类型来使用，而在实现这个interface的文件中，如果需要引用这个类的实体变量或者方法之类的，还是需要import这个在@class中声明的类。
	    #import比起#include的好处就是不会引起重复包含。
###2.方法前加号和减号的区别  
	加号：类方法  
	减号：实例方法

###3.MVC是什么？有什么特性？ 
	MVC是一种设计模式，由模型、视图、控制器3部分组成。  
	模型：保存应用程序数据的类，处理业务逻辑的类   
	视图：窗口，控件和其他用户能看到的并且能交互的元素  
	控制器：将模型和试图绑定在一起，确定如何处理用户输入的类

###4.obj-c有多重继承么?不是的话有什么替代方法?
	oc不支持多重继承，用protocol委托代理来替代

###5.NSNotification和KVO的区别和用法是什么？什么时候应该使用通知，什么时候应该使用KVO，它们的实现上有什么区别吗？如果用protocol和delegate（或者delegate的Array）来实现类似的功能可能吗？如果可能，会有什么潜在的问题？如果不能，为什么？

	KVO只能监测属性的变化，通过NSString类型的属性名来实现。但是实现了自动监测，当属性值变化时，会自动通知观察者，不用再添加代码了。 NSNotification比较灵活，可以监测的内容较多，但是需要被观察者手动发送通知，观察者才能响频。 protocol通过添加一个NSArray也能实现类似的功能，但是实现上需要自己处理delegate的添加与删除，自己在属性变化时手动通知，较繁琐，易出错。

###6.您是否做过异步的网络处理和通讯方面的工作？如果有，能具体介绍一些实现策略么？

	做过。 通过注册代码或者block的方式，实现回调。在网络处理方面，统一处理出错的情况，没出错的情况下，将数据分别发送给接收者。

###7.你实现过一个框架或者库以供别人使用么？如果有，请谈一谈构建框架或者库时候的经验；如果没有，请设想和设计框架的public的API，并指出大概需要如何做、需要注意一些什么方面，来使别人容易地使用你的框架。

	曾经移植过一个框架，把C++的一套类库移植到OC上面，主要工作就是做一个oc++的接口层。做的过程中，遇到的问题就是在原来框架中，很多头文件中用结构体或者类的地方，没有用指针，导致不能用声明的方式来使用类和结构体，必须在头文件中把其它头文件导入，这样导致整个接口需要提供的头文件太多了。 还封装过供他人调用的接口。建议就是调用方法尽可能简单，做好传入参数的安全检查及错误提醒。因为你无法确定你的调用者给你传什么样的数据进来。如果实现方法中耗时较长，需要用异步的方式进行结果返回，可以选用delegate或者block的方式。
	参见唐巧的[http://blog.devtang.com/blog/2015/01/31/write-sdk-tips/](博客)

###8.NSOperation vs Grand Central Dispatch
	GCD is a low-level C-based API that enables very simple use of a task-based concurrency model. NSOperation and NSOperationQueue are Objective-C classes that do a similar thing. NSOperation was introduced first, but as of 10.6 and iOS 4, NSOperationQueue and friends are internally implemented using GCD.

		In general, you should use the highest level of abstraction that suits your needs. This means that you should usually use NSOperationQueue instead of GCD, unless you need to do something that NSOperationQueue doesn't support.

		Note that NSOperationQueue isn't a "dumbed-down" version of GCD; in fact, there are many things that you can do very simply with NSOperationQueue that take a lot of work with pure GCD. (Examples: bandwidth-constrained queues that only run N operations at a time; establishing dependencies between operations. Both very simple with NSOperation, very difficult with GCD.) Apple's done the hard work of leveraging GCD to create a very nice object-friendly API with NSOperation. Take advantage of their work unless you have a reason not to.

		Caveat: On the other hand, if you really just need to send off a block, and don't need any of the additional functionality that NSOperationQueue provides, there's nothing wrong with using GCD. Just be sure it's the right tool for the job.

###9.static的作用  
	作用范围为该函数体，不同于auto变量。
	1）该变量的内存只被分配一次，因此其值在下次调用时扔维持上次的值；
	2）在模块内的static全局变量尅被模块内所用函数访问，但不能被模块外其他函数访问；
	3）在模块内的static函数只可被这一模块内的其他函数调用，这个函数的使用范围被限制在它的模块内；
	4）在类中的static成员变量属于整个类所拥有，对类的所有对象只有一份拷贝；
	5）在类中的static成员函数属于整个类所拥有，这个函数不接收this指针，因而只能访问类的static成员变量。

###10.const的作用
	const意味着“只读”，例子：
	const int a;
	int const a;
	const int *a;
	int * const a;
	int const *a const;
	前两个作用一样，a是一个长整数型，第三个意味着a是一个指向常整数型的指针；第四个a是一个指向整数型的常指针；第五个a是一个指向常整数型的常指针；
	欲阻止一个变量被改变，可以使用 const 关键字。
	（1）在定义该 const 变量时，通常需要对它进行初始化，因为以后就没有机会再去改变它了；
	（2）对指针来说，可以指定指针本身为 const，也可以指定指针所指的数据为 const，或二者同时指
	定为 const；
	（3）在一个函数声明中，const 可以修饰形参，表明它是一个输入参数，在函数内部不能改变其值；
	（4）对于类的成员函数，若指定其为 const 类型，则表明其是一个常函数，不能修改类的成员变量；
	（5）对于类的成员函数，有时候必须指定其返回值为 const 类型，以使得其返回值不为“左值”。
