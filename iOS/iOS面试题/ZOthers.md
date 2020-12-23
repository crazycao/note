# 其他

##  iOS反射机制

iOS反射机制：运行时选择创建哪个实例，并动态选择调用哪个方法。

常用判断方法

```
// 当前对象是否这个类或其子类的实例
- (BOOL)isKindOfClass:(Class)aClass;
// 当前对象是否是这个类的实例
- (BOOL)isMemberOfClass:(Class)aClass;
// 当前对象是否遵守这个协议
- (BOOL)conformsToProtocol:(Protocol *)aProtocol;
// 当前对象是否实现这个方法
- (BOOL)respondsToSelector:(SEL)aSelector;
```

获取Class的三种方法

```
// 通过字符串获取class  
Class class = NSClassFromString(@"NSString");  
NSLog(@"class type : %@", class);  

// 直接用class 来创建对象 ,通过对象来获取class 
id str = [[class alloc] init];   
NSLog(@"%@", [str class]);  

// 通过类来获取class  
NSLog(@"%d", class==NSString.class);   
```

- OC中使用反射的优点
	- 松耦合，类与类之间不需要太多依赖
	- 构建灵活

- OC中使用反射的缺点
	- 不利于维护。使用反射模糊了程序内部实际发生的事情，隐藏了程序的逻辑。这种绕过源码的方式比直接代码更为复杂，增加了维护成本。
	- 性能较差。使用反射匹配字符串间接命中内存比直接命中内存的方式要慢。

## @property的作用

- 自动生成私有属性（一般是下划线+属性名）
- 自动生成 `getter` 和 `setter` 方法

在声明property属性后，有2种实现选择

- @synthesize：表示如果你没有手动实现 setter 方法和 getter 方法，那么编译器会自动为你加上这两个方法
	- Xcode6以后可以省略，会默认在 `implementation.m` 中添加这个 `@synthesize xxx = _xxx`
- @dynamic 告诉编译器：属性的setter与getter方法由用户自己实现，系统不自动生成

## iOS 属性修饰符

> 根据MRC和ARC划分属性修饰符的使用范围
> 
> ```
> MRC：nonatomic,atomic,retain,assign,copy,readwrite,readonly
> ARC：nonatomic,atomic,strong,weak,assign,copy,readwrite,readonly
> ```

- nonatomic
	- nonatomic 表示非原子属性
		- 并发访问性能高，但是访问不安全
			- 它直接访问内存中的地址，不关心其他线程是否在改变这个值，并且中间没有死锁保护；所以可能拿不到完整的值
- atomic 
	- atomic 表示原子属性
		- 系统生成的 getter/setter 会保证 get、set 操作的完整性，不受其他线程影响
			- 系统生成的 getter/setter 方法中，使用了 @synchronized(self)
			- 如果一个线程正在执行 getter/setter，其他线程就得等待
		- 如果有另一个线程同时在调 [property release]，那可能就会crash，因为 release 不受 getter/setter 操作的限制。
			- 也就是说，atomic 修饰的属性只能说是读/写安全的，但并不是线程安全的
				- 因为别的线程还能进行读写之外的其他操作。
				- 线程安全需要开发者自己来保证。 
	- 系统默认的属性是 atomic 
- strong
	- strong 表示对对象的强引用
		- 强引用时，引用计数会 +1
		- 给 strong 属性赋值时，setter 方法中会先 release 旧值再 retain 新值并赋值
		- 两个对象之间相互强引用会造成循环引用，内存泄漏
- weak
	- weak 表示对象的弱引用
		- 弱引用时，不会使传入的对象计数+1
		- 被其修饰的对象随时可能被系统销毁回收
			- 当该对象的引用计数为 0，则会被回收，对象被释放以后，weak 指针会被自动设置为 nil 
				- 在OC的运行时环境中，维护了一种 weak 表（哈希表）
					- 这张哈希表用对象的首地址作为 key，用 weak 指针自身的地址组成的数组作为 value
				- 当对象被释放后，通过这个对象的起始地址来找到所有指向它的 weak 指针，并将它们指向nil
		- weak 多用于避免循环引用
- __unsafe\_unretain 
	- __unsafe\_unretain 与 weak 非常类似
	- 不同之处仅在于，当对象被释放后，指针依然指向之前的内存地址
	- 此时，访问被释放的地址就会 crash，所以说他是不安全的
		- 这个已经被释放了的对象被称为 **僵尸对象**
