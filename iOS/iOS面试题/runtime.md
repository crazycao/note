# runtime

## runtime是什么？
- OC是一门动态性比较强的编程语言，允许很多操作推迟到程序运行时再进行
- OC的动态性就是由Runtime来支撑和实现的
- Runtime是一套C语言的API，封装了很多动态性相关的函数
- 平时编写的OC代码，底层都是转换成了Runtime API进行调用

## 讲一下 OC 的消息机制
- OC中的方法调用其实都是转成了`objc_msgSend`函数的调用，给receiver（方法调用者）发送了一条消息（selector方法名）
- objc_msgSend底层有3大阶段：
	- 消息发送（在类及其父类的方法列表中查找）
		- objc在向一个对象发送消息时，runtime库会根据对象的 isa 指针找到该对象实际所属的类
		- 然后在该类中的方法列表以及其父类方法列表中寻找方法运行 
	- 动态方法解析（动态添加方法）
		- 没有找到要执行的方法时，会调用类方法 `+ (BOOL)resolveInstanceMethod:(SEL)sel`来动态为其新增实例方法以处理该 selector
			- 通过 `class_addMethod` 添加实例方法。  
	- 消息转发（重定向、转发）
		- 当动态方法解析不作处理，`resolveInstanceMethod:` 方法返回 `NO` 时，消息转发机制会被触发。
		- 在消息转发机制执行前，通过重载 `- (id)forwardingTargetForSelector:(SEL)aSelector` 方法替换消息的 receiver 为其他对象。
		- 如果此方法返回 `nil`或`self`，则会进入消息转发机制；否则将向返回的对象重新发送消息。
		- 消息转发时，`forwardInvocation:`方法会被执行，我们可以重写这个方法来定义我们的转发逻辑：
			- 可以将一个消息“翻译”成另外一个消息。
			- 也可以简单的“吃掉”某些消息，因此没有响应也没有错误。

## 平时开发中哪些地方会用到 runtime ？

- 利用关联对象（AssociatedObject）给分类添加属性

	```
	// 动态添加属性的本质是：让对象的某个属性与值产生关联
    // 保存name
    objc_setAssociatedObject(self, "name", name, OBJC_ASSOCIATION_RETAIN_NONATOMIC);
    
    //读取name
    objc_getAssociatedObject(self, "name");
  ```
- 遍历类的所有成员变量
	- 字典转模型
	- 修改textfield的占位文字颜色
	- 自动归档解档
- 交换方法实现（交换系统的方法）
- 利用消息转发机制解决方法找不到的异常问题，异常捕获，避免Crash
- 实现多重继承（OC本身是不支持多重继承的）
	- 让子类同时继承A类和B类的协议
	- 在调用方法时，将消息转发到A类和B类里

## runtime 如何通过 selector 找到对应的 IMP 地址？
> IMP：函数指针，指向 objetive-C 方法(method)实现代码块的地址。

- 每一个类对象中都一个对象方法列表（对象方法缓存）。
- 类方法列表是存放在类对象中 isa 指针指向的元类对象中（类方法缓存）。
- 方法列表中每个方法结构体中记录着方法的名称，方法实现，以及参数类型。

	```
	struct objc_method
	{
  		SEL method_name; // 方法名。方法名为此方法的签名，有着相同函数名和参数名的方法有着相同的方法名。
  		char * method_types; // 方法类型。方法类型描述了参数的类型。
  		IMP method_imp; // IMP即函数指针，为方法具体实现代码块的地址，可像普通C函数调用一样使用IMP。
	};
	typedef objc_method Method; // 方法结构体
	```
- selector 本质就是方法名称，通过这个方法名称就可以在方法列表中找到对应的方法实现。

	```
	SEL method=@selector(func1);
	```
- （实例方法）当我们发送一个消息给一个 NSObject 对象时，这条消息会在对象的类对象方法列表里查找。
- （类方法）当我们发送一个消息给一个类时，这条消息会在类的元类对象的方法列表里查找。


## runtime如何实现 weak 变量的自动置 nil ？

- runtime 会将 weak 对象放入一个 hash 表中。
- 用 weak 指向的对象内存地址作为 key，Value是Weak指针的地址的数组。（假设 weak 指向的对象内存地址是a，那么就会以 a 为 key）
- 当此对象的引用计数为0的时候会调用对象的 dealloc 方法，runtime 在这个 weak hash 表中搜索，找到所有以 a 为 key 的 weak 对象，从而设置为 nil。

## 什么是method swizzling（俗称黑魔法）

