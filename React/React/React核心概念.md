# React核心概念


## React
React 是一个用于构建用户界面的 JavaScript 库。

## JSX
JSX 是一个 JavaScript 的语法扩展。

```
const element = <h1>Hello, world!</h1>;
```
JSX 是一个看起来很像 XML 的 JavaScript 语法扩展。
我们不需要一定使用 JSX，但它有以下优点：

- JSX 执行更快，因为它在编译为 JavaScript 代码后进行了优化。
- 它是类型安全的，在编译过程中就能发现错误。
- 使用 JSX 编写模板更加简单快速。。

```
// 在 JSX 语法中，你可以在大括号内放置任何有效的 JavaScript 表达式。
const element = <h1>Hello, {user.firstName}</h1>;
```
```
// 为了便于阅读，我们会将 JSX 拆分为多行。同时，我们建议将内容包裹在括号中。
const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);
```
```
// 可以使用 /> 来闭合标签，就像 XML 语法一样
const element = <img src={user.avatarUrl} />;
```
Babel 会把 JSX 转译成一个名为 React.createElement() 函数调用，最后生成对象。这些对象被称为 “React 元素”。

```
// 上一条例子等于创建了一个这样的对象。（注意：这是简化过的结构）
const element = {
  type: 'img',
  props: {
    className: 'src',
    children: user.avatarUrl
  }
};
```

## 元素
元素是构成 React 应用的最小砖块。
想要将一个 React 元素渲染到根 DOM 节点中，只需把它们一起传入 ReactDOM.render()：

```
const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```

React 元素是不可变对象。一旦被创建，你就无法更改它的子元素或者属性。
更新 UI 唯一的方式是创建一个全新的元素，并将其传入 ReactDOM.render()。

```
const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
const anotherElement = <h1>Goodby, world</h1>;
ReactDOM.render(anotherElement, document.getElementById('root'));
```
React DOM 会将元素和它的子元素与它们之前的状态进行比较，并只会进行必要的更新（只更新它需要更新的部分）来使 DOM 达到预期的状态。

## 组件 和 props

组件，从概念上类似于 JavaScript 函数。它接受任意的入参（即 “**props**”），并返回用于描述页面展示内容的 React 元素。也被称为“**函数组件**”。

定义组件最简单的方式就是编写 JavaScript 函数：

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

也可以使用 ES6 的 class 来定义组件，这种称为“**class 组件**”：

```
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```
React 元素可以是 DOM 标签，也可以是用户自定义的组件。因此，可以把组件返回的元素渲染显示，也可以直接渲染组件。

```
const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```
 
```
ReactDOM.render(
  <Welcome name="Sara" />,
  document.getElementById('root')
);
```
如果你想写的组件只包含一个 render 方法，并且不包含 state，那么使用函数组件就会更简单。

**State** 与 props 类似，但是 state 是私有的，并且完全受控于当前组件。一般在 class组件的构造函数中对 state 进行初始化。

```
class Welcome extends React.Component {
  constructor(props) {
    super(props);
    this.state = {name: 'Sara'};
  }
  render() {
    return <h1>Hello, {this.state.name}</h1>;
  }
}
```

> 注意：
> 
> 构造函数中应该始终包含 `super(props);` 以调用父类的构造函数。
> 
> 但是，如果不初始化 state 或不进行方法绑定，则不需要为 React 组件实现构造函数。

## 生命周期

当 class 组件第一次被渲染到 DOM 中的时候，就为其设置一个计时器。这在 React 中被称为“**挂载（mount）**”。

同时，当 DOM 中 class 组件被删除的时候，应该清除计时器。这在 React 中被称为“**卸载（unmount）**”。

当组件挂载或卸载时就会去执行的这些方法，就叫做“**生命周期方法**”。

### 挂载

当组件实例被创建并插入 DOM 中时，其生命周期方法调用顺序如下：

- **constructor()**  构造函数

- **render()**  渲染函数

  `render()` 函数应该为纯函数，这意味着在不修改组件 state 的情况下，每次调用时都返回相同的结果，并且它不会直接与浏览器交互。
  
  它是 class 组件中唯一必须实现的方法。
  
- **componentDidMount()**

  在组件挂载后（插入 DOM 树中）立即调用。
  
  依赖于 DOM 节点的初始化应该放在这里。
  
  如需通过网络请求获取数据，此处是实例化请求的好地方。
  
  这个方法是比较适合添加订阅的地方。如果添加了订阅，请不要忘记在 `componentWillUnmount()` 里取消订阅。
  
  可以在这里直接调用 setState()。它将触发额外渲染，但此渲染会发生在浏览器更新屏幕之前。请谨慎使用该模式，因为它会导致性能问题。

### 更新

当组件的 props 或 state 发生变化时会触发更新。组件更新的生命周期方法调用顺序如下：

- **render()**
- **componentDidUpdate(prevProps, prevState, snapshot)**
  
  在更新后会被立即调用。首次渲染不会执行此方法。

  你可以在此方法中对更新前后的 props 和 state 进行比较。也可以选择在此处进行网络请求。
  
  中直接调用 setState()，但请注意它必须被包裹在一个条件语句里，否则会导致死循环。它还会导致额外的重新渲染，虽然用户不可见，但会影响组件性能。
