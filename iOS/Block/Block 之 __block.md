# Block 之 __block

按照 block 的捕获机制。对于 block 外的变量引用，block 默认是将其复制到其数据结构中（自动变量截获）来实现访问的。自动变量截获发生于 block 初始化的瞬间，之后 block 内外的变量修改互不影响。

`__block` 可以用于解决block内部无法修改auto变量值的问题。

对于用 `__block` 修饰的外部变量引用，block 是复制其引用地址来实现访问的。

此时，block 读取的是外部变量最新的值，并且可以修改 `__block` 修饰的外部变量的值。

```
__block int age = 10;
void (^myblock)(void) =  ^{
  NSLog(@"%d",age);
  age = 30;
};
age  = 20;
myblock(); // 打印 20
myblock(); // 打印 30
```

> **注意：**__block不能修饰全局变量、静态变量（static)。

## 原理

对于 `__block` 修饰的变量，编译器会把它包装成一个对象，然后把这个变量放到这个对象的内部。

```
// 加上 __block 修饰符以后，int age 变成了一个对象
struct __Block_byref_age_0 {
  void *__isa;
__Block_byref_age_0 *__forwarding;
 int __flags;
 int __size;
 int age;
};
```

对象里有一个 `__forwarding` 指针，此指针指向自身对象。

修改变量的值时，实际上是修改这个对象内部的变量的值。

```
(age.__forwarding->age) = 20;
```

## __block 的内存管理

1. 当 `__block` 变量在栈上时，不会对指向的变量产生强引用。

2. 当 `__block` 变量复制到堆上时（在ARC环境下，编译器会根据情况自动将栈上的 block 复制到堆上，详见《Block 的存储域》），会根据这个变量的修饰符是`__strong`，`__weak`，`__unsafe_unretained`做出相应的操作形成强引用（retain）或者弱引用。（ARC会retain，MRC不会）。

	> 复制到堆上以后，`__forwarding` 指针指向堆上的对象（包装变量的对象）。

3. 当 block 从堆中移除时 会调用 block 内部的 dispose 函数，该函数会自动释放引用的 `__block` 变量（release)。

## 循环引用问题

某个类将block作为自己的属性变量（强引用），然后该类在block的方法体里面又使用了该类本身，block内部会将这个类(的实例)作为 auto 变量进行强引用（block内部使用强引用），这里会产生循环引用问题。

```
self.someBlock = ^(Type var) { 
    [self dosomething];
};
```

### 用`__weak`、`__unsafe_unretained`解决解决循环引用问题 - ARC

```
__weak typeof(self) weakSelf = self;
self.someBlock = ^(Type var) { 
    [weakSelf dosomething]; 
};
```
### 用`__block`、`__unsafe_unretained`解决解决循环引用问题 - ARC

```
__unsafe_unretained id weakSelf = self;
self.block = ^{
    NSLog(@"%p", weaSelf);
};
__block typeof(self) blockSelf = self;
self.someBlock = ^(Type var) {

    [blockSelf dosomething];
}
```
