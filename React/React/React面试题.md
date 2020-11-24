# React面试题 

## 函数组件与类组件的区别？如何选用？如何将函数组件转换成类组件？

- 函数组件：一个函数，接收“props”对象，并返回一个React元素的函数。

- 类组件：使用 ES6 的 class 来定义组件，扩展React.Component类，包含一个 render 方法，render中返回一个 React 元素。

- 二者的区别：

	1. 类组件可以使用更多特性，如 this、内部状态 state 和生命周期钩子。
	2. 函数组件的性能比类组件的性能要高，因为类组件使用的时候要实例化，而函数组件直接执行函数取返回结果即可。

- 选用：
	1. 为了提高性能，尽量使用函数组件。
	2. 在组件需要包含内部状态或者使用到生命周期函数的时候使用类组件。

- 将函数组件转换成类组件有五个步骤：

	1. 创建一个同名的 ES6 class，并且继承于 `React.Component`。
	1. 添加一个空的 `render()` 方法。
	1. 将函数体移动到 `render()` 方法之中。
	1. 在 `render()` 方法中使用 `this.props` 替换 `props`。
	1. 删除剩余的空函数声明。

## React 的生命周期函数有哪些？调用顺序如何？


- 挂载：（**当组件实例被创建并插入 DOM 中时**）

	1. **constructor()**  构造函数
	2. **render()**  渲染函数
	3. **componentDidMount()** 在组件挂载后（插入 DOM 树中）立即调用。

- 更新：（**当组件的 props 或 state 发生变化时**）

	1. **render()**
	2. **componentDidUpdate()** 在更新后会被立即调用。

- 卸载：（**当组件从 DOM 中移除时**）

	1. **componentWillUnmount()** 在组件卸载及销毁之前直接调用。

## 什么是高阶组件？

高阶组件（HOC）是参数为组件，返回值为新组件的函数。它是一种基于 React 的组合特性而形成的设计模式。

## React 中 refs 的作用是什么？如何创建 refs ？

- 作用：

	Refs 是 React 提供给我们的安全访问 DOM 元素或者某个组件实例的句柄。

	下面是几个适合使用 refs 的情况：

	1. 管理焦点，文本选择或媒体播放。
	2. 触发强制动画。
	3. 集成第三方 DOM 库。

- 在类组件中创建 refs
	
	1. Refs 是使用 React.createRef() 创建的，
	2. 并通过 ref 属性附加到 React 元素。
	3. 对该节点的引用可以在 ref 的 current 属性中被访问。
	
- 在函数组件中创建 refs

	1. 默认情况下，你不能在函数组件上使用 ref 属性，因为它们没有实例（this）。
	2. 通过使用 useRef()，你可以在函数组件内部使用 ref 属性。

## 简单介绍 Redux 如何使用？

## Hooks 是什么？useState/useEffects/useContext的使用？

- Hooks是 React 16.8 中的新添加内容。它们允许在不编写类的情况下使用 state 和其他 React 特性。

## Hooks 的使用要遵循哪些规则？为什么？

- 规则一：只在最顶层使用 Hook。 不要在循环，条件或嵌套函数中调用 Hook。

	1. React 允许在单个组件中使用多个 State Hook 或 Effect Hook。它依赖 Hook 调用的顺序，才能正确地将内部 state 和对应的 Hook 进行关联。
	2. 循环，条件或嵌套函数会改变 hooks 的调用顺序，从而导致bug。
	3. 如果我们想要有条件地执行一个 effect，可以将判断放到 Hook 的内部。

- 规则二：只在 React 函数中调用 Hook。不要在普通的 JavaScript 函数中调用 Hook。

	1. 建议在 React 的函数组件中调用 Hook；或者在自定义 Hook 中调用其他 Hook。
	2. 遵循此规则，确保组件的状态逻辑在代码中清晰可见。