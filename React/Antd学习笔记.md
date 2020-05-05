# Antd学习笔记

> **Ant Design React 和 Ant Design Pro 有什么区别？**
> 可以理解为 Ant Design React 是一套 React 组件库，而 Pro 是使用了这套组件库的完整前端脚手架。

## 安装
新建一个 **空的** 文件夹作为项目目录，并在目录下执行：

```
npm create umi
```
屏幕会出现如下选项，请选择 `ant-design-pro`。

```
Select the boilerplate type (Use arrow keys)
❯ ant-design-pro  - Create project with an layout-only ant-design-pro boilerplate, use together with umi block.
  app             - Create project with a simple boilerplate, support typescript.
  block           - Create a umi block.
  library         - Create a library with umi.
  plugin          - Create a umi plugin.
```

然后，安装程序会询问你是否新手。如果写 `y`，那就会按照默认配置自动安装脚手架；写 `N` 则进入自定义配置。 

```
? 🧙 Be the first to experience the new umi@3 ? (y/N) 
```

为了说明配置情况，我们选择 `N`。然后安装器继续询问使用哪种语言。默认配置中是 `TypeScript`。然而，如果你只会 `JavaScript` 就别逞强。

```
? 🤓 Which language do you want to use? (Use arrow keys)
❯ TypeScript 
  JavaScript 
```

安装器接下来问是要安装所有的模块，还是只是一个简单的脚手架。默认配置中是 `simple`。个人习惯是先安装完整的，然后再慢慢删除。当然也可以先安装简单的再慢慢添加。

```
? 🚀 Do you need all the blocks or a simple scaffold? (Use arrow keys)
❯ simple 
  complete 
```

最后一个询问的问题，作者在安利你用 `antd@4`。用最新版的框架也没什么不好，就选 `Y`。选 `n` 的话就安装 `antd@3`，然而可能安装失败……

```
? 🦄 Time to use better, faster and latest antd@4! (Y/n) 
```

终于开始自动安装，所以一开始就承认自己是新手就省心很多。最后出现下面的输出说明已经安装完成。

```
> 🚚 clone success
> [Sylvanas] Prepare js environment...
> [JS] Clean up...
> Clean up...
✨ File Generate Done
```

## 目录结构

```
├── config                   # umi 配置，包含路由，构建等配置
├── mock                     # 本地模拟数据
├── public
│   └── favicon.png          # Favicon
├── src
│   ├── assets               # 本地静态资源
│   ├── components           # 业务通用组件
│   ├── e2e                  # 集成测试用例
│   ├── layouts              # 通用布局
│   ├── models               # 全局 dva model
│   ├── pages                # 业务页面入口和常用模板
│   ├── services             # 后台接口服务
│   ├── utils                # 工具库
│   ├── locales              # 国际化资源
│   ├── global.less          # 全局样式
│   └── global.ts            # 全局 JS
├── tests                    # 测试工具
├── README.md
└── package.json
```

## ESlint

```
npm install eslint --save-dev
npm install eslint-plugin-react --save-dev
```

## 布局

页面整体布局是一个产品最外层的框架结构，往往会包含导航、页脚、侧边栏、通知栏以及内容等。

在 Ant Design Pro 中，我们抽离了使用过程中的通用布局，都放在 layouts 目录中，分别为：

- BasicLayout：基础页面布局，包含了头部导航，侧边栏和通知栏。
- UserLayout：抽离出用于登录注册页面的通用布局。
- BlankLayout：空白的布局。

## 路由

目前脚手架中所有的路由都通过 `config.ts` 来统一管理。

配置规则参见 [Umi路由](./Umi/Umi路由.md) （https://umijs.org/zh-CN/docs/routing）。支持以下主要参数：

- `path` 路由路径。用于与 `url` 匹配。可以包含通配符。
- `component` 路由组件。配置用于渲染的 React 组件路径。可以是绝对路径，也可以是相对路径。
- `exact` 是否严格匹配。默认 `false`，`url` 中包含 `path` 即可匹配上；否则需要完全相同才能匹配成功。
- `redirect` 配置路由跳转。访问 `path` 时则会跳转到 `redirect` 指定的另一个路由。
- `routes` 子路由。是一个数组，数组内每个元素都是一个路由，可以循环下去。

Antd 在 umi 的配置中我们增加了一些参数，来辅助生成菜单。

- `name` 和 `icon` 分别代表生成菜单项的文本和图标。
- `hideChildrenInMenu` 用于隐藏不需要在菜单中展示的子路由。
- `hideInMenu` 可以在菜单中不展示这个路由，包括子路由。
- `authority` 用来配置这个路由的权限，如果配置了将会验证当前用户的权限，并决定是否展示。

## 菜单
根据路由配置来生成菜单。菜单项名称，嵌套路径与路由高度耦合。

可以配置 `hideInMenu: true` 让某些路由在菜单中不显示。

可以在 `src/layouts/BasicLayout.tsx` 中修改 `menuDataRender`，并在代码中发起 `http` 请求，只需服务器返回路由配置格式的 json 即可。

## 面包屑

面包屑由 `PageHeaderWrapper` 实现。

`PageHeaderWrapper` 必须要被 `ProLayout` 包裹才能自动生成面包屑和标题。