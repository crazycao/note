# Umi学习笔记

## 环境准备
操作系统：支持 macOS，Linux，Windows

运行环境：Node >= 10，React >= 16.8.0，IE 8 以上

## 介绍
Umi，中文可发音为乌米。

Umi 是蚂蚁金服的底层前端框架。

Umi 以路由为基础的，同时支持配置式路由和约定式路由，保证路由的功能完备，并以此进行功能扩展。然后配以生命周期完善的插件体系，覆盖从源码到构建产物的每个生命周期，支持各种功能扩展和业务需求。

## 快速上手

1. 创建一个 **空目录**。

	```
	mkdir myapp && cd myapp
	```

2. 通过官方工具创建项目。

	```
	yarn create @umijs/umi-app
	```

3. 安装依赖。

	```
yarn
```

4. 启动项目。

	```
yarn start
```

	在浏览器里打开 [http://localhost:8000/](http://localhost:8000/) 即可。

5. 构建。

	```
	yarn build
	```

	构建产物默认生成到 `./dist` 下。

6. 本地验证构建产物。

	```
	yarn global add serve
	serve ./dist
	```

	发布之前，可以通过 serve 做本地验证。

## 目录结构

```
├── package.json		// 包含插件和插件集，以 @umijs/preset-、@umijs/plugin-、umi-preset- 和 umi-plugin- 开头的依赖会被自动注册为插件或插件集。
├── .umirc.ts			// 配置文件，包含 umi 内置功能和插件的配置。
├── .umirc.local.ts		// 本地临时配置，不要提交到 git 仓库。
├── .env				// 环境变量。
├── dist				// 执行 umi build 后，产物默认会存放在这里。
├── mock				// 存储 mock 文件，此目录下所有 js 和 ts 文件会被解析为 mock 文件。
├── public				// 此目录下所有文件会被 copy 到输出路径。
├── config				// 配置文件目录。
|   ├── config.ts			// 配置文件	
|   └── config.local.ts		// 本地临时配置文件。不要提交到 git 仓库。
└── src					// 代码目录
    ├── .umi					// 临时文件目录，比如入口文件、路由等，都会被临时生成到这里。不要提交 .umi 目录到 git 仓库。
    ├── layouts/index.tsx		// 约定式路由时的全局布局文件。
    ├── pages					// 所有路由组件存放在这里。
    |   ├── index.less
    |   └── index.tsx
    └── app.ts					// 运行时配置文件，可以在这里扩展运行时的能力，比如修改路由、修改 render 方法等。
```

## 配置

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

## 路由

[Umi路由](./Umi/Umi路由.md)