# Dva核心概念

## 数据流向

数据的改变发生通常是通过用户交互行为或者浏览器行为（如路由跳转等）触发的，当此类行为会改变数据的时候可以通过 `dispatch` 发起一个 `action`，如果是同步行为会直接通过 `Reducers` 改变 `State` ，如果是异步行为会先触发 `Effects` 然后流向 `Reducers` 最终改变 `State`，所以在 dva 中，数据流向非常清晰简明。

![dva](./dva数据流向.png)

## State —— 状态

`State` 表示 `Model` 的状态数据，通常表现为一个 javascript 对象（当然它可以是任何值）。

## Action —— 操作

`Action` 是一个普通 javascript 对象，它是改变 `State` 的唯一途径。

`type` 属性指明具体的行为，其它字段可以自定义。

## dispatch 函数 —— 调度

`dispatch` 函数 是一个用于触发 `action` 的函数。

`action` 是改变 `State` 的唯一途径，但是它只描述了一个行为，而 `dipatch` 可以看作是触发这个行为的方式，而 `Reducer` 则是描述如何改变数据的。

```
dispatch({
  type: 'user/add', // 在 model 外调用，需要添加 namespace
  payload: {}, // 需要传递的信息
});
```

## Reducer —— 累加器

处理**同步**动作的 `Action` 处理器，用来算出最新的 `State`。

## Effect —— 副作用

处理**异步**动作的 `Action` 处理器。

## Subscription —— 订阅

`Subscriptions` 是一种从 **源** 获取数据的方法，用于订阅一个数据源，然后根据条件 `dispatch` 需要的 `action`。

## Router —— 路由

路由通常指的是前端路由。

## Route Components —— 路由组件

在 dva 中，通常需要 connect Model 的组件都是 `Route Components`，组织在 `/routes/` 目录下，而 `/components/` 目录下则是纯组件（Presentational Components）。


