# Block 的原理解析

参考文章：[https://www.jianshu.com/p/0312f18e846f](https://www.jianshu.com/p/0312f18e846f)

block本质上是一个OC对象，通常称为代码块或者闭包。

## 实验方法

block 在实际编译时无法转换成我们能够理解的源代码，但可以通过 clang（LLVM编译器）转换成可读的源代码，步骤如下：

1. 打开终端，输入 `cd` 把要转换的文件拖到终端，然后回车进入要转换文件的目录；

	要转换文件代码如下：

	```
int main(int argc, const char * argv[]) {
    @autoreleasepool {

        void (^myBlock)(void) = ^{
            printf("Block\n");
        };

        myBlock();
    }
    return NSApplicationMain(argc, argv);
}
```
2. 输入命令 `clang -rewrite-objc main.m -o main.cpp`
	
3. __weak问题解决

	在使用clang转换OC为C++代码时，可能会遇到以下问题

	```
cannot create __weak reference in file using manual reference
```

	解决方案：支持ARC、指定运行时系统版本，比如：

	```
xcurn -sdk iphoneos clang -arch arm64 -rewrite-objc -fobjc-arc -fobjc-runtime=ios-8.0.0 main.m
```

## 实验结果

源码通过clang，去掉一些类型转换我们可以得到以下代码：

```
struct _block_impl {
void *isa;
int Flags;
int Reserved;
void *FuncPtr;
};
//Block定义的结构体
struct __main_block_impl_0 {
struct _block_impl impl;
struct __main_block_desc_0 *Desc;
__main_block_impl_0(void *fp,struct __main_block_desc_0 *desc, int flags=0)
{
impl.isa = &_NSConcreteStackBlock;
impl.Flags = flags;
impl.FuncPtr = fp;
Desc = desc;
}
};
//我们的Block里面的函数
static void __main_block_func_0(struct __main_block_impl_0 *_cself)
{
printf("Block\n");
}
//存储block的其他信息，大小等
struct __main_block_desc_0 {
unsigned long reserved;
unsigned long Block_size;
} __mainBlock_desc_0_DATA = {
0,
sizeof(struct __main_block_impl_0)
};
/*以下是我们的代码部分*/
//赋值部分，
struct __main_block_impl_0 temp = __main_block_impl_0(__main_block_func_0,&__mainBlock_desc_0_DATA);
struct __main_block_impl_0 *myBlock = &temp;
//调用部分
(*myBlock->impl.FuncPtr)(myBlock);
/*以下是我们的代码部分*/
```

看起来好像很麻烦？居然两句代码变出了这么多代码。慢慢分析起来其实也不难理解：

首先，系统自动给我们生成了三个结构体。

```
//block的结构体定义
struct __main_block_impl_0 {
struct _block_impl impl;//Block isa ，函数地址等定义
struct __main_block_desc_0 *Desc;//Block size等信息定义
};
struct _block_impl {
void *isa;//所属的类
int Flags;
int Reserved;
void *FuncPtr;//函数地址
};
struct __main_block_desc_0 {
unsigned long reserved;
unsigned long Block_size;
} 
```

其次，生成了两个函数。

```
//Block信息初始化的函数
__main_block_impl_0(void *fp,struct __main_block_desc_0 *desc, int flags=0)
{
impl.isa = &_NSConcreteStackBlock; // 这个block的class类型为_NSConcreteStackBlock
impl.Flags = flags;
impl.FuncPtr = fp; // 保存函数地址
Desc = desc;
}
//我们的 block 里面的函数，_cself就是调用这个函数的调用者的指针
static void __main_block_func_0(struct __main_block_impl_0 *_cself)
{
printf("Block\n");
}
```

然后，将我们定义 block 的代码转化成

```
/*
初始化
__main_block_func_0:函数地址,
__mainBlock_desc_0_DATA:block的size信息
*/
struct __main_block_impl_0 temp = __main_block_impl_0(__main_block_func_0,&__mainBlock_desc_0_DATA);
struct __main_block_impl_0 *myBlock = &temp;
```

**我们 block 里面的函数被转化成了 `__main_block_func_0`，函数地址作为第一个参数传入了 `__main_block_impl_0` 函数，保存在了 `_block_impl` 结构体的 `FuncPtr` 中。`__main_block_impl_0` 函数初始化得到了 `__main_block_impl_0` 类型的结构体 `temp`，`myBlock` 是指向 `temp` 的指针。** 所以说，iOS 里的 block 是被当成一个类来看待的，有自己的存储空间。

最后调用时，拿到上面定义的 block 变量 myBlock，找到函数地址，调用函数，并把调用者也就是 myBlock 传递进去。

```
//调用部分
(*myBlock->impl.FuncPtr)(myBlock);
```




