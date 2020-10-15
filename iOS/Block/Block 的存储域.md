# Block 的存储域

参考文章：[https://zhuanlan.zhihu.com/p/151399818](https://zhuanlan.zhihu.com/p/151399818)

block 有3种类型，可以通过调用 class 方法或者 isa 指针查看具体类型，最终都是继承自 NSBlock 类型。

1、**NSGlobalBlock (_NSConcreteGlobalBlock)**

没有访问 auto 变量的 block 是 `__NSGlobalBlock__`；

也称为全局块，存储在全局内存中，相当于单例。

2、**NSStrackBlock (_NSConcreteStackBlock)**

访问 auto 变量的 block 是 `__NSStrackBlock__`；

也称为栈块，存在于栈内存中，超出其作用域则马上销毁。

3、**NSMallocBlock (_NSConcreteMallocBlock)**

`__NSStrackBlock__` 调用了copy 是 `__NSMallocBlock__`

也称为堆块，存在于堆内存中，是一个带引用计数的对象，需要自行管理其内存。

|block类型|名称|环境条件|isa指针类型|内存分配|copy效果|说明|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|NSGlobalBlock|全局块|没有访问 auto 变量|_NSConcreteGlobalBlock|程序的数据区域</br>.data区|什么也不做|block 存储在全局内存中，相当于单例|
|NSStrackBlock|栈块|访问了 auto 变量| _NSConcreteStackBlock |栈区|从栈复制到堆|block 存在于栈内存中，超出其作用域则马上销毁 |
| NSMallocBlock|堆块|NSStrackBlock 调用了 copy 就是 NSMallocBlock|_NSConcreteMallocBlock |堆区|引用计数增加|存在于堆内存中，是一个带引用计数的对象，需要自行管理其内存|

## ARC环境下的copy

在ARC环境下，编译器会根据情况自动将栈上的 block 复制到堆上，比如以下情况：

1. block作为函数返回值；
2. 将block赋值给__strong指针时；
3. block作为Cocoa API中方法名含有using Block的方法参数时；
4. block作为GCD API的方法参数时；

> 我们在声明一个block属性的时候，习惯用copy关键字，是为了把栈区的block拷贝到堆区，让我们来管理他的生命周期。
