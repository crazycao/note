# React Hooks

**React 函数组件** 有重大限制，必须是纯函数，不能包含状态，也不支持生命周期方法，因此无法取代类。

**React Hooks 的设计目的**，就是加强版函数组件，完全不使用"类"，就能写出一个全功能的组件。

Hook 这个单词的意思是"钩子"。

React Hooks 的意思是，组件尽量写成纯函数，如果需要外部功能和副作用，就用钩子把外部代码"钩"进来。

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
		- 第二个参数可以省略，这时每次组件渲染时，都会执行 `useEffect()`。

```
useEffect(()  =>  {
  // Async Action
}, [dependencies])
```