- assign
	- assign 主要用于修饰基本数据类型
		- 包括OC基本数据类型（NSInteger，CGFloat）和C数据类型（int, float, double, char）
	- 基本数据类型存储在栈中，内存不用程序员管理。
	- assign 也可以修饰对象，特性与 __unsafe\_unretain 相同。
	- assign 在 MRC 和 ARC 下都可以使用
- retain
	- 在 MRC 下使用，用于修饰对象
	- 被 retain 修饰的对象，retainCount要+1
	- retain 只能修饰 OC 对象，不能修饰非 OC 对象（如 Core Foundation 对象）
		- retain 会增加对象的引用计数，而基本数据类型或者 Core Foundation 对象都没有引用计数 
	- 赋值时，先 release 旧值，再 retain 新值
- copy
	- copy 表示在赋值时使用传入值的一份拷贝
		- 赋值时，创建了一个新的对象，并将传入对象的值全部拷贝到新对象
		- 赋值后，新对象的 retainCount 为 1，而旧对象的 retainCount 不变  
	- 只能修饰对象类型，不能修饰基础数据类型
	- 用copy修饰的对象，必须实现
 NSCopying 协议，也就是实现方法 `-(id)copyWithZone:(nullable NSZone *)zone`
 		- 系统自动生成的 setter 方法中会调用这个方法
 		- 至于 copy 是深拷贝还是浅拷贝完全是看 copyWithZone 的实现方式，copy 修饰符和深拷贝、浅拷贝没有关系
	- 一般用于修饰不可变的属性（NSArray,NSDictionary,NSString,block）
		- 即使用 copy 修饰 NSMutableArray，将一个可变 NSMutableArray 赋值给 copy 修饰的属性也会变成不可变数组 NSArray
			- 若 a 被 copy 修饰，则 `a = b` 等价于 `a = [b copy]`。
		- 为什么要用 copy 修饰 NSString
			- 如果用 retain 修饰 NSString，当把 NSMutableString 赋值给 NSString 时，只是拷贝了指针；
				- 如果赋值后源字符串改变，这个属性值也跟着改变。（不可控）
			- 如果用 copy 修饰 NSString，当把 NSMutableString 赋值给 NSString 时，会生成一份拷贝内容；
				- 即使赋值后源字符串改变，这个属性值也不会改变。（保证了安全） 
- readwrite
	- readwrite 表示该属性可读可写
	- 系统会自动创建 setter 和 getter 方法
	- readwrite 是默认的属性修饰符
- readonly 
	- readonly 表示该属性只可读，不可写
	- 只会生成get方法
	- 对用 readonly 修饰的属性赋值时，编译器会报错提示：“Assignment to readonly property”。

## git rebase 与 git merge 有什么区别
- git merge 和 git rebase 都是用来合并两个分支的。
- git merge [branch]：将指定分支合并到当前分支中，如果没有冲突则自动进行新的提交。
- git rebase [branch]：在另一个分支基础之上重新应用修改，把另一个分支上的 commit 都放到当前分支的 commit 之后，就好像当前分支是从另一个分支的最新 commit 上拉出来的一样。（所以称为“变基”）
	- 冲突处理：手动处理完冲突以后，用 `git add` 命令去更新这些内容的索引，然后执行 `git rebase --continue` 继续应用(apply)余下的补丁。
	- 在任何时候，可以用 `git rebase --abort` 参数来终止rebase的操作，并且当前分支会回到rebase开始前的状态。

## 64位与32位的区别
- 处理速度不同（64和32表示CPU一次性可以处理的最大位数。）
	- 64位CPU的数据宽度为64位，处理器一次可提取64位数据。只要两个指令，一次提取8个字节的数据。
	- 32位CPU需要四个指令，一次提取4个字节的数据。比64位的提取速度慢了近一倍。
- 支持的软件不同
	- 32位的CPU，仅支持基于32位的软件。不能运行64位的软件。
	- 64位的CPU可以同时支持两种软件，基本上与各种软件都兼容。尤其现在新的软件版本都只支持64位。
- 支持不同的内存
	- 64位CPU容量大，支持大容量内存，如（ 64G 128G 256G）。假如主板上有足够的内存插口，是可以无限支持的。
	- 32位的CPU，最多只能支持4G的内存，实际容量仅为3.25G。

