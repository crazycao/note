# Swift核心基础知识

参考：

- 官方文档：[https://docs.swift.org/swift-book/documentation/the-swift-programming-language/guidedtour](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/guidedtour)
- SwiftGG翻译：[https://swiftgg.gitbook.io/swift/huan-ying-shi-yong-swift/03_a_swift_tour](https://swiftgg.gitbook.io/swift/huan-ying-shi-yong-swift/03_a_swift_tour)


## 值

- 使用 `let` 来声明常量，使用 `var` 来声明变量。
- 通过一个值来声明变量和常量时，可以不用明确地声明类型，编译器会自动推断其类型。
	- 如果初始值没有提供足够的信息（或者没有初始值），则必须在变量后面声明类型。
- 一个值要转换成其他类型，必须显示转换，Swift 不支持隐式转换类型。
	- 特例：把值转换成字符串的简单方法——反斜杠＋括号，如 `"I have \(value) apples."`。
- 使用双引号（`"`）来包含字符串内容，使用三个双引号（`"""`）来包含多行字符串内容。 
- 使用方括号 [] 来创建数组和字典，并使用下标或者键（key）来访问元素。最后一个元素后面允许有个逗号。
	- 如果类型信息可以被推断出来，你可以用 [] 和 [:] 来创建空数组和空字典。
- 可选类型表示两种可能，有值或没有值，用类名加 `?` 表示。
	- 解包时如果有值，则返回该值；如果没有值则返回 `nil`。 
	- 使用可选值调用方法时，要在变量名后面加 `?`，如果 `?` 之前的值是 `nil`，`?` 后面的东西都会被忽略，并且整个表达式返回 `nil`。
- 可以在可选型变量后面加一个感叹号（!）来获取值。这个感叹号表示“我知道这个可选型变量有值，请使用它。”（强制解包）

## 控制流

- 使用 `if` 和 `switch` 来进行条件操作，使用 `for-in`、`while` 和 `repeat-while` 来进行循环。
	- 包裹条件和循环变量的括号可以省略，但是语句体的大括号是必须的。 
- 在 `if` 语句中，条件必须是一个布尔表达式，不会隐式地与 0 做对比。（一个布尔值也是布尔表达式！）
- 使用 `if let` 来处理值缺失的情况，如可选值判断。（可选绑定）
	- 如果变量的可选值是 `nil`，条件会判断为 `false`，大括号中的代码会被跳过。如果不是 `nil`，会将值解包并赋给 `let` 后面的常量，可以在代码块中使用这个值。
	- 还可以使用较短的代码解包一个值，表示在代码块中使用相同的名称。 
	
	 ```
	 if let nickname {
	       print("Hey, \(nickname)") 
	 }
	 ```
- 通过使用 `??` 操作符可以为可选值提供一个默认值。如果值缺失，则返回默认值。
- `switch` 支持任意类型的数据以及各种比较操作——不仅仅是整数或枚举。
- `switch` 中匹配到的 case 语句执行完之后，程序会退出 `switch` 语句，所以不需要在每个子句结尾写 `break`。
- 可以使用 `for-in` 来遍历字典，需要一对变量来表示每个键值对，键和值以任意顺序迭代结束。
	
	 ```
	 for (key, value) in dict {
	       print("Key = \(key)  Value = \(value)") 
	 }
	 ```
- 可以在循环中使用 `..<` 来表示范围，如 `for i in 0..<4`；如果包含上边界的话需要使用 `...`，如 `for i in 0...3`。

## 函数和闭包

- 使用 `func` 来声明一个函数，使用名字和参数来调用函数。使用 `->` 来指定函数返回值的类型。
- 默认情况下，函数使用它们的参数名称作为它们参数的标签，在参数名称前可以自定义参数标签，或者使用 `_` 表示不使用参数标签。

	```
	func greet(_ person: String, on day: String) -> String {
		return "Hello \(person), today is \(day)."
	}
	```
- 可以使用元组来生成复合值，让一个函数返回多个值。该元组的元素可以用名称或数字来获取。如：

	```
	func calculateStatistics(scores: [Int]) -> (min: Int, max: Int, sum: Int)` {
		...
		return (min, max, sum)
	}
	```
 `。
- 函数可以嵌套（即在函数里定义函数）。被嵌套的函数可以访问外侧函数的变量。
- 函数是一种类型。函数可以作为另一个函数的返回值。也可以当做参数传入另一个函数。
- 闭包是一段能之后被调取的代码。
- 闭包中的代码能访问闭包作用域中的变量和函数，即使闭包在另一个不同作用域中被执行。

	```
	let closure = { (number: Int) -> Int in
		let result = 3 * number
		return result
	}
	let mappedNumbers = numbers.map(closure)
	```
- 如果一个闭包的类型已知，比如作为一个代理的回调，可以忽略参数，返回值，甚至两个都忽略。单个语句闭包会把它语句的值当做结果返回。

	```
	let mappedNumbers = numbers.map({ number in 3 * number })
	```
- 可以通过参数位置而不是参数名字来引用参数。
- 当一个闭包作为最后一个参数传给一个函数的时候，它可以直接跟在圆括号后面。

	```
	let mappedNumbers = numbers.map({ 3 * $0 })
	```
- 当一个闭包是传给函数的唯一参数，函数可以不写圆括号。
	```
	let mappedNumbers = numbers.map { 3 * $0 }
	```
	
## 对象和类

- 使用 `class` 和类名来创建一个类。创建类的时候并不需要一个标准的根类。
- 使用点语法来访问实例的属性和方法。
- 使用 `init` 创建一个构造函数。使用 `deinit` 创建一个析构函数。
- 子类如果要重写父类的方法的话，需要用 `override` 标记。
- 除了简单的存储属性，还有使用 getter 和 setter 的计算属性。在 setter 中，新值的名字是 `newValue`。
- 如果你不需要计算属性，但是仍然需要在设置一个新值之前或者之后运行代码，使用 `willSet` 和 `didSet`。

	```
	class EquilateralTriangle: NamedShape {
		var sideLength: Double = 0.0
		init(sideLength: Double, name: String) {
			self.sideLength = sideLength
			super.init(name: name)
			numberOfSides = 3
		}
		
		var perimeter: Double {
			get {
				return 3.0 * sideLength
			}
			set {
				sideLength = newValue / 3.0
			}
		}
		
		var square: Square {
			willSet {
				sideLength = newValue * newValue
			}
		}
	```

## 枚举和结构体

- 使用 `enum` 来创建一个枚举。
- 枚举可以包含方法。
- 默认情况下，从 0 开始每次加 1 的方式为原始值进行赋值，也可以通过显式赋值。支持字符串或者浮点数作为枚举的原始值。
- 使用 `rawValue` 属性来访问一个枚举成员的原始值。
- 使用 `init?(rawValue:)` 初始化构造器来从原始值创建一个枚举实例。
- 在任何已知变量类型的情况下都可以使用缩写引用枚举成员。
- 可以为枚举成员设定关联值，关联值是在创建实例时决定的。
	- 同一枚举成员不同实例的关联值可以不相同。
	- 可以把关联值想象成枚举成员实例的存储属性。
	
	```
	enum ServerResponse {
		case result(String, String)
		case failure(String)
		
		func printSunTime() -> String {
			switch self {
			case let .result(sunrise, sunset):
				print("Sunrise is at \(sunrise) and sunset is at \(sunset)")
			case let .failure(message):
				print("Failure...  \(message)")
			}
		}
	}
	```
- 使用 `struct` 来创建一个结构体。
	- 结构体和类有很多相同的地方，包括方法和构造器。 
	- 最大的区别是结构体是传值，类是传引用。

## 并发性

- 使用 `async` 标记异步运行的函数。

```
func fetchUserID(from server: String) async -> Int { ... }
```
- 在函数名前添加 `await` 来标记对异步函数的调用。程序会等待异步函数返回后才继续执行。

```
func fetchUsername(from server: String) async -> String {
	let userID = await fetchUserID(from: server)
	...
}
```

- 使用 `async let` 来调用异步函数，并让其与其它异步函数并行运行。 使用 `await` 以使用该异步函数返回的值。

```
func connectUser(to server: String) async {
	async let userID = fetchUserID(from: server)
	async let username = fetchUsername(from: server)
	let greeting = await "Hello \(username), user ID \(userID)"
	...
}
```

- 使用 `Task` 从同步代码中调用异步函数且不等待它们返回结果。

```
Task {
	await connectUser(to: "primary")
}
```

## 协议和扩展

- 使用 `protocol` 来声明一个协议。
- 类、枚举和结构体都可以遵循协议。
- 使用 `mutating` 关键字用来标记一个会修改结构体的属性的方法。
	- 类的方法中不需要关键字标记，类的方法可以直接修改类属性。
- 使用 `extension` 来为现有的类型添加功能。
- 可以直接使用协议名来标识变量类型。
	- 它的值必须是实现了该协议的对象，并且只能使用协议内定义的方法。

## 错误处理

- 使用遵守 `Error` 协议的类型来表示错误。
- 使用 `throw` 来抛出一个错误和使用 `throws` 来表示一个可以抛出错误的函数。

```
func send(job: Int, toPrinter printerName: String) throws -> String {
    if printerName == "Never Has Toner" {
        throw PrinterError.noToner
    }
    ...
}
```

- 可以使用 `do-catch` 来进行错误处理。
	- 在 `do` 代码块中，使用 `try` 来标记可以抛出错误的代码。
	- 在 `catch` 代码块中，除非你另外命名，否则错误会自动命名为 `error` 。
	- 可以使用多个 `catch` 块来处理特定的错误，类似 `switch`。

```
do {
    let printerResponse = try send(job: 1440, toPrinter: "Gutenberg")
    print(printerResponse)
} catch PrinterError.onFire {
    print("I'll just put this over here, with the rest of the fire.")
} catch let printerError as PrinterError {
    print("Printer error: \(printerError).")
} catch {
    print(error)
} 
```

- 也可以使用 `try?` 来进行错误处理。
	- 如果函数抛出错误，该错误会被抛弃并且结果为 `nil`。
	- 否则，结果会是一个包含函数返回值的可选值。

```
let printerSuccess = try? send(job: 1884, toPrinter: "Mergenthaler")
```

- 使用 `defer` 代码块来表示在函数返回前，函数中最后执行的代码。
	- 无论函数是否会抛出错误，这段代码都将执行。

## 泛型

- 在尖括号里写一个名字来创建一个泛型函数或者类型（类、枚举和结构体）。
- 在函数名或类型名后面使用 `where` 来指定对类型的一系列需求。

```

func anyCommonElements<T: Sequence, U: Sequence>(_ lhs: T, _ rhs: U) -> Bool
	where T.Element: Equatable, T.Element == U.Element
{
	...
}
```





