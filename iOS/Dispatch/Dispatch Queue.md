# Dispatch Queue 调度队列

原文链接：[https://developer.apple.com/documentation/dispatch/dispatch_queue?language=objc](https://developer.apple.com/documentation/dispatch/dispatch_queue?language=objc)

An object that manages the execution of tasks serially or concurrently on your app's main thread or on a background thread.
	
一个管理任务执行的对象，它可以控制任务在你的 APP 的主线程或后台线程上串行或并行的执行。

## Overview 概览

Dispatch queues are FIFO queues to which your application can submit tasks in the form of block objects. Dispatch queues execute tasks either serially or concurrently. Work submitted to dispatch queues executes on a pool of threads managed by the system. Except for the dispatch queue representing your app's main thread, the system makes no guarantees about which thread it uses to execute a task.

调度队列是先进先出队列，你的程序可以以 block 对象的形式提交任务到队列中。调度队列串行或并行的执行任务。提交到调度队列的工作在一个由系统管理的线程池中执行。除了表示应用主线程的调度队列以外，系统不保证使用哪个线程来执行一个任务。

You schedule work items synchronously or asynchronously. When you schedule a work item synchronously, your code waits until that item finishes execution. When you schedule a work item asynchronously, your code continues executing while the work item runs elsewhere.

你可以同步或异步的安排工作项目。当你同步的安排一个工作项目时，你的代码会等待，直到项目执行完成。当你异步的安排一个项目时，你的代码会继续执行，而这个工作项目在其他地方运行。

> **Important**
>
> Attempting to synchronously execute a work item on the main queue results in deadlock.
> 
> **重要**
> 
> 尝试在主队列上同步执行工作项会导致死锁。

Dispatch queues provide minimal support for autoreleased objects by default. System APIs may return autoreleased objects to your code. For example, NSError objects are often autoreleased. If you see memory pressure increase because of autoreleased objects created in your blocks, consider adding autorelease pools to those blocks to relieve the pressure. You can also configure the default autorelease behavior of any custom dispatch queues using the [dispatch\_queue\_attr\_make\_with\_autorelease\_frequency](https://developer.apple.com/documentation/dispatch/1642197-dispatch_queue_attr_make_with_au?language=objc) function at creation time.

调度队列默认提供对自动释放对象的最小支持。系统 API 可能会返回自动释放对象给你的代码。例如，NSError 对象通常是自动释放的。如果你发现由于在你的 block 中创建的自动释放对象导致内存压力增加，可以考虑在那些 block 中添加自动释放池以减轻压力。你也可以在创建时使用 [dispatch\_queue\_attr\_make\_with\_autorelease\_frequency](https://developer.apple.com/documentation/dispatch/1642197-dispatch_queue_attr_make_with_au?language=objc) 方法配置任意自定义调度对象的默认自动释放行为。

### Avoiding Excessive Thread Creation 避免过多的线程创建

When designing tasks for concurrent execution, do not call methods that block the current thread of execution. When a task scheduled by a concurrent dispatch queue blocks a thread, the system creates additional threads to run other queued concurrent tasks. If too many tasks block, the system may run out of threads for your app.

当设计并发执行的任务时，不要调用会阻塞当前线程执行的方法。当安排到并发调度队列中的任务阻塞一个线程时，系统会创建额外的线程运行其他在队列中的并发任务。如果太多的任务阻塞了，系统可能会用完你的 app 的线程。

Another way that apps consume too many threads is by creating too many private concurrent dispatch queues. Because each dispatch queue consumes thread resources, creating additional concurrent dispatch queues exacerbates the thread consumption problem. Instead of creating private concurrent queues, submit tasks to one of the global concurrent dispatch queues. For serial tasks, set the target of your serial queue to one of the global concurrent queues. That way, you can maintain the serialized behavior of the queue while minimizing the number of separate queues creating threads.

另一个消耗过多线程的方法是，创建过多的私有并发调度队列。因为每一个调度队列都会消耗线程资源，创建额外的并发调度队列会加剧线程消耗问题。与其创建私有并发队列，不如将任务提交到一个全局并发调度队列。对于串行任务，可把串行队列的目标设置到一个全局并发队列中。这样，您就可以维护队列的序列化行为，同时最小化创建线程的独立队列的数量。

## Topics 主题

### Creating a Dispatch Queue 创建一个调度队列

- [dispatch\_get\_main\_queue](https://developer.apple.com/documentation/dispatch/1452921-dispatch_get_main_queue?language=objc)

	Returns the serial dispatch queue associated with the application’s main thread.
	
	返回与 app 的主线程相关联的串行调度队列。
	
- [dispatch\_get\_global\_queue](https://developer.apple.com/documentation/dispatch/1452927-dispatch_get_global_queue?language=objc)

	Returns a system-defined global concurrent queue with the specified quality-of-service class.
	
	返回一个系统定义的具有指定服务质量类的全局并发队列。
	
- [dispatch\_queue\_create](https://developer.apple.com/documentation/dispatch/1453030-dispatch_queue_create?language=objc)

	Creates a new dispatch queue to which you can submit blocks.
	
	创建一个新的调度队列，你可以向其提交 block。
	
- [dispatch\_queue\_create\_with\_target](https://developer.apple.com/documentation/dispatch/1642205-dispatch_queue_create_with_targe?language=objc)

	Creates a new dispatch queue to which you can submit blocks.
	
	创建一个新的调度队列，你可以向其提交 block。

- [DISPATCH\_QUEUE\_SERIAL](https://developer.apple.com/documentation/dispatch/dispatch_queue_serial?language=objc)

	A dispatch queue that executes blocks serially in FIFO order.
	
	一个以先进先出的顺序串行的执行 block 的调度队列。
	
- [DISPATCH\_QUEUE\_CONCURRENT](https://developer.apple.com/documentation/dispatch/dispatch_queue_concurrent?language=objc)

	A dispatch queue that executes blocks concurrently.
	
	并发执行 block 的调度队列。
	
- [dispatch\_queue\_t](https://developer.apple.com/documentation/dispatch/dispatch_queue_t?language=objc)

	A lightweight object to which your application submits blocks for subsequent execution.
	
	一个轻量级的对象，你的程序向其提交 block 以供后续执行。
	
- [dispatch\_queue\_main\_t](https://developer.apple.com/documentation/dispatch/dispatch_queue_main_t?language=objc)

	A dispatch queue that is bound to the app's main thread and executes tasks serially on that thread.
	
	一个绑定到 APP 的主线程并在该线程上串行执行任务的调度队列。
	
- [dispatch\_queue\_global\_t](https://developer.apple.com/documentation/dispatch/dispatch_queue_global_t?language=objc)

	A dispatch queue that executes tasks concurrently using threads from the global thread pool.
	
	一个使用来自全局线程池中的线程并发的执行任务的调度队列。
	
- [dispatch\_queue\_serial\_t](https://developer.apple.com/documentation/dispatch/dispatch_queue_serial_t?language=objc)

	A dispatch queue that executes tasks serially in first-in, first-out (FIFO) order.
	
	一个以先进先出的顺序串行的执行任务的调度队列。
	
- [dispatch\_queue\_concurrent\_t](https://developer.apple.com/documentation/dispatch/dispatch_queue_concurrent_t?language=objc)

	A dispatch queue that executes tasks concurrently and in any order, respecting any barriers that may be in place.
	
	一个以任意顺序并发的执行任务的调度队列，并考虑栅栏可能存在的情况。
	
### Configuring Queue Execution Parameters 配置队列执行参数

- [dispatch\_queue\_attr\_t]

	Attributes describing the behaviors of a dispatch queue.
- [dispatch\_queue\_attr\_make\_with\_qos\_class]

	Returns attributes suitable for creating a dispatch queue with the desired quality-of-service information.
- [dispatch\_queue\_get\_qos\_class]

	Returns the quality-of-service class for the specified queue.
- [dispatch\_qos\_class\_t]

	Quality-of-service classes that specify the priorities for executing tasks.
- [dispatch\_queue\_attr\_make\_initially\_inactive]

	Returns an attribute that configures a dispatch queue as initially inactive.
- [dispatch\_queue\_attr\_make\_with\_autorelease\_frequency]

	Returns an attribute that specifies how the dispatch queue manages autorelease pools for the blocks it executes.
- [dispatch\_autorelease\_frequency\_t]

	Constants indicating the frequency with which a dispatch queue creates autorelease pools for its tasks.

### Executing Tasks Asynchronously 异步执行任务

- [dispatch\_async]

	Submits a block for asynchronous execution on a dispatch queue and returns immediately.
- [dispatch\_async\_f]

	Submits an application-defined function for asynchronous execution on a dispatch queue and returns immediately.
- [dispatch\_after]

	Enqueues a block for execution at the specified time.
- [dispatch\_after\_f]

	Enqueues an application-defined function for execution at the specified time.
- [dispatch\_function\_t]

	The prototype of functions submitted to dispatch queues.
- [dispatch\_block\_t]

	The prototype of blocks submitted to dispatch queues, which take no arguments and have no return value.

### Executing Tasks Synchronously 同步执行任务

- [dispatch\_sync]

	Submits a block object for execution and returns after that block finishes executing.
- [dispatch\_sync\_f]

	Submits an application-defined function for synchronous execution on a dispatch queue.

### Executing a Task Only Once 只执行一次任务

- [dispatch\_once]

	Executes a block object only once for the lifetime of an application.
- [dispatch\_once\_f]

	Executes an application-defined function only once for the lifetime of an application.
- [dispatch\_once\_t]

	A predicate for use with the dispatch\_once function.

### Executing a Task in Parallel 并行执行任务

- [dispatch\_apply]

	Submits a single block to the dispatch queue and causes the block to be executed the specified number of times.
- [dispatch\_apply\_f]

	Submits a single function to the dispatch queue and causes the function to be executed the specified number of times.
- [DISPATCH\_APPLY\_AUTO]

### Managing Queue Attributes 管理队列属性

- [dispatch\_queue\_get\_label]

	Returns the label you assigned to the dispatch queue at creation time.
- [DISPATCH\_CURRENT\_QUEUE\_LABEL]

	Pass this constant to the dispatch\_queue\_get\_label function to retrieve the label of the current queue.
- [dispatch\_set\_target\_queue]

	Specifies the dispatch queue on which to perform work associated with the current object.

### Getting and Setting Contextual Data 获取和设置上下文数据
- [dispatch\_get\_specific]

	Returns the value for the key associated with the current dispatch queue.
- [dispatch\_queue\_set\_specific]

	Sets the key/value data for the specified dispatch queue.
- [dispatch\_queue\_get\_specific]

	Gets the value for the key associated with the specified dispatch queue.

### Managing the Main Dispatch Queue 管理主调度队列
- [dispatch\_main](https://developer.apple.com/documentation/dispatch/1452860-dispatch_main?language=objc)

	Executes blocks submitted to the main queue.
	
	执行提交到主队列的 block。

