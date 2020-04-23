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

## 约定式路由

如果没有 routes 配置，Umi 会进入约定式路由模式，然后分析 `src/pages` 目录拿到路由配置。

比如以下文件结构：

```
.
  └── pages
    ├── index.tsx
    └── users.tsx
```

会得到以下路由配置，

```
[
  { exact: true, path: '/', component: '@/pages/index' },
  { exact: true, path: '/users', component: '@/pages/users' },
]
```

> 注意：满足以下任意规则的文件不会被注册为路由
> 
> - 以 . 或 _ 开头的文件或目录
> - 以 d.ts 结尾的类型定义文件
> - 以 test.ts、spec.ts、e2e.ts 结尾的测试文件（适用于 .js、.jsx 和 .tsx 文件）
> - components 和 component 目录
> - utils 和 util 目录
> - 不是 .js、.jsx、.ts 或 .tsx 文件
> - 文件内容不包含 JSX 元素

## 动态路由

约定 [] 包裹的文件或文件夹为动态路由。

比如以下文件结构，

```
.
  └── pages
    └── [post]
      ├── index.tsx
      └── comments.tsx
    └── users
      └── [id].tsx
    └── index.tsx
```

会生成路由配置，

```
[
  { exact: true, path: '/:post/', component: '@/pages/[post]/index' },
  { exact: true, path: '/:post/comments', component: '@/pages/[post]/comments', },
  { exact: true, path: '/users/:id', component: '@/pages/users/[id]' },
  { exact: true, path: '/', component: '@/pages/index' },
];
```

## 嵌套路由

Umi 里约定目录下有 `_layout.tsx` 时会生成嵌套路由，以 `_layout.tsx` 为该目录的 layout。layout 文件需要返回一个 React 组件，并通过 `props.children` 渲染子组件。

比如以下目录结构，

```
.
└── pages
    └── users
        ├── _layout.tsx
        ├── index.tsx
        └── list.tsx
```

会生成路由，

```
[
  { exact: false, path: '/users', component: '@/pages/users/_layout',
    routes: [
      { exact: true, path: '/users', component: '@/pages/users/index' },
      { exact: true, path: '/users/list', component: '@/pages/users/list' },
    ]
  }
]
```

## 全局 layout

约定 `src/layouts/index.tsx` 为全局路由。返回一个 React 组件，并通过 `props.children` 渲染子组件。

比如以下目录结构，

```
.
└── src
    ├── layouts
    │   └── index.tsx
    └── pages
        ├── index.tsx
        └── users.tsx
```

会生成路由，

```
[
  { exact: false, path: '/', component: '@/layouts/index',
    routes: [
      { exact: true, path: '/', component: '@/pages/index' },
      { exact: true, path: '/users', component: '@/pages/users' },
    ],
  },
]
```

一个自定义的全局 layout 如下：

```
import { IRouteComponentProps } from 'umi'

export default function Layout({ children, location, route, history, match }: IRouteComponentProps) {
  return children
}
```