# Vue.js 目录结构
```
vue-project
├── build                                Webpack构建命令目录，包括运行环境、项目打包等配置文件
│   ├── build.js                         生产环境构建代码
│   ├── check-version.js                 检查node、npm等版本
│   ├── dev-client.js                    开发服务器热重载脚本，主要用来实现开发阶段的页面自动刷新
│   ├── dev-server.js                    构建本地服务器相关
│   ├── util.js                          构建相关工具方法
│   ├── webpack.base.conf.js             webpack基础配置  
│   ├── webpack.dev.conf.js              webpack开发环境配置  
│   └── webpack.prod.conf.js             webpack生产环境配置             
├── config                               webpack和node基础，不同环境变量的配置
|   ├── index.js                         项目配置文件，配置node监听端口、静态文件位置，静态文件引用前缀、node代理等
|   ├── dev.env.js                       开发环境变量
│   ├── prod.env.js                      生产环境变量
|   └── test.env.js                      测试环境变量
├── dist                                 webpack打包后生成的静态文件目录
├── node_modules                         项目依赖的 js 库
├── src                                  项目源代码目录
|   ├── api                              项目所有接口封装目录
|   ├── assets                           资源目录，这里的资源会被wabpack构建
|   ├── components                       公共组件目录   
|   ├── directives                       自定义指令目录         
|   ├── router                           前端路由目录         
|   ├── store                            状态管理（如 vuex）目录         
|   ├── styles                           Sass 目录         
|   ├── libs                             第三方库目录         
│   ├── utils                            自定义工具集
│   ├── views                            页面文件所在目录
│   |   ├── login
│   |   |   ├── components               login页面用到的专用组件
│   |   |   └── index.vue                login页面的核心源码
│   |   └── 404.vue                      找不到时的通用页面
│   ├── App.vue                          项目的根组件 
│   └── main.js                          项目入口文件，在此处加载各种公共组件
├── static                               纯静态资源，不会被wabpack构建
├── .babelrc                             ES6语法编译配置
├── .editorconfig                        vs code 的相关配置
├── .eslintignore                        eslint检测忽略的文件配置
├── .eslint                              eslint规则配置，可以单独控制每一条检查规则的开启和关闭
├── .gitignore                           git忽略提交
├── .postcssrc.js                        css autoprefixer配置
├── fav.ico                              项目 icon 图标
├── index.html                           项目 icon 图标，还可以添加一些 meta 信息或统计代码啥的。
├── package.json                         项目配置文件
└── README.md	                            项目的说明文档，markdown 格式
```
