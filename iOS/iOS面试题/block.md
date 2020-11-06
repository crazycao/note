# block

## 请介绍 block 的实现原理

- block本质上是一个OC对象，通常称为代码块或者闭包。
- 声明 block 时，系统自动生成了三个结构体和两个函数：
	- 三个结构体 
		- `_block_impl`：包含了 Block isa ，函数地址（FunPtr）等定义
		- `__main_block_desc_0`：包含了 Block size等信息定义
		- `__main_block_impl_0`：（block结构体）包含了上面两个结构体
	- 两个函数
		- `__main_block_impl_0`： 一个是 block 信息初始化函数
		- `__main_block_func_0`：另一个是我们真正的 block 函数
- 初始化时
	- block 函数的函数地址传入了 block 信息初始化函数
	- block 信息初始化函数初始化了 block 结构体，并将 block 函数地址保存在了 block 结构体中
	- 我们开发中的 block 变量的实际是指向 block 结构体的指针
- 调用时
	- block 变量 -> block 结构体 -> FunPtr 指针 -> 函数调用
	- block 函数调用时会把 block 变量自身也传入进去。

## block 的存储区域是在栈上还是堆上？

- 没有访问自动变量的 block，存储在全局内存中，程序的数据区域。
- 访问了自动变量的 block，存储在栈内存中。
- 调用了 copy 以后，block 存在于堆内存中，是一个带引用计数的对象，需要自行管理其内存

> 我们在声明一个block属性的时候，习惯用copy关键字，是为了把栈区的block拷贝到堆区，让我们来管理他的生命周期。