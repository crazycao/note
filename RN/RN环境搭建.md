# RN环境搭建


前置条件，安装 node

```
	brew install node
```	


## Expo方式（适用于只有js代码时）
1. 在手机上安装 Expo Client。从 App Store 下载。
2. 在电脑上安装 Expo Client 工具

	```
	npm install -g expo-cli
	```
3. 初始化RN工程

	```
	expo init [工程名称]
	```
4. 启动工程

	```
	cd [工程目录]
	npm start （或  yarn start）
	```
5. 打开手机上的 Expo Client APP。
6. 找到相应的工程打开。（注意手机和电脑要在同一局域网内。）

## RN-Client 方式（常用方式）
1. 在电脑上安装 Xcode 和 Android Studio。
2. 确认 Xcode 已有命令行工具 Command Line Tools。
`Xcode --> Preferences --> Locations --> Command Line Tools`
3. 安装 watchman 文本变动监控工具

	```
	brew install watchman
	```
4. 安装 RN-Client
 
	```
	npm install -g react-native-cli
	```
5. 初始化 RN 工程

	```
	react-native init [工程名称]
	```
6. 启动工程（会启动模拟器）

	```
	cd [工程目录]
	react-native run-ios（或 react-native run-ios）
	```

## npx 方式（新方式）

1. 在电脑上安装 Xcode 和 Android Studio。
2. 确认 Xcode 已有命令行工具 Command Line Tools。
`Xcode --> Preferences --> Locations --> Command Line Tools`
3. 安装 watchman 文本变动监控工具

	```
	brew install watchman
	```
4. 注意卸载 RN-Client 避免冲突
 
	```
	npm uninstall -g react-native-cli
	```
5. 设置淘宝源

	```
	npx nrm use taobao
	```
5. 初始化 RN 工程

	```
	npx react-native init [工程名称]
	```
6. 启动工程（会启动模拟器）

	```
	cd [工程目录]
	yarn ios（或 yarn android）
	```
	也可以在 Xcode 中直接运行应用。（打开 `.xcworkspace` 文件即可。）