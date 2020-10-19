# runloop

## 什么是 Runloop？

- 一个run loop就是一个事件处理的循环。
- 它内部就是do-while循环，在这个循环内部不断地处理各种任务。说通俗来说就是一个死循环。
- 一个线程对应一个RunLoop，主线程的RunLoop默认已经启动，子线程的RunLoop得手动启动。
- 使用run loop的目的是让你的线程在有工作的时候忙于工作，而没工作的时候处于休眠状态。

## NSRunLoop 和 CFRunLoopRef 的区别

- CFRunLoopRef 是在 CoreFoundation 框架内的，它提供了纯 C 函数的 API，所有这些 API 都是线程安全的。

- NSRunLoop 是基于 CFRunLoopRef 的封装，提供了面向对象的 API，但是这些 API 不是线程安全的。

- 两种类型的run loop完全可以混合使用，NSRunLoop类可以通过实例方法`getCFRunLoop`获取对应的CFRunLoopRef类。

## NSTimer准吗？为什么？如果不准该怎样实现一个精确的NSTimer?

- NSTimer不准确。有两个原因：
	- CPU性能及函数执行带来的时间误差。这个误差基本非常小，正常在1毫秒以内。
	- 如果定时器所在线程执行了耗时操作，这期间有可能会错过很多次NSTimer的循环周期，但是NSTimer并不会将前面错过的执行次数在后面都执行一遍，而是继续执行后面的循环。
		- 由于定时器在一个RunLoop中被检测一次，如果在这一次的RunLoop中做了耗时的操作，当前RunLoop持续的时间超过了定时器的间隔时间，那么定时器触发就被延后了。然后再以后的所有触发都相应延后。
- 实现准确的NSTimer的方法：
	- 方法一：在子线程中创建timer，在主线程进行定时任务的操作。
	- 方法二：在子线程中创建timer，在子线程中进行定时任务的操作，需要UI操作时切换回主线程进行操作。

## 为什么当我们拖动UITableView的时候timer方法不执行了？

- 主线程的RunLoop有两种预设的模式，DefaultMode 和 TrackingMode。
	- NSDefaultRunLoopMode：默认模式，优先处理跟踪模式下的事件。
	- NSEventTrackingRunLoopMode：跟踪跟踪来自用户交互的事件（比如UITableView上下滑动）。

- 当定时器被添加到主线程中且无指定模式时，会被默认添加到 DefaultMode 中，一般情况下定时器会正常触发定时任务。

- 但是当用户进行UI交互操作时（比如滑动tableview），主线程会切换到 TrackingMode，此时仍然处在 DefaultMode 下的Timer并不会触发。

- 解决方案是把定时器添加到主线程的 CommonMode 中，或者在子线程中创建定时器。

	```
	[[NSRunLoop mainRunLoop]addTimer:timer forMode:NSRunLoopCommonModes];
	```

	- NSRunLoopCommonModes：一个伪模式，为一组run loop mode的集合。这个模式等效于NSDefaultRunLoopMode 和 NSEventTrackingRunLoopMode的结合。

## Runloop 的生命周期如何？

- Runloop 的生命周期可以分为三步：创建->运行(启动，内部循环)->退出
- 创建
	- 苹果不允许开发人员手动创建runloop，runloop是伴随着线程的创建而创建。
		- 主线程：系统会自动创建runloop。
		- 子线程：系统不会自动创建，开发人员必须显示的调用`[NSRunLoop currentRunLoop]`方法来获取runloop的时候，系统才会创建，类似懒加载。

	- 线程与runloop是一一对应的。
		- 系统只提供了两种方法获取runloop，`currentRunLoop`和`mainRunLoop`。
