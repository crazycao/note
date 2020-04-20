# React学习笔记

## 环境准备
操作系统：支持 macOS，Linux，Windows

运行环境：Node >= 8.10 和 npm >= 5.6

## 创建项目

直接在终端执行以下命令：

```
npx create-react-app my-app
```

* 注意：项目名称不能包含大写字母，react的习惯是用 - 连接。

执行完 `npx create-react-app my-app` 以后，终端最后显示如下则表示创建成功：

```
Success! Created my-app at /Users/caowenfeng/MyDemo/ReactDemo/my-app
Inside that directory, you can run several commands:

  yarn start
    Starts the development server.

  yarn build
    Bundles the app into static files for production.

  yarn test
    Starts the test runner.

  yarn eject
    Removes this tool and copies build dependencies, configuration files
    and scripts into the app directory. If you do this, you can’t go back!

We suggest that you begin by typing:

  cd a-game-demo
  yarn start

Happy hacking!
```

## 清除 Hello 代码

从零开始学习时，Hello 代码会非常 nice，但正式项目中就没必要再包含它们了。删除掉新项目中 `src` 文件夹下的所有文件。

> 注意：
不要删除整个 `src` 文件夹，只删除里面的源文件。React 需要 `src` 文件夹。


或者在终端执行下列命令清除。

```
cd my-app
cd src

# 如果你使用 Mac 或 Linux:
rm -f *

# 如果你使用 Windows:
del *

# 然后回到项目文件夹
cd ..
```

后续只要再按自己的需要添加代码即可。

## 语法高亮

VSCode 使用 **vscode-language-babel** 插件来支持 React 的语法高亮。

- 在 VSCode 编辑器中，选择 `查看` --> `扩展` （也可以直接点击左侧边栏的 `Extensions` 图标）。
- 在 `在应用商店中搜索扩展` 输入框中输入 `vscode-language-babel` 进行搜索。
- 点击搜索到的 `Babel JavaScript` 扩展的 `Install` 按钮。
- 安装完成后可自动对 JSX 语法高亮。

## 核心概念

### React
React 是一个用于构建用户界面的 JavaScript 库。

### JSX
JSX 是一个 JavaScript 的语法扩展。

```
const element = <h1>Hello, world!</h1>;
```
这个有趣的标签语法既不是字符串也不是 HTML，却可以很好地描述 UI 应该呈现出它应有交互的本质形式。

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

### 元素
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

### 组件 和 props

组件，从概念上类似于 JavaScript 函数。它接受任意的入参（即 “props”），并返回用于描述页面展示内容的 React 元素。也被称为“函数组件”。

定义组件最简单的方式就是编写 JavaScript 函数：

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

也可以使用 ES6 的 class 来定义组件，这种称为“class 组件”：

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

## 参数传递

### 父组件传递值给子组件

```
class Son extends React.Component {
  render() {
    return <h1>My name is {this.props.name}.</h1>;
  }
}

class Daddy extends React.Component {
  const name = "Jack";
  render() {
    return <Son name={name}>;
  }
}
```

### 子组件传递值给父组件 & 子组件调用父组件的方法

```
class Son extends React.Component {
  const something = "gift";
  render() {
    return <button onClick={()=>this.props.handleClick(something)} />;
  }
}
  
class Daddy extends React.Component {
  handleClick(something) {
    console.log(`Get ${something} from my son.`);
  }
    
  render() {
    return <Son handleGift={(i)=>this.handleClick(i)} />;
  }
}
```

### 父组件调用子组件的方法

```
class Son extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      gift: "gift",
    };
  }

  handleGift(something) {
    this.setState({
      gift: something
    })
  }

  render() {
    return <div>Get {this.state.gift} from my daddy.</div>;
  }
}

class Daddy extends React.Component {
  handleClick() {
    this.refs.son.handleGift("flower");
  }
    
  render() {
    return (
      <div>
        <button onClick={()=>this.handleClick()}>Send a Gift</button>
        <Son ref="son" />
      </div>
    )
  }
}
```

