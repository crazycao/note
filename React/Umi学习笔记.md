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

[Umi配置](./Umi/Umi配置.md)

## 路由

[Umi路由](./Umi/Umi路由.md)

## 插件

[Umi插件](./Umi/Umi插件.md)

## Mock

[Umi Mock](./Umi/Umi\ Mock.md)

## 样式和资源

[Umi样式和资源](./Umi/Umi样式和资源.md)


