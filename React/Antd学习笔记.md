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

## 路由

目前脚手架中所有的路由都通过 `config.ts` 来统一管理。