- 运行
	- 开启
		- 主线程的runloop由系统自动运行。
		- 子线程需要开发人员通过以下方法开启：
			- NSRunLoop提供的方法：
		
				```
				// 默认模式启动，会不断调用 runMode:beforeDate: 方法维持循环
				- (void)run; 
				// 以默认模式启动runloop，并在超过时间限制后退出
				- (void)runUntilDate:(NSDate *)limitDate;
				// 在到达指定时间时，启动一次runloop
				- (BOOL)runMode:(NSRunLoopMode)mode beforeDate:(NSDate *)limitDate;
 				```  
 			- NSRunLoop提供的方法：
							
				```
				/// 默认模式
				void CFRunLoopRun(void);
				/// 在指定模式，指定时间，运行
				CFRunLoopRunResult CFRunLoopRunInMode(CFRunLoopMode mode, CFTimeInterval seconds, Boolean returnAfterSourceHandled);

 				```  
 	- 内部循环
 		- 循环逻辑
 		
 			1. 通知Observer，即将进入循环（kCFRunLoopEntry）
 			2. 通知Observer，即将处理Timer（kCFRunLoopBeforeTimers）
 			3. 通知Observer，即将处理Source（kCFRunLoopBeforeSources）
 			4. 处理Source0
 				- Source0，非Source1的事件，一般是APP内部的事件, 如`hitTest:withEvent:`、`performSelectors`等事件 
 			5. 如果有Source1，跳转到步骤9
 				- Source1是指来自系统内核或者其他进程或线程的事件。
 			6. 通知Observer，线程即将休眠（kCFRunLoopBeforeWaiting）
 			7. 休眠，如果有（Source0、Source1或timer事件，则被唤醒）
 			8. 通知Observer，线程被唤醒（kCFRunLoopAfterWaiting）
 			9. 处理唤醒时收到的事件（Source0、Source1或timer事件），完成后跳转到步骤2
 			10. 通知Observer，即将推出循环（kCFRunLoopExit）
 		
 		- RunLoop 的消息类型（事件源）
 			- Port：基于 Mach_Ports 的事件。监听程序的 Mach ports。内核通过port这种方式将信息发送，而mach则监听内核发来的port信息，然后将其整理，打包发给runloop。
 			- Customer：自定义事件源。必须由另一个线程来发送。很少用到。
 			- Selector Sources：NSObject类提供的一系列 performSelector 方法。
 				- 如果调用线程和指定线程为同一线程，且`wait`参数设为YES，那么 selector 会直接在指定线程运行，不再添加到runloop。
 			- Timer Sources：定时器源。定时器默认以 DefaultMode 添加到主线程的 runloop，也可以指定添加到其他线程。
 	- 退出
 		- 可以用以下方式退出runloop：
 			- 通过 `runUntilDate:` 启动的 runloop 最大时间到期
 				- 推荐使用这种方式
 			- 事件源为空，runloop所在线程中既没有source也没有timer
 				- 并不推荐这样退出，因为一些系统的事件源（Source1）我们并不知道
 			- 调用CFRunLoopStop，退出runloop并将程序控制权交给调用者
				- 无法退出通过`run`和`runUntilDate:`启动的 runloop，因为它们会不断调用 `runMode:beforeDate:` 方法维持循环
			
## AutoreleasePool 何时释放？

- 自动释放池是 Objective-C 开发中的一种自动内存回收管理的机制。当对象调用 autorelease 方法后会被放到自动释放池中延迟释放时机，当缓存池需要清除dealloc时，会向这些 Autoreleased对象做 release 释放操作。
- 自动释放池何时释放：
	- ARC自动释放池：`@autoreleasepool {}`，在大括号结束时释放。
		- 从 main.m 文件中可以看到，**整个 iOS 的应用都是包含在一个自动释放池 block 中的**。
		- 所以临时使用的大量数据应该放到自己创建的 `@autoreleasepool` 中，以保证及时释放。
	- MRC自动释放池：通过 `[NSAutoreleasePool new]` 创建，调用 `[pool drain]` 方法时释放。
	- Runloop自动释放池：
		- 即将进入Loop时，调用`_objc_autoreleasePoolPush()`创建自动释放池。
		- 即将进入休眠时，调用`_objc_autoreleasePoolPop()`和`_objc_autoreleasePoolPush()`释放旧的池并创建新池。
		- 即将退出Loop时，调用`_objc_autoreleasePoolPop()`释放自动释放池。