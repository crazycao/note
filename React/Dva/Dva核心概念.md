# Dva核心概念

## 数据流向

数据的改变发生通常是通过用户交互行为或者浏览器行为（如路由跳转等）触发的，当此类行为会改变数据的时候可以通过 `dispatch` 发起一个 `action`，如果是同步行为会直接通过 `Reducers` 改变 `State` ，如果是异步行为（副作用）会先触发 `Effects` 然后流向 `Reducers` 最终改变 `State`，所以在 dva 中，数据流向非常清晰简明，并且思路基本跟开源社区保持一致。

![dva](./dva数据流向.png)

## State —— 状态

`State` 表示 `Model` 的状态数据，通常表现为一个 javascript 对象（当然它可以是任何值）；操作的时候每次都要当作不可变数据（immutable data）来对待，保证每次都是全新对象，没有引用关系，这样才能保证 `State` 的独立性，便于测试和追踪变化。

## Action —— 操作

`Action` 是一个普通 javascript 对象，它是改变 `State` 的唯一途径。无论是从 UI 事件、网络回调，还是 WebSocket 等数据源所获得的数据，最终都会通过 `dispatch` 函数调用一个 `action`，从而改变对应的数据。`action` 必须带有 `type` 属性指明具体的行为，其它字段可以自定义，如果要发起一个 `action` 需要使用 `dispatch` 函数；需要注意的是 `dispatch` 是在组件 connect Models 以后，通过 `props` 传入的。

```
dispatch({
  type: 'add',
});
```

## dispatch 函数 —— 调度

`dispatch` 函数 是一个用于触发 `action` 的函数，`action` 是改变 `State` 的唯一途径，但是它只描述了一个行为，而 `dipatch` 可以看作是触发这个行为的方式，而 `Reducer` 则是描述如何改变数据的。

```
dispatch({
  type: 'user/add', // 在 model 外调用，需要添加 namespace
  payload: {}, // 需要传递的信息
});
```

## Reducer —— 累加器

Reducer（也称为 reduce 函数）函数接受两个参数：之前已经累积运算的结果和当前要被累积的值，返回的是一个新的累积结果。该函数把一个集合归并成一个单值。

> 参考：JavaScript 的 reduce 函数（累加器）。

需要注意的是 `Reducer` 必须是纯函数，所以同样的输入必然得到同样的输出，它们不应该产生任何副作用。并且，每一次的计算都应该使用 immutable data，这种特性简单理解就是每次操作都是返回一个全新的数据（独立，纯净），所以热重载和时间旅行这些功能才能够使用。

## Effect —— 副作用

Effect 被称为副作用，在我们的应用中，最常见的就是异步操作。它来自于函数编程的概念，之所以叫副作用是因为它使得我们的函数变得不纯，同样的输入不一定获得同样的输出。

dva 为了控制副作用的操作，底层引入了 `redux-sagas` 做异步流程控制，由于采用了 `generator` 的相关概念，所以将异步转成同步写法，从而将 `effects` 转为纯函数。

## Subscription —— 订阅

`Subscriptions` 是一种从 **源** 获取数据的方法，它来自于 `elm`。

`Subscription` 语义是订阅，用于订阅一个数据源，然后根据条件 `dispatch` 需要的 `action`。

数据源可以是当前的时间、服务器的 websocket 连接、keyboard 输入、geolocation 变化、history 路由变化等等。

```
import key from 'keymaster';
...
app.model({
  namespace: 'count',
  subscriptions: {
    keyEvent({dispatch}) {
      key('⌘+up, ctrl+up', () => { dispatch({type:'add'}) });
    },
  }
});
```

## Router —— 路由

路由通常指的是前端路由。

由于我们的应用现在通常是单页应用，所以需要前端代码来控制路由逻辑，通过浏览器提供的 History API 可以监听浏览器 url 的变化，从而控制路由相关操作。

dva 实例提供了 `router` 方法来控制路由，使用的是 `react-router`。

```
import { Router, Route } from 'dva/router';
app.router(({history}) =>
  <Router history={history}>
    <Route path="/" component={HomePage} />
  </Router>
);
```

## Route Components —— 路由组件

在 dva 中，通常需要 connect Model 的组件都是 `Route Components`，组织在 `/routes/` 目录下，而 `/components/` 目录下则是纯组件（Presentational Components）。


