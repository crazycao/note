
# Taro学习笔记

参考资料：[https://taro-docs.jd.com/taro/docs/README](https://taro-docs.jd.com/taro/docs/README)

## 简介

Taro 是一个开放式跨端跨框架解决方案，支持使用 React/Vue/Nerv 等框架来开发微信/京东/百度/支付宝/字节跳动/ QQ 小程序/H5 等应用。

## 快速开始

- 环境依赖：node.js（>=8.0.0）

- CLI 工具安装：

    ```
    # 使用 npm 全局安装 CLI
    npm install -g @tarojs/cli
    ```

- 项目初始化：

    ```
    taro init myApp
    ```

- 安装依赖：

    ```
    npm install
    ```

- 运行

    - 微信小程序

    选择微信小程序模式，需要自行下载并打开微信开发者工具，然后选择项目根目录进行预览。

    ```
    npm run dev:weapp
    npm run build:weapp
    ```

    - 百度小程序

    选择百度小程序模式，需要自行下载并打开百度开发者工具，然后在项目编译完后选择项目根目录下 dist 目录进行预览。

    ```
    npm run dev:swan
    npm run build:swan
    ```

    - 支付宝小程序

    选择支付宝小程序模式，需要自行下载并打开支付宝小程序开发者工具，然后在项目编译完后选择项目根目录下 dist 目录进行预览。

    ```
    npm run dev:alipay
    npm run build:alipay
    ```

    - 字节跳动小程序

    选择字节跳动小程序模式，需要自行下载并打开字节跳动小程序开发者工具，然后在项目编译完后选择项目根目录下 dist 目录进行预览。

    ```
    npm run dev:tt
    npm run build:tt
    ```

    - QQ小程序

    选择 QQ 小程序模式，需要自行下载并打开QQ 小程序开发者工具，然后在项目编译完后选择项目根目录下 dist 目录进行预览。

    ```
    npm run dev:qq
    npm run build:qq
    ```

