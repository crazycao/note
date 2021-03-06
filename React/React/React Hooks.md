# React Hook

参考文档：[https://reactjs.bootcss.com/docs/hooks-intro.html](https://reactjs.bootcss.com/docs/hooks-intro.html)

## Hook 简介

Hook 是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。

Hook 这个单词的意思是"钩子"。

React Hook 的意思是，组件尽量写成纯函数，如果需要外部功能和副作用，就用钩子把外部代码"钩"进来。

Hook 是：

- **完全可选的。** 如果你不想，你不必现在就去学习或使用 Hook。
- **100% 向后兼容的。** Hook 不包含任何破坏性改动。
- **现在可用。** Hook 已发布于 v16.8.0。
- **没有计划从 React 中移除 class。**
- **Hook 不会影响你对 React 概念的理解。**

### 动机——为什么要在 React 中引入 Hook 的原因

#### 在组件之间复用状态逻辑很难
	
React 没有提供将可复用性行为“附加”到组件的途径。虽然已有解决方案，但是每种方案都将使代码变得复杂。React 需要为共享状态逻辑提供更好的原生途径。
	
Hook 使你在无需修改组件结构的情况下复用状态逻辑。 

#### 复杂组件变得难以理解

组件起初很简单，但是逐渐会被状态逻辑和副作用充斥。每个生命周期常常包含一些不相关的逻辑。在多数情况下，不可能将组件拆分为更小的粒度，因为状态逻辑无处不在。这也给测试带来了一定挑战。

Hook 将组件中相互关联的部分拆分成更小的函数（比如设置订阅或请求数据），而并非强制按照生命周期划分。

#### 难以理解的 class

class 是学习 React 的一大屏障。你必须去理解 JavaScript 中 this 的工作方式，这与其他语言存在巨大差异。还不能忘记绑定事件处理器。

但是函数组件有重大限制，必须是纯函数，不能包含状态，也不支持生命周期方法，因此无法取代 class 组件。

Hook 使你在非 class 的情况下可以使用更多的 React 特性。 

## useState()：状态钩子
`useState()` 用于为函数组件引入状态（state）。纯函数不能有状态，所以把状态放在钩子里面。

`useState()` 

- 接受一个参数，state 的初始值。
- 返回一个数组，对 state 的读写操作。
	- 数组的第一个成员是一个变量，指向 state 的当前值。
	- 数组的第二个成员是一个函数，用来更新 state，约定是set前缀加上 state 的变量名。

```
import React, { useState } from "react";

export default function  Button()  {
  const  [buttonText, setButtonText] =  useState("Click me,   please");

  function handleClick()  {
    return setButtonText("Thanks, been clicked!");
  }

  return  <button  onClick={handleClick}>{buttonText}</button>;
}
```

## useContext()：共享状态钩子

如果需要在组件之间共享状态，可以使用 `useContext()`。

假设需要在下面两个组件之间共享状态：

```
<div className="App">
  <Navbar/>
  <Messages/>
</div>
```

第一步：使用 React Context API，在组件外部建立一个 Context。

```
const AppContext = React.createContext({});
```

第二步：使用 Provider 包装组件。

```
<AppContext.Provider value={{
  username: 'superawesome'
}}>
  <div className="App">
    <Navbar/>
    <Messages/>
  </div>
</AppContext.Provider>
```

第三步：在子组件中获取共享状态。

```
const Navbar = () => {
  const { username } = useContext(AppContext);
  return (
    <div className="navbar">
      <p>AwesomeSite</p>
      <p>{username}</p>
    </div>
  );
}
// 另一个子组件中用法相同
```

## useEffect()：副作用钩子

`useEffect()` 用来引入具有副作用的操作，最常见的就是向服务器请求数据。

以前放在 `componentDidMount` 里面的代码，现在可以放在 `useEffect()`。

`useEffect()` 

- 接受两个参数。
	- 第一个参数是一个函数，异步操作的代码放在里面。（各种 async 函数）
	- 第二个参数是一个数组，用于给出 Effect 的依赖项，只要这个数组发生变化，`useEffect()` 就会执行。
		- 第二个参数可以省略，默认情况下，在第一次渲染之后和每次更新之后，都会执行 `useEffect()`。

```
useEffect(()  =>  {
  // Async Action
}, [dependencies])
```




