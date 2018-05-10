# iOS Runtime


> 

### 一、isa 指针
在了解 runtime 之前，我们先介绍一下 isa 指针。

在 Objective-C 中，任何类的定义都是对象。类和类的实例（对象）没有任何本质上的区别，任何对象都有 isa 指针。

在源码中，是怎么定义类的呢？（objc.h 文件）
	
	typedef struct objc_class *Class;

	@interface NSObject <NSObject> {
	    Class isa  OBJC_ISA_AVAILABILITY;
	}

可以看出 Class 是一个 objc_class 结构体类型的指针

然后，咱们来看看 objc_class 在的源码中的定义：
	
	
	struct objc_class {
	    Class isa  OBJC_ISA_AVAILABILITY;
	#if !__OBJC2__
	    Class super_class                                        OBJC2_UNAVAILABLE;
	    const char *name                                         OBJC2_UNAVAILABLE;
	    long version                                             OBJC2_UNAVAILABLE;
	    long info                                                OBJC2_UNAVAILABLE;
	    long instance_size                                       OBJC2_UNAVAILABLE;
	    struct objc_ivar_list *ivars                             OBJC2_UNAVAILABLE;
	    struct objc_method_list **methodLists                    OBJC2_UNAVAILABLE;
	    struct objc_cache *cache                                 OBJC2_UNAVAILABLE;
	    struct objc_protocol_list *protocols                     OBJC2_UNAVAILABLE;
	#endif
	} OBJC2_UNAVAILABLE;


来看看各个参数的意思：

isa： 是一个 Class 类型的指针，每个实例对象都有 isa 指针，他指向对象的类，而 Class 中也有 isa 指针，指向 metaClass（元类）。元类中保存了类方法的列表。当类方法被调用时，先会从本身查找类方法的实现，如果没有，元类会向他的父类查找该方法，如此进行。最终会有一个 root Class （根类）即 NSObject

super_class： 父类，如果为根类，则为null

version：类的版本信息,默认为0

info：供运行期使用的一些位标识。

instance_size：该类的实例变量大小

ivars：成员变量的数组

现在来看看个各类、对象、元类之间的关系（图中，实线是 super_class 指针，虚线则是 isa 指针）：

![](https://github.com/MineJay/iOS-Note/blob/master/iOS%20%E5%AD%A6%E4%B9%A0%E9%83%A8%E5%88%86/Runtime/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-05-09%20%E4%B8%8B%E5%8D%887.58.25.png)



### 二、runtime

那么，什么是 runtime 呢？苹果官方的文档说法是这样的：

	The Objective-C language defers as many decisions as it can from compile time and link time to runtime.
	（尽量将决定放到运行的时候，而不是在编译和链接过程）


我们都知道，从代码到可执行文件，可以将其过程简化为三个方面：

	1. 编译
	2. 链接
	3. 运行


编译：将代码转化成底层可执行的语言（如汇编），简单来说，就是把你能看懂的语言，转化为系统底层能看懂的东西。这中间通常会有优化、预处理、再编译等

链接：在编译的过程中，如果有调用其他的类的方法的时候，是不会发生警告或者错误的，因为在编译的时候，默认你已经实现了该方法，而链接要做的工作就是去检查你要调用的方法或者类是否存在。

运行：执行最终的可执行文件

稍微低级一点的语言，如 C 语言，一个函数的执行内容，在编译阶段就已经确定了。

而在 runtime 中，编译阶段，只能确认要最终执行的函数名，具体执行什么，实在运行的时候才能确定，这也就大大增加了程序的灵活性。


### Objective-C runtime 简介

Objective-C是一门运行时语言，这意味着代码执行可以更加灵活：我们动态的创建一个新的类，还可以转发消息给其他的消息。（消息转发是runtime的一个重要组成部分，后面会介绍）。

这种特性要求Objective-C语言会尽可能把决定从编译和链接的阶段延迟到运行时(runtime)阶段，因此Objective-C不仅有一个编译器，还有一个runtime系统来执行被编译过的代码


### Runtime 交互
有三种方式可以使用 runtime：

	1. Objective-C 源代码
	2. NSObject 方法
	3. Runtime 方法

### Objective-C 源代码
一般来说，runtime 都是默默地在后台运行工作，我们只是写 Objective-C 源代码，就使用了 runtime。当我们编译包含Objective-C的类和方法时，编译器就会生成包含 runtime 特性的数据结构和方法。数据结构中包含了从类、category、协议中定义的的信息，如：selector、变量等等。主要的 runtime 方法是发送信息的方法 Message,在下一章节会讲到。

### NSObject方法
Cocoa 中大部分的类都是继承自 NSObejct，所以他们都会集成了 NSObject 定义的方法。（值得注意的例外是 NSProxy 类）因此NSObject 的方法就决定了其他类的行为。（当然，在少数情况下，这样说并不正确，在这些情况下，NSObject 会仅仅定义了方法，没有实现必须的代码。比如 description，如果子类没有重写，那么会返回类名和地址，这是因为 NSObject 没法获得更多的信息。）

一些NSObject类的方法可以直接查询 runtime 系统的信息，从而获得自身的信息。比如class，isKindeOfClass，respondsToSelector等方法。

### Runtime方法
Runtime 系统是一个共享的 library，包含了许多方法和数据结构，地址在/usr/include/objc。其中的很多方法，允许你使用C来重写编译行为，其他的方法是通过 NSObject 类使用。有了这些方法，我们就可以为 runtime 系统增加接口，或者工具。


### runtime的应用

	1. 动态创建一个类(比如KVO的底层实现)
	
	2. 动态地为某个类添加属性\方法, 修改属性值\方法
	
	3. 遍历一个类的所有成员变量(属性)\所有方法
	
	实质上，以上的是通过相关方法来获取对象或者类的isa指针来实现的。








































