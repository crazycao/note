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

[React核心概念](./React/React核心概念.md)

## 将函数组件转换成 class 组件

五个步骤：

1. 创建一个同名的 ES6 class，并且继承于 `React.Component`。
2. 添加一个空的 `render()` 方法。
3. 将函数体移动到 `render()` 方法之中。
4. 在 `render()` 方法中使用 `this.props` 替换 `props`。
5. 删除剩余的空函数声明。

函数组件：

```
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}
```

转换后的 class 组件：

```
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

