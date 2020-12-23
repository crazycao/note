# React Native


## React Native相对于原生的ios和Android有哪些优势？

- 性能媲美原生APP
- 使用JavaScript编码，只要学习这一种语言
- 绝大部分代码安卓和IOS都能共用
- 组件式开发，代码重用性很高
- 跟编写网页一般，修改代码后即可自动刷新，不需要慢慢编译，节省很多编译等待时间
- 支持APP热更新，更新无需重新安装APP

缺点：

- 内存占用相对较高
- 版本还不稳定，一直在更新，现在还没有推出稳定的1.0版本
- 仍然有一些兼容性的坑需要自己填

## 请介绍 React Native 组件的生命周期

- mount x2 : 组件挂载时
	- componentWillMount
	- componentDidMount

- update x3 : props 或 state 变化时
	- shouldComponentUpdate
	- componentWillUpdate
	- componentDidUpdate

- unmount x1 : 组件卸载时
	- componentWillUnmount 

## React 在调用 setState 之后发生了什么

- 将传入的对象与组件当前状态合并
- 根据新的状态构建 React 元素树
- 计算新树与老树的节点差异
- 根据节点差异对界面进行最小化重新渲染（这就保证了按需更新）

## React Native 如何与原生通信

- RN 调原生：通过 `RCTBridgeModule`
	- 实现一个遵守 `RCTBridgeModule` 协议的类（原生模块）
	- 在类的实现中通过 `RCT_EXPORT_METHOD` 宏来声明要给 JS 导出的方法
	- 在 JS 中通过 `NativeModules` 获取原生模块，并调用导出的方法
	- 原生模块通过回调函数把返回值传给 JS
- 原生调 RN：通过 `RCTEventEmitter`
	- 原生模块需要继承 `RCTEventEmitter` 类
	- 并实现 `suppportEvents` 方法，在该方法中返回 Event 数组
	- 在需要发送事件时调用 `[self sendEventWithName:] ` 
	- 在 JS 中 new 一个 `NativeEventEmitter` 对象来订阅 Event，并在 `componentWillUnmount` 时取消订阅。

## iOS和Android的第三方原生依赖库，分别是以什么方式导入到原生工程的。
- 第一步：安装一个带原生依赖的库：

	```
	$ npm install 某个带有原生依赖的库
	```
- 第二步：链接依赖库

	```
	$ npx react-native link
	```
	
	它会根据package.json文件中的dependencies和devDependencies记录来链接所有需要链接的库
- 好了！现在原生依赖就成功地链接到你的 iOS/Android 项目了。

## react-native父组件和子组件直接如何交互？

- 父组件传递属性到子组件
	- 子组件可以设定属性的类型和默认值，由父组件传入 
- 父组件调用子组件的方法
	- 通过ref指向子组件，在需要的地方调用子组件中的方法。 
		- 给子组件添加 ref 属性
		- 在父组件中可以直接调用，`this.子组件的ref.子组件的方法`
- 子组件调用父组件中的方法
	- 父组件将回调方法作为属性传递给子组件，子组件在需要的地方调用 

## react-native兄弟组件之间如何交互?
- 借助共同的父组件进行通信
- 使用 connect 绑定共同的 store

## https://blog.csdn.net/qq_33323251/article/details/80014166
