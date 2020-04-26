# Umi插件

每个插件都会对应一个 id 和一个 key，**id 是路径的简写**，**key 是进一步简化**后用于配置的唯一值。

比如插件 `/node_modules/@umijs/plugin-foo/index.js`，通常来说，其 id 为 `@umijs/plugin-foo`，key 为 `foo`。

## 启用插件

插件有多种启用方式。

### package.json 依赖

Umi 会自动检测 `dependencies` 和 `devDependencies` 里的 umi 插件。检测到的插件会自动被注册，无需在配置里重复声明。如:

```
{
  "dependencies": {
    "@umijs/preset-react": "1"
  }
}
```

### 配置

在配置里可通过 `presets` 和 `plugins` 配置插件，比如：

```
export default {
  presets: ['./preset', 'foo/presets'],
  plugins: ['./plugin'],
}
```

通常用于几种情况：

- 项目相对路径的插件
- 非 npm 包入口文件的插件

> 注意：请不要配置 npm 包的依赖，否则会报重复注册的错误。

### 环境变量

还可通过环境变量 `UMI\_PRESETS` 和 `UMI\_PLUGINS` 注册额外插件。

比如：

```
$ UMI_PRESETS=/a/b/preset.js umi dev
```

> 注意：项目里不建议使用，通常用于基于 umi 的框架二次封装。


## 禁用插件

有两种方式可禁用插件。

### 配置 key 为 false

比如：

```
export default {
  mock: false,
}
```

会禁用 Umi 内置的 mock 插件及其功能。


### 在插件里禁用其他插件

可通过 ```api.skipPlugins(pluginId[])``` 的方式禁用。