- method swizzling 即进行方法交换。
- 交换方法的几种实现方式
	- 利用 method_exchangeImplementations 交换两个方法的实现 IMP
	- 利用 class_replaceMethod 替换方法的实现 IMP
	- 利用 method_setImplementation 来直接设置某个方法的实现 IMP

## 类的结构、实例对象的结构

```
typedef struct objc_class *Class; // Class本身其实也是一个对象，称为“类对象”
typedef struct objc_object *id;   // NSObject 真正到底层的 是一个 objc_object（C/C++）的结构体类型
```
```
struct objc_class : objc_object { // objc_class 继承自 objc_object
    // Class ISA;				  // 该变量来自于父类，所以这里注释掉
    Class superclass;			  // 父类的指针
    const char *name ; 			  // 类名
 	long version ; 				  // 类的版本信息，初始化默认为0，可以通过runtime函数class_setVersion和class_getVersion进行修改、读取
    long info; 					  // 一些标识信息，如CLS_CLASS (0x1L) 表示该类为普通 class，其中包含对象方法和成员变量;CLS_META (0x2L) 表示该类为 metaclass，其中包含类方法;
	long instance_size ; 		  // 该类的实例变量大小(包括从父类继承下来的实例变量);
    struct objc_ivar_list *ivars; // 实例变量列表。用于存储每个成员变量的地址
    struct objc_method_list **methodLists ; // 方法列表。与 info 的一些标志位有关,如CLS_CLASS (0x1L),则存储对象方法，如CLS_META (0x2L)，则存储类方法;
 
    struct objc_cache *cache; 	  // 指向最近使用的方法的指针，用于提升效率
    struct objc_protocol_list *protocols; // 存储该类遵守的协议
}
```
```
struct objc_object {
private:
    isa_t isa; // 实例对象的 isa 指向其类对象，类对象的 isa 指向其元类，元类的 isa 指向 NSObject
public:
    // ISA() assumes this is NOT a tagged pointer object
    Class ISA();
    ...
}
```


- Class本身其实也是一个对象，我们称之为类对象，类对象在编译期产生用于创建实例对象，是单例
- struct objc_classs结构体里存放的数据称为元数据(metadata)，包括以下：
	- isa 指针
	- 指向父类的指针（supper_class）
	- 类的名字（name）
	- 版本（version）
	- 标识信息，class 或 metaclass（info）
	- 实例变量大小（instanse_size）
	- 实例变量列表（ivars）
	- 方法列表（methodLists）
	- 缓存，指向最近使用的方法（cache）
	- 遵守的协议列表（protocols）
- 类对象中的元数据存储的都是如何创建一个实例的相关信息
- 类对象的 isa 指针指向的我们称之为元类(metaclass)。元类 是系统给的，其定义和创建都是由编译器完成。元类 中保存了创建 类对象 以及 类方法 所需的所有信息。

- 实例对象的 isa 指针是指向 类对象
- 类对象的 isa 指针指向 元类
- 元类的 isa 指针指向 根类（NSObject）
- 根类的 isa 指针指向自己
- 类对象的 super_class 指针指向了父类的类对象
- 元类的 super_class 指针指向了父类的元类

## isKindOfClass 与 isMemberOfClass 的区别

- isKindOfClass

	- 判断当前类或实例是否当前类的子类，会循环对比父类
	- 类方法：`元类 --> 根元类 --> 根类 --> nil` 依次与 `传入类` 的对比
	- 实例方法：`对象的类 --> 父类 --> 根类 --> nil` 依次与 `传入类`的对比

- isMemberOfClass

	- 判断当前类是否传入类的实例，不会循环对比父类
	- 类方法： `类的元类` 与 `传入类` 对比
	- 实例方法：`对象的类` 与 `传入类` 对比

## KVO 的实现原理
> KVO（Key-Value-Observer）也就是观察者模式，是苹果提供的一套事件通知机制。允许对象监听另一个对象特定属性的改变，并在改变时接收到事件。

- KVO是通过 method-swizzling 技术实现的

- 在运行时根据原类创建一个中间类，这个中间类是原类的子类
- 动态修改当前对象的isa指向中间类
- 重写setter方法
	- willChangeValueForKey
	- set的赋值操作
	- didChangeValueForKey
		- 在 didChangeValueForKey 的内部实现中会调用观察者的回调方法，返回被观察对象的相关参数  
- 将class方法重写，返回原类的Class
	
	- 苹果不想暴露kvo的内部实现
	
	- > KVC是Key Value Coding的简称。它是一种可以通过字符串的名字（key）来访问类属性的机制。而不是通过调用Setter、Getter方法访问。注意区分