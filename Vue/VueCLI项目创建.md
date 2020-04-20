# VueCLI项目创建


> **环境依赖**
> 
> Node.js 8.9 或更高版本 (推荐 8.11.0+)**

## 安装

```
npm install -g @vue/cli
```

## 查看版本

```
vue --version
```

## 创建一个项目

```
vue create hello-world
```

然后，终端会出现，下面的选项：

```
? Please pick a preset: (Use arrow keys)
❯ default (babel, eslint) 
  Manually select features 
```
可以直接选择 默认配置，也可以通过手动选择添加 Vuex、Router 等能力。后面就根据提示按需选择就好。

## 运行项目

在这个 App.vue 文件所在的目录下运行：

```
vue serve
```

## 构建项目

```
vue build
```


