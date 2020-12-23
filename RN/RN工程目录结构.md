# RN工程目录结构

```
project-name
├── android						Android调试Demo目录
├── ios							iOS调试Demo目录
├── node_modules				各npm依赖库，该文件夹应加入到.gitignore
├── business  					当前RN模块的核心业务代码所在目录
│	 ├── common						公共资源文件所在目录，指多个RN工程都会用到的资源，该目录下不同RN工程的资源文件会被合并到一起
│	 │   └── images						公共图片文件所在目录
│	 └── src					源码目录
│	 	 ├── api 					API请求所在目录
│	 	 │	 └── index.js 				封装各API请求，向外暴露接口；如果用到的接口过多，可以拆分成多个文件，或者按照对接不同的中台拆分成多个文案
│	 	 ├── config					项目配置所在目录
│	 	 │	 ├── constants.js 			当前项目用到的常量定义，包括宏、常量、枚举、字典等
│	 	 │	 └── router.js				页面路由
│	 	 ├── images					项目图片所在目录
│	 	 ├── pages					具体页面代码文件所在目录
│	 	 │	 ├── Page1					页面1的目录，注意页面名称首字母大写，如果页面比较简单，也可以不用文件夹，而直接是一个 Page1.jsx 文件
│	 	 │	 │	 ├── index.jsx				页面的根组件，如果页面较大，可以拆出业务js和子组件
│	 	 │	 │	 ├── business.js			页面的业务逻辑文件，如有多个单词，用“-”连接
│	 	 │	 │	 └── components				页面内的子组件目录
│	 	 │	 │	  	 ├── Component1				组件1的目录，注意组件名称首字母大写
│	 	 │	 │	  	 │	 └── index.jsx			组件1的根文件，如果组件比较简单，也可以不用文件夹，而直接使用 Component1.jsx 文件
│	 	 │	 │	  	 └── Component2.jsx		组件2
│	 	 │	 └── Page2.jsx				页面2，由于内容简单，未使用文件夹
│	 	 ├── redux					redux目录
│	 	 │	 ├── actions				action目录
│	 	 │	 ├── reducers				reducer目录
│	 	 │	 └── store					store目录
│	 	 └── tools					工具文件目录，工具文件名采用“小写字母+连词符”
├── app.json					模块名称配置目录
├── babel.config.js				JS编译器 babel 的配置文件，一般无需修改
├── index.js  					RN应用的入口文件
├── metro.config.js				官方提供的打包模块 Metro 的配置文件，一般无需修改（详见 https://www.jianshu.com/p/d144f0f2dd62）
└── package.json				打包及运行脚本
        
```