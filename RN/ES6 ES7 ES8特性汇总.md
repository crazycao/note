# ES6 ES7 ES8

|版本|发布时间|新增特性|
|:--|:--|:--|
|ES5|2019.11|扩展了Object、Array、Function的功能等|
|ES6|2015.6|类，模块化，箭头函数，函数参数默认值|
|ES7|2016.3|
|ES8|2017.6|


## ES6 新特性
### 1 类
ES6引入类的概念，让JS的面向对象编程更加简单和易于理解。

#### 类的定义和实例化
```
class 类名 {
	constructor() {
	}
	function() {
	}
}
var 实例 = new 类名()
```
####类可以继承
```
class 子类 extends 父类 {
	constructor() {
		super(); // 子类若有构造函数，一定要显示调用super函数。
	}
}
```

### 2 模块化

#### 导出（export）
一个文件只能有一个导出。

```
// 导出变量、常量
var name = '竹本';
export name;
```
```
// 导出对象
var man = {name, age}; 
export man;
```
```
// 导出函数
export function myFunction() {
	return a;
}
```
#### 导入（import）
从文件导入可以得到该文件导出的内容。

```
import man from 'man.js'; 
var name = man.name;

import class from 'myClass.js';
class.myFunction();
```

### 3 箭头函数
箭头函数与包围它的代码共享同一个 `this`。所以不需要用 `var that = this;`引用外围this。

箭头函数格式如下：

```
(p)=>{return p;}
```
`()`里可以传入参数；`{}`里是看书实现。

### 4 函数默认值
```
function foo(num = 0) {
	// ...
}
```

### 5 模板字符串

ES5 的字符串拼接：
```
var hello = 'Hello, ' + name + '! Welcome to' + place + '.';
```

ES6可使用模板字符串：
```
var hello = 'Hello, ${name}! Welcome to ${place}.';
```

### 6 解构赋值
```
var man = {name:'竹本', age:8};
var {name, age = 18} = man; // 没取到 age 时，默认为 18
```

### 7 延展操作符（...）
```
var arr = [1, 2, 3];
var arr2 = [...arr]; // 等价于 arr.slice();
```

在ECMAScript 2018中延展操作符增加了对对象的支持。

注意，延展操作符只能实现浅拷贝。

```
var p = {
	a: '123',
	b: '456',
	c: '789'
}
var {b, ...other} = p; // other 的值为 {a: '123', c: '789'};
```

### 8 对象属性简写
```
var name = '竹本';
var age = 18;
var man = {name, age}; // 生成的 man 对象，key与变量名相同
```

### 9 异步方案 Promise
```
// 准备
var waitSecond = new Promise(
	function(resolve, reject) {
		setTimeout(resolve, 100);
	}
)
// 调用
waitSecond.then(
	// 调用成功的处理方法
	function() {
	}
)
```

### 10 支持 let 与 const

var 声明的变量为函数级作用域。
let 和 const 声明的变量为块级作用域。

## ES7的特性
### 1.
