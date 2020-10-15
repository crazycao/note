
# Taro面试题

## Taro 界面跳转时，如何传参？

- 界面跳转时在所有跳转的 url 后面添加查询字符串参数进行跳转传参
- 在跳转成功的目标页的生命周期方法里就能通过 getCurrentInstance().router.params 获取到传入的参数

## Taro 的尺寸单位是多少？如果设计稿的尺寸是 320，怎么办？

- Taro 默认以 750px 作为换算尺寸标准。
- 如果设计稿的尺寸是 320，可以修改项目配置 `config/index.js` 中的 designWidth 配置为 320，同时在 DEVICE_RATIO 中添加换算规则如下：

```
const config = {
  projectName: 'myProject',
  date: '2018-4-18',
  designWidth: 640,
  ....
}

const DEVICE_RATIO = {
  '640': 2.34 / 2,
  '750': 1,
  '828': 1.81 / 2,
  '320': 750 / 320
}
```

## Taro 中如何使用全局变量

- 使用 redux
- 新增一个自行命名的 JS 文件，例如 `global_data.js`，在里面定义全局变量和读写全局变量的方法，并在需要使用时导入该文件。

## Taro 如何分包

通过配置入口文件 app.config.js ，添加 `subpackages` 字段，在 `subpackages` 字段添加分包
