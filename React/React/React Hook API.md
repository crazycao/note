# React Hooks

原文链接：[https://zh-hans.reactjs.org/docs/hooks-reference.html](https://zh-hans.reactjs.org/docs/hooks-reference.html)

_Hook 是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。_

- 基础 Hook
	- [useState](#useState)
	- [useEffect](#useEffect)
	- [useContext](#useContext)
- 额外的 Hook
	- [useReducer](#useReducer)
	- [useCallback](#useCallback)
	- useMemo
	- useRef
	- useImperativeHandle
	- useLayoutEffect
	- useDebugValue

<span id='useState'>
## useState 状态钩子

```
const [state, setState] = useState(initialState);
```

### 传入参数

- initialState

初始状态，在初始渲染期间，返回的状态 (state) 与传入的第一个参数 (initialState) 值相同。

### 返回值

- state

当前的状态值。

- setState

更新 state 的函数。

> setState 函数接收一个新的 state 值并将组件的一次重新渲染加入队列。
>
> ```
> setState(newState);
> ```
>
> 在后续的重新渲染中，state 的值将始终是更新后最新的 newState。

### 说明

state 和 setState 只是 useState 的返回的两个值，可以用任意变量名承接。建议使用成对的变量名，增加可读性。

#### *函数式更新*

如果新的 state 需要通过使用先前的 state 计算得出，那么可以将函数传递给 setState。该函数将接收先前的 state，并返回一个更新后的值。

```
// 每次调用都对 count 加1 
setCount(preCount => {
  return preCount + 1;
});
// 如果 state 是一个对象，useState 不会自动合并更新对象，需要自己实现
setState(prevState => {
  // 也可以使用 Object.assign
  return {...prevState, ...updatedValues};
});
```

#### *惰性初始 state*

如果初始 state 需要通过复杂计算获得，则可以传入一个函数，在函数中计算并返回初始的 state，此函数只在初始渲染时被调用。

```
const [state, setState] = useState(() => {
  const initialState = someExpensiveComputation(props);
  return initialState;
});
```

<span id='useEffect'>
## useEffect 副作用钩子

```
useEffect(effect, dependent);
```

### 传入参数

- effect

副作用函数。一个包含命令式、且可能有副作用代码的函数。该函数会在组件渲染到屏幕之后执行。

常见的副作用操作有改变 DOM、添加订阅、设置定时器、记录日志以及执行其他包含副作用的操作。

该函数可以返回一个清除函数。用于组件卸载时清除 effect 创建的诸如订阅或计时器 ID 等资源。

- dependent

副作用函数（effect）所依赖的值数组。非必填。

默认情况下，不传 dependent 时，effect 会在每轮组件渲染完成后执行。

传入数值组以后，一旦依赖的数值发生改变，effect 就会执行。建议所有 effect 函数中引用的值都应该出现在依赖项数组中。

如果传入空数组，effect 仅在组件挂载和卸载时执行。（这并不属于特殊情况 —— 它依然遵循输入数组的工作方式。）

### 使用示例

```
useEffect(() => {
  // 创建订阅
  const subscription = props.source.subscribe();
  return () => {
    // 清除订阅
    subscription.unsubscribe();
  };
},
[props.source]);
```

在该示例中，只有当 props.source 改变后才会重新创建订阅。

<span id='useContext'>
## useContext 上下文钩子

```
const value = useContext(MyContext);
```

调用了 useContext 的组件总会在 context 值变化时重新渲染。

### 传入参数

- MyContext

上下文对象。一般是 React.createContext 的返回值。

> 注意：必须是 context 对象本身就，MyContext.Consumer 和 MyContext.Provider 都是不行的。

### 返回值

- value

该上下文（context）的当前值。

### 说明

useContext(MyContext) 只是让你能够读取 context 的值以及订阅 context 的变化。你仍然需要在上层组件树中使用 <MyContext.Provider> 来为下层组件提供 context。

### 使用示例

```
const themes = {
  light: {
    foreground: "#000000",
    background: "#eeeeee"
  },
  dark: {
    foreground: "#ffffff",
    background: "#222222"
  }
};

const ThemeContext = React.createContext(themes.light);

function App() {
  return (
    <ThemeContext.Provider value={themes.dark}>
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

function ThemedButton() {
  const theme = useContext(ThemeContext);
  return (
    <button style={{ background: theme.background, color: theme.foreground }}>
      I am styled by theme context!
    </button>
  );
}
```

当最外层 ThemeContext.Provider 的值（value）改变时，ThemedButton 会重新渲染，而 Toolbar 不变。

<span id='useReducer'>
## useReducer 累加器钩子

```
const [state, dispatch] = useReducer(reducer, initialArg, init);
```

### 传入参数

- reducer

累加器函数。累加器函数接受要改变的 state 和 action，并返回操作之后的 newState。一般写成箭头函数，形如 `(state, action) => newState`。

根据 Redux 的原则，action 应该是一个纯函数（输入相同，输出就一定相同的函数。）。

- initialArg

state 的初始值。

- init

初始值函数。非必填。

适用于如果初始 state 需要通过复杂计算获得的情况。init 函数以 initialArg 为入参，返回计算后的 state。

这么做可以将用于计算 state 的逻辑提取到 reducer 外部，这也为将来对重置 state 的 action 做处理提供了便利

### 返回值

- state

当前的状态值。

- dispatch

与 state 配套的 dispatch 方法。

### 使用示例

```
function init(initialCount) {
  return {count: initialCount};
}

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    case 'reset':
      return init(action.payload);
    default:
      throw new Error();
  }
}

function Counter({initialCount}) {
  const [state, dispatch] = useReducer(reducer, initialCount, init);
  return (
    <>
      Count: {state.count}
      <button
        onClick={() => dispatch({type: 'reset', payload: initialCount})}>
        Reset
      </button>
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
    </>
  );
}
```

<span id='useCallback'>
## useCallback 回调钩子

```
const memoizedCallback = useCallback(fn, deps)
```
### 传入参数

- fn

	内联回调函数。
	
- deps

	依赖项数组。

### 返回值

- memoizedCallback

	内联回调函数的 memoized 版本。
	
### 说明

回调函数仅在某个依赖项改变时才会变化。

依赖项数组并不会作为参数传递给回调函数。

### 使用示例

```
const memoizedCallback = useCallback(
  () => {
    doSomething();
  },
  [a, b],
);
```

## useMemo 记录钩子

## useRef 引用钩子

## useImperativeHandle

## useLayoutEffect

## useDebugValue





