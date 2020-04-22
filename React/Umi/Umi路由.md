# Umi路由

通过配置文件（`.umirc.ts` 或 `config/config.ts`）中的 routes 进行配置，格式为路由信息的数组。

## 配置参数
### path
- 路由路径
- Type: string

用于与 location 匹配。可以包含通配符。

### component
- 路由组件
- Type: string

配置用于渲染的 React 组件路径。可以是绝对路径，也可以是相对路径，如果是相对路径，会从 `src/pages` 开始找起。也可以用 `@` 指向 `src` 目录的文件。

```
export default {
  routes: [
    { path: '/', component: 'index' },
    { path: '/user', component: '../page/user' },
    { path: '/home', component: '@/page/home' },
  ],
}
```
### exact
- 是否严格匹配
- Type: boolean
- Default: false

即 location 是否和 path 完全对应上。

```
export default {
  routes: [
    // url 为 /one/two 时匹配失败
    { path: '/one', exact: true },
    
    // url 为 /one/two 时匹配成功
    { path: '/one' },
    { path: '/one', exact: false },
  ],
}
```

### routes
- 子路由
- Type: Object[]


配置子路由，通常在需要为多个路径增加 layout 组件时使用。

```
export default {
  routes: [
    { path: '/login', component: 'login' },
    {
      path: '/',
      component: '@/layouts/index',
      routes: [
        { path: '/list', component: 'list' },
        { path: '/admin', component: 'admin' },
      ],
    }, 
  ],
}
```

然后在 src/layouts/index 中通过 props.children 渲染子路由，

```
export default (props) => {
  return <div style={{ padding: 20 }}>{ props.children }</div>;
}
```
这样，访问 `/list` 和 `/admin` 就会带上 `src/layouts/index` 这个 layout 组件。

### redirect

- 配置路由跳转
- Type: string

```
export default {
  routes: [
    { exact: true, path: '/', redirect: '/list' },
    { exact: true, path: '/list', component: 'list' },
  ],
}
```

访问 `/` 会跳转到 `/list`，并由 `src/pages/list` 文件进行渲染。

### wrappers
- 配置路由的高阶组件封装。
- Type: string[]

可以用于路由级别的权限校验：

```
export default {
  routes: [
    { path: '/user', component: 'user',
      wrappers: [
        '@/wrappers/auth',
      ],
    },
    { path: '/login', component: 'login' },
  ]
}
```

然后在 `src/wrappers/auth` 中，

```
export default (props) => {
  const { isLogin } = useAuth();
  if (isLogin) {
    return <div>{ props.children }</div>;
  } else {
    redirectTo('/login');
  }
}
```

这样，访问 `/user`，就通过 `useAuth` 做权限校验，如果通过，渲染 `src/pages/user`，否则跳转到 `/login`，由 `src/pages/login` 进行渲染。

### title

- 配置路由的标题。
- Type: string

## 页面跳转

```
import { history } from 'umi';

// 跳转到指定路由
history.push('/list');

// 带参数跳转到指定路由
history.push('/list?a=b');
history.push({
  pathname: '/list',
  query: {
    a: 'b',
  },
});

// 跳转到上一个路由
history.goBack();
```

## Link 组件

```
import { Link } from 'umi';

export default () => (
  <div>
    <Link to="/users">Users Page</Link>
  </div>
);
```

然后点击 Users Page 就会跳转到 /users 地址。

> 注意：
> Link 只用于单页应用的内部跳转，如果是外部地址跳转请使用 a 标签。

## 传递参数给子路由

```
import React from 'react';

export default function Layout(props) {
  return React.Children.map(props.children, child => {
    return React.cloneElement(child, { foo: 'bar' });
  });
}
```

通过 `cloneElement` 传递参数。