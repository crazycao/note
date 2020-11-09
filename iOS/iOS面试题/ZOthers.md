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

- 自动生成私有属性.
- 自动生成 getter 和  setter 方法


## iOS NSString,NSArray,NSDictionary属性关键字copy

- 如果strong来修饰
	- 如果NSMutableString的值赋值给NSString,
	- 那么只是将NSString指向了NSMutableString的内存地址,并对NSMUtbaleString计数器加一
	- 此时,如果对NSMutableString进行修改,也会导致NSString的值修改
- 如果是copy修饰
	- 在用NSMutableString给他赋值时,会进行深拷贝,
	- 即把内容也给拷贝了一份,两者指向不同的位置,
	- 即使改变了NSMutableString的值,NSString的值也不会改变
- 以上规则不止适用于NSString，NSArray，NSDictionary等同理


## git rebase 与 git merge 有什么区别
- git merge 和 git rebase 都是用来合并两个分支的。
- git merge [branch]：将指定分支合并到当前分支中，如果没有冲突则自动进行新的提交。
- git rebase [branch]：在另一个分支基础之上重新应用修改，把另一个分支上的 commit 都放到当前分支的 commit 之后，就好像当前分支是从另一个分支的最新 commit 上拉出来的一样。（所以称为“变基”）
	- 冲突处理：手动处理完冲突以后，用 `git add` 命令去更新这些内容的索引，然后执行 `git rebase --continue` 继续应用(apply)余下的补丁。
	- 在任何时候，可以用 `git rebase --abort` 参数来终止rebase的操作，并且当前分支会回到rebase开始前的状态。

