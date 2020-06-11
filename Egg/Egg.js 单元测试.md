# Egg.js 单元测试

官方推荐测试框架：[Mocha](https://mochajs.org)（[Mocha中文网](https://mochajs.cn/)）

官方推荐断言库：[power-assert](https://github.com/power-assert-js/power-assert)

Egg.js 中已经内置 `Mocha`、`co-mocha`、`power-assert`，`nyc` 等模块，只需要在 `package.json` 上配置好 `scripts.test` 即可。

```
{
  "scripts": {
    "test": "egg-bin test"
  }
}
```

运行测试：

```
npm test
```

> **注意：** `npm test` 会首先进行 eslint 检查，若有 `error` 则不会开始测试。

指定文件路径，可以对某一个单元测试文件进行测试：

```
npm test ./path/to/user.test.js
```

## 目录结构

约定 `test` 目录为存放所有测试脚本的目录。

测试脚本文件统一按 `${filename}.test.js` 命名，必须以 `.test.js` 作为文件后缀。


```
test
├── controller
│   └── home.test.js
├── hello.test.js
└── service
    └── user.test.js
```

## Controller 测试

`describe` 和 `it` 的第一个字段都只是描述，每一个 `it` 是一个测试用例。

```
'use strict';

const { app, assert } = require('egg-mock/bootstrap');

describe('test/app/controller/home.test.js', () => {
  it('should assert', () => {
    const pkg = require('../../../package.json');
    assert(app.config.keys.startsWith(pkg.name));

    // const ctx = app.mockContext({});
    // yield ctx.service.xx();
  });

  it('should GET /', () => {
    return app.httpRequest()
      .get('/')
      .expect(200) // 期望返回的 status 200
      .expect('hi, egg'); // 期望返回的 body，支持 string/
  });
});

describe('test/app/controller/user.test.js', () => {
  it('should POST /users', () => {
    return app.httpRequest()
      .post('/users')
      .send({ username: 'Mike', password: '123456' }) // post body
      .expect(200)
      .then(response => {
        // response.text 是返回的 body 字符串，转成 json 后再通过 assert 校验
        const res = JSON.parse(response.text);
        assert.equal(res.code, '0000'); // 业务错误码应该为'0000'，字符串对比
        assert(res.data.userId); // userId应该存在，注意 0、false、空字符串 都会被判定为失败，与 assert.ok() 等效
      });
  });
});
```

## 前置和后置步骤

Mocha 使用 `before/after/beforeEach/afterEach` 来处理前置后置任务，基本能处理所有问题。 每个用例会按 `before` -> `beforeEach` -> `it` -> `afterEach` -> `after` 的顺序执行，而且可以定义多个。

```
describe('egg test', () => {
  // Mocha 刚开始运行的时候会载入所有用例，这时调用 describe 方法，执行 before()
  before(() => console.log('order 1')); // 在当前 describe 中的所有用例之前执行
  before(() => console.log('order 2')); // 支持多个 before 函数
  after(() => console.log('order 6')); // 在当前 describe 中的所有用例之后执行
  beforeEach(() => console.log('order 3')); // 在当前 describe 中的每个用例之前执行
  afterEach(() => console.log('order 5')); // 在当前 describe 中的每个用例之后执行
  it('should worker', () => console.log('order 4'));
});
```

## Service 测试

也可以直接对 Service 层进行测试。

```
describe('get()', () => {
  it('should get exists user', async () => {
    // 创建 ctx
    const ctx = app.mockContext();
    // 通过 ctx 访问到 service.user
    const user = await ctx.service.user.get('admin');
    assert(user);
    assert(user.name === '管理员');
  });

  it('should get null when user not exists', async () => {
    const ctx = app.mockContext();
    const user = await ctx.service.user.get('admin');
    assert(!user);
  });
});
```

