
# Taro目录结构

参考资料：[https://taro-docs.jd.com/taro/docs/tutorial](https://taro-docs.jd.com/taro/docs/tutorial)

## 初始

```
├── dist                   编译结果目录
├── config                 配置目录
|   ├── dev.js             开发时配置
|   ├── index.js           默认配置
|   └── prod.js            打包时配置
├── src                    源码目录
|   ├── pages              页面文件目录
|   |   ├── index          index 页面目录
|   |   |   ├── index.config.js   index 页面配置
|   |   |   ├── index.js   index 页面逻辑
|   |   |   └── index.css  index 页面样式
|   ├── app.config.js      项目全局配置
|   ├── app.css            项目总通用样式
|   └── app.js             项目入口文件
└── package.json
```


## 完整

```
.
├── config
│   ├── dev.js 				// 开发打包配置
│   ├── index.js 			// 基础项目打包配置
│   └── prod.js				// 生产打包配置
├── dist 						// 编译结果目录
├── package.json
└── src						// 源码目录
    ├── app.config.js     	// 项目全局配置
    ├── app.css           	// 项目总通用样式
    ├── app.js            	// 项目入口文件
    ├── assets            	// 项目资源目录
    ├── actions // redux相关
    ├── api 					// 业务网络接口请求
    ├── components 			// 公共组件==》待沉淀到业务组件库
    ├── pages					// 页面文件目录
    │   └── index 			// index 页面目录
    │       ├── assets 		// (可选)index 页面资源文件
    │       │   ├── images	//	index 页面的图片
    │       │   └── less	// index 页面的样式
    │       ├── components	// (可选)index 页面中用到的组件
    │       │   └── test-component
    │       │       ├── assets // 可选
    │       │       │   ├── images
    │       │       │   └── less
    │       │       └── index.jsx
    │       ├── index.config.js	// index 页面配置
    │       ├── index.jsx			// index 页面逻辑
    │       └── index.less			// index 页面样式
    ├── reducers // redux相关
    └── store // redux相关
    └── utils // 功能函数==》待沉淀到业务工具库
```