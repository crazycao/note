# EGG学习笔记

## 环境准备
操作系统：支持 macOS，Linux，Windows
运行环境：建议选择 Node.js 的 LTS 版本，最低要求 8.x。


## 项目初始化
直接使用脚手架，在终端执行以下命令：

```
$ mkdir egg-example && cd egg-example
$ npm init egg --type=simple
$ npm i
```

执行完 `npm init egg --type=simple` 以后，终端显示如下：

```
npx: 385 安装成功，用时 20.051 秒
[egg-init] use registry: https://registry.npmjs.org
[egg-init] target dir is /Users/caowenfeng/Documents/MyDemo/NodeDemo/egg-example
[egg-init] fetching npm info of egg-init-config
[egg-init] use boilerplate: simple(egg-boilerplate-simple)
[egg-init] fetching npm info of egg-boilerplate-simple
[egg-init] downloading https://registry.npmjs.org/egg-boilerplate-simple/-/egg-boilerplate-simple-3.3.1.tgz
[egg-init] extract to /var/folders/bb/byxxxbfs4_92g5vkx_2d_4nr0000gn/T/egg-init-boilerplate
[egg-init] collecting boilerplate config...
? project name (example) 
```

依次输入工程名称、描述、作者和Cookie安全字符串，就可以自动完成Egg工程的初始化。

```
? project name EggDemo
? project description Egg学习演示
? project author crazycao
? cookie security keys 1574322065322_2399
```

然后 `npm i` 会安装所依赖的 node 库。

> **骨架类型**
> 
> 初始化Egg项目时，可以选择使用骨架类型。
> 如 `$ npm init egg --type=simple` 中的 `--type`。
> 可选的骨架类型及说明如下：
> 
> - simple：简单 egg 应用程序骨架
> - empty：空的 egg 应用程序骨架
> - plugin：egg plugin 骨架
> - framework：egg framework 骨架
> 

## 目录结构
```
egg-project
├── package.json                         项目描述文件
├── app.js (可选)                         用于自定义启动时的初始化工作
├── agent.js (可选)
├── app                                  用于编写项目代码
|   ├── router.js                        用于配置 URL 路由规则
│   ├── controller                       用于解析用户的输入，处理后返回相应的结果
│   |   └── home.js
│   ├── service (可选)                    用于编写业务逻辑层
│   |   └── user.js
│   ├── model(可选)                       用于放置领域模型
│   |   └── user.js
│   ├── middleware (可选)                 用于编写中间件
│   |   └── response_time.js
│   ├── schedule (可选)                   用于定时任务
│   |   └── my_task.js
│   ├── public (可选)                     用于放置静态资源
│   |   └── reset.css
│   ├── view (可选)                       用于放置模板文件，模板渲染
│   |   └── home.tpl
│   └── extend (可选)                     用于框架的扩展
│       ├── helper.js (可选)
│       ├── request.js (可选)
│       ├── response.js (可选)
│       ├── context.js (可选)
│       ├── application.js (可选)
│       └── agent.js (可选)
├── config                               用于编写配置文件
|   ├── plugin.js                        用于配置需要加载的插件
|   ├── config.default.js
│   ├── config.prod.js
|   ├── config.test.js (可选)
|   ├── config.local.js (可选)
|   └── config.unittest.js (可选)
└── test                                 用于单元测试
    ├── middleware
    |   └── response_time.test.js
    └── controller
        └── home.test.js
```

## Egg-Sequelize 使用

在 Node.js 社区中，sequelize 是一个广泛使用的 ORM 框架，它支持 MySQL、PostgreSQL、SQLite 和 MSSQL 等多个数据源。

### 引入 Egg-Sequelize 

- 安装并配置 egg-sequelize 插件（它会辅助我们将定义好的 Model 对象加载到 app 和 ctx 上）和 mysql2 模块：

```
npm install --save egg-sequelize mysql2
```

- 在 config/plugin.js 中引入 egg-sequelize 插件

```
// 引入 egg-sequelize 插件
exports.sequelize = {
  enable: true,
  package: 'egg-sequelize',
};

// 注意需要把原来的 exports 删除
// /** @type Egg.EggPlugin */
// module.exports = {
//   // had enabled by egg
//   // static: {
//   //   enable: true,
//   // }
// }
```

- 在 config/config.default.js 中编写 sequelize 配置

```
  config.sequelize = {
    dialect: 'mysql', // 数据库类型，支持: mysql, mariadb, postgres, mssql
    host: '127.0.0.1', // 数据库所在服务器地址
    port: 3306, // 数据库服务端端口
    database: 'telecom_contacts', // 数据库名称
    username: 'root', // 数据库登录账号
    password: 'bwton123' // 数据库登录密码
  }
```

### 定义模型

首先，定义一个模型。默认情况下，所有的 Model 都放在 app/model/ 目录下，可以通过如下方式访问 Model 对象。

|model file|	class name|
|:--:|:--:|
|user.js|	app.model.User|
|person.js|	app.model.Person|
|user_group.js|	app.model.UserGroup|
|user/profile.js|app.model.User.Profile|

> 注意：options.delegate 默认是指向 model 的，所以 app.model 是一个 Sequelize 实例，因此你可以使用这些方法：app.model.sync，app.model.query ...

一个简单的 Model 如下所示：

```
// app/model/user.js

'use strict'

module.exports = app => {
  const { STRING, INTEGER, DATE } = app.Sequelize

  // app.model 实际是一个 Sequelize 的实例；而 sequelize.define 内部调用的是 Model.init 方法。初始化并返回了 Sequelize.Model 的扩展类 User 的实例
  const User = app.model.define(
    'user', // 表名
    {
      // 属性
      id: {
        type: INTEGER, // 类型，整型
        primaryKey: true, // 是否主键
        autoIncrement: true // 是否自增
      },
      accont: { type: STRING(32), allowNull: false },
      password: {
        type: STRING(64), // 类型，64位字符串
        allowNull: false, // 是否可空
        defaultValue: '123456' // 默认值
      },
      role: { type: INTEGER, allowNull: false, defaultValue: 1 },
      state: INTEGER, // 简单写法，只标明类型即可
      created_at: DATE,
      updated_at: DATE
    },
    {
      // 参数，可选
      tableName: 'user' // 指定表名，不然会框架自动查找的表名与controller相同
    })
    
  // 自定义模型方法
  User.findByLogin = async function(login) {
    return await this.findOne({
      where: {
        login: login
      }
    });
  }

  return User
}
```

### 访问模型

可以在 Controller 和 Service 中通过 app.model.User 或者 ctx.model.User 访问到上面定义的 Model。

```
// app/controller/user.js
class UserController extends Controller {
  async index() {
    const users = await this.ctx.model.User.findAll();
    this.ctx.body = users;
  }

  async show() {
    const user = await this.ctx.model.User.findByLogin(this.ctx.params.login);
    await user.logSignin();
    this.ctx.body = user;
  }
}
```
