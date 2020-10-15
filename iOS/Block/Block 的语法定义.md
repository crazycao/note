## Block 语法定义

block本质上是一个OC对象，通常称为代码块或者闭包。

### block 的声明

```
	// 返回值 (^代码块名称)(参数列表) = ^(参数列表){表达式}
	<returnType>(^blockName)(arguments)=^(arguments){body; }
```

- 等号后面是真正的代码块。
- 返回值是指等号后面的代码块的返回值。可以为 void，但不能不写。
- 代码块名称将在执行等号后面的代码块时被使用。
- 等号左右的参数列表应该一一对应。
	- 等号左边的参数列表可以只写参数类型不写参数名称。
	- 等号右边的参数列表需要写明参数类型和参数名称，参数名称将在表达式中被使用。
	- 如果没有参数，参数列表可以只写一个 void，或者不写。
- 表达式是放在 {} 中的一系列代码语句，相当于函数体。

### block 的使用

block 的使用非常简单，与正常的 C 语言函数调用一样。

```
// 代码块名称(参数列表);
blockName(arguments);
```

### 用法示例

- 无参数，无返回值

```
// 无参数，无返回值，声明和定义
void (^myBlock)(void) = ^(void) {

    NSLog(@"Block:无参数，无返回值");
};

// block的调用
myBlock();
```

- 无参数，有返回值

```
// 无参数，有返回值，声明和定义
int (^myBlock)(void) = ^{

    NSLog(@"Block: 无参数，有返回值(9)");
    return 9;
};

myBlock();
```

- 有参数，无返回值

```
// 有参数，无返回值，声明和定义
void (^myBlock)(int a) = ^(int a) {

    NSLog(@"Block: 有参数（%ld)，无返回值", (long)a);
};

myBlock(5);
```

- 有参数，有返回值

```
// 有参数，有返回值
int (^myBlock)(int, int) = ^(int a, int b) {

    NSLog(@"Block: 有参数（a:%ld, b:%ld)", (long)a,(long)b);
    NSLog(@"Block: 有返回值（%ld)",(long)(a+b));

    return a+b;
};

myBlock(1, 2);
```

> **注意**
> 
> 实际开发中，经常用到 typedef 定义一个 Block。
> 
> 例如: 
> 
> `typedef int (^MyBlock)(int, int);` 
> 
> 这样我们定义了一个带有两个整型参数的 Block，并且返回值也是一个整型。 
> 
> 定义类属性：
> 	
> `@property (nonatomic, copy) MyBlock myBlock; `
> 
> 使用： 
> 
> `self.myBlock = ^int(int a, int b) { return a+b; };`
> 
> `self.myBlock(1, 2);`


