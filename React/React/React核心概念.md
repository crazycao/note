# React核心概念


- [React](#React)
- [JSX](#JSX)
- [元素](#元素)
- [组件](#组件)
  - [函数组件](#函数组件)
  - [class组件](#class组件)
  - [渲染组件](#渲染组件)
  - [将函数组件转换成 class 组件](#transfer)
- [Props](#props)
- [State](#State)
  - [正确地使用State](#usage)
  - [props vs state](#vs)
- [生命周期](#生命周期)
  - [挂载](#挂载)
  - [更新](#更新)
  - [卸载](#卸载)
- [事件处理](#事件处理)
  - [向事件处理程序传递参数](#向事件处理程序传递参数)
- [条件渲染](#条件渲染)
  - [if ... else ...](#if...else)
  - [与运算符 &&](#与运算符)
  - [三目运算符 ? :](#三目运算符)
  - [阻止组件渲染](#阻止组件渲染)
- [列表](#列表)
  - [key](#key)
- [表单](#表单)
  - [受控组件](#受控组件)
  - [处理多个输入](#处理多个输入)
- [状态提升](#Lifting_State_Up)
- [组合 vs 继承](#Composition_vs_Inheritance)

<span id='React'>

## React
React 是一个用于构建用户界面的 JavaScript 库。

<span id='JSX'>

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

<span id='元素'>

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

<span id='组件'>

## 组件

组件允许你将 UI 拆分为独立可复用的代码片段，并对每个片段进行独立构思。

<span id='函数组件'>

### 函数组件

定义组件最简单的方式就是编写 JavaScript 函数：

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

> `一个有效的React组件 = 接收唯一带有数据的“props”(代表属性)对象 + 返回一个React元素`

这类组件被称为“**函数组件**”，因为它本质上就是 JavaScript 函数。

<span id='class组件'>

### class组件

也可以使用 ES6 的 class 来定义组件，这种称为“**class 组件**”：

```
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

上述两个组件在 React 里是等效的。

<span id='渲染组件'>

### 渲染组件

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

- **注意： 组件名称必须以大写字母开头。**
 React 会将以小写字母开头的组件视为原生 DOM 标签。例如，`<div />`代表 HTML 的 div 标签，而 `<Welcome />` 则代表一个组件，并且需在作用域内使用 Welcome。

<span id='transfer'>

### 将函数组件转换成 class 组件

五个步骤：

1. 创建一个同名的 ES6 class，并且继承于 `React.Component`。
1. 添加一个空的 `render()` 方法。
1. 将函数体移动到 `render()` 方法之中。
1. 在 `render()` 方法中使用 `this.props` 替换 `props`。
1. 删除剩余的空函数声明。

```
// 函数组件
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}
```

```
// 类组件
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

<span id='props'>

## props

当 React 元素为用户自定义组件时，它会将 JSX 所接收的属性（attributes）以及子组件（children）转换为单个对象传递给组件，这个对象被称之为 “**props**”。

```
function Welcome(props) {
  return <h1>Hello, {props.name}. Welcome to {props.school}!</h1>;
}

const element = <Welcome name="Sara" school="Harvard University"/>;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

组件无论是使用函数声明还是通过 class 声明，都决不能修改自身的 props。

React 严格规定：
**所有 React 组件都必须像纯函数一样保护它们的 props 不被更改。**

> 纯函数：函数不会尝试更改入参，且多次调用下相同的入参始终返回相同的结果。如
>```
>function sum(a, b) {
>    return a + b;
>}
>```

<span id='State'>

## State

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
> class 组件的构造函数中应该始终包含 `super(props);` 以调用父类的构造函数。
> 
> 但是，如果不初始化 state 或不进行方法绑定，则不需要为 React 组件实现构造函数。

<span id='usage'>

### 正确地使用 State

- **不要直接修改 State**，而是应该使用 `setState()`
  
  ```
  this.setState({comment: 'GoodNight'});
  ```

构造函数是唯一可以给 `this.state` 赋值的地方

  ```
  constructor(props) {
      super(props);
      this.state = {comment: 'Hello'};
  }
  ```
  
- **State 的更新可能是异步的**，所以你不要依赖 `this.props` 和 `this.state` 的值来更新下一个状态。

  解决办法是，当需要用到state里的一个属性给另一个属性赋值时，让 `setState()` 接收一个函数。
  
  ```
  this.setState((state, props) => ({
      counter: state.counter + props.increment
  }));
  ```
- **State 的更新会被合并**。当调用 `setState()` 的时候，React 会把你提供的对象合并到当前的 `state`。

  ```
  constructor(props) {
      super(props);
      this.state = {a: 0, b: 0};
  }
  componentDidMount() {
      this.setState({
        a: 1
      });
      this.setState({
        b: 2
      });
  } // 执行以后的 state 为 { a: 1, b: 2}
  ```
- **数据是向下流动的**

不管是父组件或是子组件都无法知道某个组件是有状态的还是无状态的，并且它们也并不关心它是函数组件还是 class 组件。

组件可以选择把它的 state 作为 props 向下传递到它的子组件中。

子组件会在其 props 中接收参数，但是组件本身无法知道它是来自于父组件的 state 或是 props，还是手动输入的。

这通常会被叫做 **“自上而下”或是“单向”的数据流**。

任何的 state 总是所属于特定的组件，而且从该 state 派生的任何数据或 UI 只能影响树中“低于”它们的组件。

<span id='vs'>

### props vs state

`props`（“properties” 的缩写）和 `state` 都是普通的 JavaScript 对象。它们都是用来保存信息的，这些信息可以控制组件的渲染输出，而它们的一个重要的不同点就是：

`props` 是传递给组件的（类似于*函数的形参*），而 `state` 是在组件内被组件自己管理的（类似于在一个*函数内声明的变量*）。

<span id='生命周期'>

## 生命周期

当 class 组件第一次被渲染到 DOM 中的时候，在 React 中被称为“**挂载（mount）**”。

同时，当 DOM 中 class 组件被删除的时候，在 React 中被称为“**卸载（unmount）**”。

当组件挂载或卸载时就会去执行的这些方法，就叫做“**生命周期方法**”。

<span id='挂载'>

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
  
  可以在这里直接调用 `setState()`。它将触发额外渲染，但此渲染会发生在浏览器更新屏幕之前。请谨慎使用该模式，因为它会导致性能问题。

<span id='更新'>

### 更新

当组件的 props 或 state 发生变化时会触发更新。组件更新的生命周期方法调用顺序如下：

- **render()**
- **componentDidUpdate(prevProps, prevState, snapshot)**
  
  在更新后会被立即调用。首次渲染不会执行此方法。

  你可以在此方法中对更新前后的 props 和 state 进行比较。也可以选择在此处进行网络请求。
  
  可以在此方法中直接调用 `setState()`，但请注意它必须被包裹在一个条件语句里，否则会导致死循环。它还会导致额外的重新渲染，虽然用户不可见，但会影响组件性能。

<span id='卸载'>

### 卸载

当组件从 DOM 中移除时会调用如下方法：

- **componentWillUnmount()**

  在组件卸载及销毁之前直接调用。
  
  在此方法中执行必要的清理操作，例如，清除 timer，取消网络请求或清除在 `componentDidMount()` 中创建的订阅等。

  不应在此调用 `setState()`，因为该组件将永远不会重新渲染。组件实例卸载后，将永远不会再挂载它。


<span id='事件处理'>

## 事件处理

React 元素的事件处理和 DOM 元素的很相似，但是有一点语法上的不同：

- React 事件的命名采用**小驼峰式（camelCase）**，而不是纯小写。
- 使用 JSX 语法时你**需要传入一个函数作为事件处理函数**，而不是一个字符串。
- 不能通过返回 false 的方式**阻止默认行为**。你必须显式的**使用 `preventDefault`**。

```
function ActionLink() {
  function handleClick(e) {
    e.preventDefault(); // 阻止 a 标签的跳转行为
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```

- 为了在回调中使用 `this`，**需要在构造函数中绑定 `this.handleClick`**

```
class LoggingButton extends React.Component {
  constructor(props) {
    super(props);
    // 为了在回调中使用 `this`，这个绑定是必不可少的
    this.handleClick = this.handleClick.bind(this);
  }
  
  handleClick = () => {
    console.log('this is:', this); // 若未绑定，此时 this 的值是 undefined
  }

  render() {
    return (
      // 传给 onClick 的是 this.XXX
      <button onClick={this.handleClick}> 
        Click me
      </button>
    );
  }
}
```

- 或者 可以在回调中使用**箭头函数**，此语法**确保 `handleClick` 内的 `this` 已被绑定**。但是此语法问题在于每次渲染时都会创建不同的回调函数。

```
  render() {
    return (
      // 使用箭头函数就不用再在构造函数中绑定 this 了
      <button onClick={() => this.handleClick()}>
        Click me
      </button>
    );
  }
```

<span id='向事件处理程序传递参数'>

### 向事件处理程序传递参数

```
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

上述两种方式是等价的，分别通过**箭头函数**和 **Function.prototype.bind** 来实现。

在这两种情况下，React 的事件对象 e 会被作为第二个参数传递。如果通过箭头函数的方式，事件对象必须显式的进行传递，而通过 bind 的方式，事件对象以及更多的参数将会被隐式的进行传递。

<span id='条件渲染'>

## 条件渲染

在 React 中，你可以创建不同的组件来封装各种你需要的行为。然后，依据应用的不同状态，你可以只渲染对应状态下的部分内容。

<span id='if...else'>

### if ... else ...

```
  render() {
    const isLoggedIn = this.state.isLoggedIn;
    let button;
    if (isLoggedIn) {
      button = <LogoutButton onClick={this.handleLogoutClick} />;
    } else {
      button = <LoginButton onClick={this.handleLoginClick} />;
    }

    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn} />
        {button}
      </div>
    );
  }
```

<span id='与运算符'>

### 与运算符 &&

通过花括号包裹代码，你可以在 JSX 中嵌入任何表达式。相当于 `if` 语句了。

```
render() {
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}
``` 

<span id='三目运算符'>

### 三目运算符 ?:

```
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      {isLoggedIn
        ? <LogoutButton onClick={this.handleLogoutClick} />
        : <LoginButton onClick={this.handleLoginClick} />
      }
    </div>
  );
}
```

<span id='阻止组件渲染'>

### 阻止组件渲染

在极少数情况下，你可能希望能隐藏组件，即使它已经被其他组件渲染。

在组件的 `render()` 方法中，直接返回 null，可以使该组件不进行任何渲染。（但是组件的生命周期方法仍然会被调用。）

<span id='列表'>

## 列表

使用 `map()` 函数对数组中的每一项使用 `{}` 在 JSX 内构建一个元素集合。

```
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li>{number}</li>
);
```

然后把整个 `listItems` 插入到 `<ul>` 元素中，渲染进 `DOM`：

```
ReactDOM.render(
  <ul>{listItems}</ul>,
  document.getElementById('root')
);
```

<span id='key'>

### key

`key` 帮助 React 识别哪些元素改变了，比如被添加或删除。因此你应当给数组中的每一个元素赋予一个确定的标识。

一个元素的 `key` 最好是这个元素在列表中拥有的一个独一无二的字符串。通常，我们使用数据中的 `id` 来作为元素的 `key`：

```
const todoItems = todos.map((todo) =>
  <li key={todo.id}>
    {todo.text}
  </li>
);
```

如果你选择不指定显式的 `key` 值，那么 React 将默认使用索引用作为列表项目的 `key` 值。

如果列表项目的顺序可能会变化，我们不建议使用索引来用作 key 值，因为这样做会导致性能变差，还可能引起组件状态的问题。

> 注意：元素的 `key` 只有放在就近的数组上下文中才有意义。一般在 `map()` 方法中的元素上设置 `key` 属性。

#### 用 key 提取组件

如果你提取出一个 `ListItem` 组件，你应该把 key 保留在数组中的这个 `<ListItem />` 元素上，而不是放在 `ListItem` 组件中的 `<li>` 元素上。

```
function ListItem(props) {
  // 正确！这里不需要指定 key：
  return <li>{props.value}</li>;
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // 正确！key 应该在数组的上下文中被指定
    <ListItem key={number.toString()} value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

经验法则：**在 `map()` 方法中的元素需要设置 key 属性。**

#### key 只是在兄弟节点之间必须唯一

数组元素中使用的 key 在其兄弟节点之间应该是独一无二的。然而，它们不需要是全局唯一的。当我们生成两个不同的数组时，我们可以使用相同的 key 值。

<span id='表单'>

## 表单

在 React 里，HTML 表单元素的工作方式和其他的 DOM 元素有些不同：

- 在 HTML 中，表单元素（如`<input>`、 `<textarea>` 和 `<select>`）之类的表单元素通常自己维护 state，并根据用户输入进行更新。

- 而**在 React 中，可变状态（mutable state）通常保存在组件的 state 属性中，并且只能通过使用 `setState()` 来更新**。

<span id='受控组件'>

### 受控组件

渲染表单的 React 组件还控制着用户输入过程中表单发生的操作。被 React 以这种方式控制取值的表单输入元素就叫做“**受控组件**”。

```
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    // handlechange 在每次按键时都会执行并更新 React 的 state，因此显示的值将随着用户输入而更新
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('提交的名字: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          名字:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="提交" />
      </form>
    );
  }
}
```

对于受控组件来说，输入的值始终由 React 的 state 驱动。_（最后成功输入的值并不是键盘敲入的值，而是 `setState()` 方法设置的 `value` 的值。）_

> 例外：
在 HTML 中，`<input type="file">` 允许用户从存储设备中选择一个或多个文件，将其上传到服务器，或通过使用 JavaScript 的 File API 进行控制。
因为它的 `value` 只读，所以它是 React 中的一个**非受控**组件。

- 在受控组件上指定 `value` 的值为常值，则会阻止用户更改输入。
  如：`<input value="hi" />`。
- 如果你指定了 `value`，但输入仍可编辑，则可能是你意外地将 `value` 设置为 `undefined` 或 `null`。
  如：`<input value={null} />`。

<span id='处理多个输入'>

### 处理多个输入

当需要处理多个 `input` 元素时，我们可以给每个元素添加 `name` 属性，并让处理函数根据 `event.target.name` 的值选择要执行的操作。

```
class Reservation extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isGoing: true,
      numberOfGuests: 2
    };

    this.handleInputChange = this.handleInputChange.bind(this);
  }

  handleInputChange(event) {
    const target = event.target;
    const value = target.name === 'isGoing' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }

  render() {
    return (
      <form>
        <label>
          参与:
          <input
            name="isGoing"
            type="checkbox"
            checked={this.state.isGoing}
            onChange={this.handleInputChange} />
        </label>
        <br />
        <label>
          来宾人数:
          <input
            name="numberOfGuests"
            type="number"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange} />
        </label>
      </form>
    );
  }
}
```

<span id='Lifting_State_Up'>

## 状态提升

通常，多个组件需要反映相同的变化数据，这时我们建议将共享状态提升到最近的共同父组件中去。

在 React 中，将多个组件中需要共享的 state 向上移动到它们的最近共同父组件中，便可实现共享 state。这就是所谓的“状态提升”。

实现方法是：

- 使用 `this.props.value` 替代 `this.state.value` 来读取数据
- 使用 `this.props.onChange()` 替代 `this.setState()` 来响应数据改变
- 在父组件中提供 `value` 和 `onChange()` 并传递给子组件，如下：

  ```
  <SubComponent value={value} onChange={()=>this.handleChange} />
  ```
  
应当依靠**自上而下的数据流**，而不是尝试在不同组件间同步 state。

<span id='Composition_vs_Inheritance'>

## 组合 vs 继承

React 有十分强大的组合模式。我们推荐使用**组合而非继承**来实现组件间的代码重用。

使用一个特殊的 children 属性来将子组件传递到当前组件的渲染结果中：

```
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
}

function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        Welcome
      </h1>
      <p className="Dialog-message">
        Thank you for visiting our spacecraft!
      </p>
    </FancyBorder>
  );
}
// <FancyBorder> 标签中的所有内容都会作为一个 children 属性传递给 FancyBorder 组件。
```

也可以自行约定属性名：将所需内容传入 props，并使用相应的 prop。

```
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}
      </div>
      <div className="SplitPane-right">
        {props.right}
      </div>
    </div>
  );
}

function App() {
  return (
    <SplitPane
      left={
        <Contacts />
      }
      right={
        <Chat />
      } />
  );
}
```

在 React 中没有“槽”（slot）这一概念的限制，你可以将任何东西作为 props 进行传递。


 
