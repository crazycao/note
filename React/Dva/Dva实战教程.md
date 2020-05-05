# Dva实战教程

[https://github.com/dvajs/dva-docs/tree/master/v1/zh-cn/tutorial](https://github.com/dvajs/dva-docs/tree/master/v1/zh-cn/tutorial)

## 目录结构
```
.
├── /mock/           # 数据mock的接口文件
├── /src/            # 项目源码目录
│ ├── /components/   # UI组件
│ ├── /routes/       # 路由组件（页面维度）
│ ├── /models/       # 数据模型
│ ├── /services/     # 数据接口
│ ├── /utils/        # 工具函数
│ ├── route.js       # 路由配置
│ ├── index.js       # 入口文件
│ ├── index.less     
│ └── index.html     
├── package.json     # 项目信息
└── proxy.config.js  # 数据mock配置
```

## 设计 Model

在了解了项目基本的结构划分以后，我们将要开始设计 `model`。

抽离 `model` 的原则就是抽离数据模型。

通常有以下两种方式：

- 按照数据维度
- 按照业务维度

### 数据维度

按照数据维度的 `model` 设计原则就是抽离数据本身以及相关操作的方法。

只关心数据本身，至于在使用 `model` 的组件中所遇到的状态管理等信息跟 `model` 无关，而是作为组件自身的 `state` 维护。

这种设计方式使得 `model` 很纯粹，在设计通用数据信息 `model` 的时候比较适用。

但是在数据跟业务状态交互比较紧密，数据不是那么独立的时候会有些不那么方便。

### 业务维度

按照业务维度的 `model` 设计，则是将数据以及使用强关联数据的组件中的状态统一抽象成 `model` 的方法。

将业务状态也一并放到 `model` 当中去了，这样所有状态的变化都会在 `model` 中控制，会更容易跟踪和操作。

## 组件设计

在拆分 `Component` 的过程中要尽量让每个 `Component` 专注做自己的事。

一般来说，我们的组件有两种设计：

- Container Component
- Presentational Component

### 容器组件（Container Component）

一般指的是具有监听数据行为的组件，一般来说它们的职责是绑定相关联的 `model` 数据，以数据容器的角色包含其它子组件，通常在项目中表现出来的类型为：布局、路由组件以及普通容器组件。

```
import React, { Component, PropTypes } from 'react';

// dva 的 connect 方法可以将组件和数据关联在一起
import { connect } from 'dva';

// 组件本身
const MyComponent = (props)=>{};
MyComponent.propTypes = {};

// 监听属性，建立组件和数据的映射关系
function mapStateToProps(state) {
  return {...state.data};
}

// 关联 model
export default connect(mapStateToProps)(MyComponent);
```

### 展示组件（Presentational Component）

展示组件的名称已经说明了它的职责，展示形组件，一般也称作：Dumb Component（沉默组件），它不会关联订阅 `model` 上的数据，而所需数据的传递则是通过 `props` 传递到组件内部。

```
import React, { Component, PropTypes } from 'react';

// 组件本身
// 所需要的数据通过 Container Component 通过 props 传递下来
const MyComponent = (props)=>{}
MyComponent.propTypes = {};

// 并不会监听数据
export default MyComponent;
```

### 对比

对组件分类，主要有两个好处：

- 让项目的数据处理更加集中；
- 让组件高内聚低耦合，更加聚焦；

试想如果每个组件都去订阅数据 `model`，那么一方面组件本身跟 `model` 耦合太多，另一方面代码过于零散，到处都在操作数据，会带来后期维护的烦恼。

在设计思路上两个组件也有很大不同。 展示组件是独立的纯粹的，每个组件跟业务数据并没有耦合关系，只是完成自己独立的任务，需要的数据通过 `props` 传递进来，需要操作的行为通过接口暴露出去。 而容器组件更像是状态管理器，它表现为一个容器，订阅子组件需要的数据，组织子组件的交互逻辑和展示。

### 组件定义

定义我们的组件一般有三种方式：

```
// 1. 传统写法（不推荐）
const App = React.createClass({});

// 2. es6 的写法（组件涉及 react 的生命周期方法时采用）
class App extends React.Component({});

// 3. stateless 的写法（我们推荐的写法）
const App = (props) => ({});
```

## 关联 Model

```
function mapStateToProps(state) {
	 const { users } = state; // 从顶层 state 中获取 users（namespace 为 'users' 的 model），即 model/users.js 中的 state 所有数据
    return { users }; // 返回 'users': users，可以用 this.props.users 获取
}
export default connect(mapStateToProps)(Users);
```

注意需要先在 `src/index.js` 中加载 `model`

```
app.model(require('./models/users').default);

```

## 添加 Reducer

在 dva 中 `reducers` 主要负责修改 `model` 的数据（`state`）。

## 发起 Actions

通过 `dispatch` 函数，可以通过 `type` 属性指定对应的 `actions` 类型，而这个类型名在 `reducers`（`effects`）会一一对应，从而知道该去调用哪一个 `reducers`（`effects`），除了 `type` 以外，其它对象中的参数随意定义，都可以在对应的 `reducers`（`effects`）中获取，从而实现消息传递，将最新的数据传递过去更新 `model` 的数据（`state`）。

通常我们建议在组件内部的生命周期发起，如：

```
componentDidMount() {
	this.props.dispatch({
		type: 'model/action',
	});
}
```

## 添加 Effects

`Reducers` 的本质是修改 `model` 的 `state`，而 `Effects` 主要是 控制数据流程 ，所以最终往往我们在 `Effects` 中会调用 `Reducers`。

我们需要增加 `*query` 第二个参数 `*query({ payload }, { select, call, put })` ，其中 `call` 和 `put` 是 dva 提供的方便操作 `effects` 的函数，简单理解 `call` 是调用执行一个函数而 `put` 则是相当于 `dispatch` 执行一个 `action`，而 `select` 则可以用来访问其它 `model`。

注意：无论是 `Generator` 函数，`yield` 亦或是 `async` 目的只有一个：*让异步编写跟同步一样*，从而能够很好的控制执行流程。

## 定义 Services

将请求相关（与后台系统的交互）抽离出来，单独放到 `/services/` 中，进行统一维护管理。

## Mock 数据

## 添加样式

在 dva 中，我们推荐使用 `CSS Modules` 的解决方案，不同组件的样式相互之间不会造成污染。

配合 webpack 的 `css-loader` 进行打包，会为所有的 `class name` 和 `animation name` 加 `local scope`，避免潜在冲突。

## 设计布局

布局本质上还是一种组件，将布局样式抽象成组件，能够保持子组件和父组件的独立性，不用在其中关联到布局信息。