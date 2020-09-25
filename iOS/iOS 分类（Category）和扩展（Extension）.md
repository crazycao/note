# iOS 分类（Category）和扩展（Extension）

## 概念

### 分类

分类（Category）是 Objective-C 2.0 之后添加的语言特性的特有语法，也叫类别。

它的主要作用是在不改变原有类的前提下，给这个类添加一些方法。（这是对装饰模式的一种具体实现。）

可以给任何类增加分类，即使没有源码也行，也可以给 Cocoa 系统的类添加分类。

在分类中声明的所有方法

>
A category can be declared for any class, even if you don’t have the original implementation source code (such as for standard Cocoa or Cocoa Touch classes). Any methods that you declare in a category will be available to all instances of the original class, as well as any subclasses of the original class. At runtime, there’s no difference between a method added by a category and one that is implemented by the original class.

参考链接：[https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/CustomizingExistingClasses/CustomizingExistingClasses.html#//apple_ref/doc/uid/TP40011210-CH6-SW2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/CustomizingExistingClasses/CustomizingExistingClasses.html#//apple_ref/doc/uid/TP40011210-CH6-SW2)

### 扩展

Extension是Category的一个特例。类扩展与分类相比只少了分类的名称，所以称之为“匿名分类”。

>
A class extension bears some similarity to a category, but it can only be added to a class for which you have the source code at compile time (the class is compiled at the same time as the class extension). The methods declared by a class extension are implemented in the @implementation block for the original class so you can’t, for example, declare a class extension on a framework class, such as a Cocoa or Cocoa Touch class like NSString.

参考链接：[https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/CustomizingExistingClasses/CustomizingExistingClasses.html#//apple_ref/doc/uid/TP40011210-CH6-SW3](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/CustomizingExistingClasses/CustomizingExistingClasses.html#//apple_ref/doc/uid/TP40011210-CH6-SW3)