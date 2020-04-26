# Umi配置

推荐在 `.umirc.ts` 中写配置。

```
// .umirc.ts
export default {
  base: '/docs/',
  publicPath: '/static/',
  hash: true,
  history: {
    type: 'hash',
  },
}
```

如果配置比较复杂需要拆分，可以放到 `config/config.ts` 中，并把配置的一部分拆出去，比如路由。

```
// config.ts
import { defineConfig } from 'umi';

// 通过 umi 的 defineConfig 方法定义配置，可以在写配置时也有提示
export default defineConfig({
  routes: [
    { path: '/', component: '@/pages/index' },
  ],
});
```

可以按照多环境创建多份配置，并通过环境变量 **UMI_ENV** 区分不同环境来指定配置。最后指定的配置文件会和 `.umirc.ts` 做 deep merge 后形成最终配置。

### 其他说明
- `config/config.ts` 对应的是 `config/config.local.ts`
- `.umirc.ts` 优先级比 `config.ts` 更高。若配置内容有冲突，使用 `.umirc.ts`中的配置。
- `.local.ts` 配置的优先级最高，**UMI_ENV** 指定的配置次之，`.umirc.ts` 最低。
- 不指定 **UMI_ENV** 时不会加载指定环境的配置文件。
- `.umirc.local.ts` 仅在 `umi dev` 时有效。`umi build` 时不会被加载。

> `.umirc.local.ts` > `config/config.local.ts` > `.umirc.cloud.ts` > `config/config.cloud.ts` > `.umirc.ts` > `config/config.ts`

## 运行时配置

运行时配置和配置的区别是他跑在浏览器端，基于此，我们可以在这里写函数、jsx、import 浏览器端依赖等等，注意不要引入 node 依赖。

运行时配置写在 `src/app.tsx` 文件中。

### patchRoutes({ routes }) ---- 修改路由
```
export function patchRoutes({ routes }) {
  routes.unshift({
    path: '/foo',
    exact: true,
    component: require('@/extraRoutes/foo').default,
  }); // 直接 routes，不需要 return
}
```

### render(oldRender: Function) ---- 覆写 render
```
import { history } from 'umi';

export function render(oldRender) {
  fetch('/api/auth').then(auth => {
    if (auth.isLogin) { oldRender() }
    else { history.push('/login'); }
  });
}
```

### onRouteChange({ routes, matchedRoutes, location, action }) ---- 路由切换钩子
```
export function onRouteChange({ location, matchedRoutes, routes, action }) {
  // 用于做埋点统计
  bacon(location.pathname);
  // 用于设置标题
  if (matchedRoutes.length) {
    document.title = matchedRoutes[matchedRoutes.length - 1].route.title || '';
  }
}
```

### rootContainer(LastRootContainer, args) ---- 修改根组件
```
export function rootContainer(container) {
  return React.createElement(ThemeProvider, null, container);
}
```