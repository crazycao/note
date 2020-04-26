# Umi样式和资源

## 全局样式

Umi 中约定 `src/global.css` 为全局样式，如果存在此文件，会被自动引入到入口文件最前面。

比如用于覆盖样式，

```
.ant-select-selection {
  max-height: 51px;
  overflow: auto;
}
```

## CSS Modules

Umi 会自动识别 CSS Modules 的使用，你把他当做 CSS Modules 用时才是 CSS Modules。

比如：

```
// CSS Modules
import styles from './foo.css';

// 非 CSS Modules
import './foo.css';
```


## CSS 预处理器

Umi 内置支持 `less`，上述例子中，直接把 `.css` 后缀 `.less` 同样适用。

Umi 不支持 `sass` 和 `stylus`，但如果有需求，可以通过 `chainWebpack` 配置或者 umi 插件的形式支持。


## 使用图片

**JS 里使通过 `require` 引用相对路径的图片。**

比如：

```
export default () => <img src={require('./foo.png')} />
```

支持别名，比如通过 @ 指向 src 目录：

```
export default () => <img src={require('@/foo.png')} />
```

**CSS 里直接通过相对路径引用。**

比如，

```
.logo {
  background: url(./foo.png);
}
```

CSS 里也支持别名，但需要在前面加 ~ 前缀，

```
.logo {
  background: url(~@/foo.png);
}
```

> 注意：
> 
> 这是 `webpack` 的规则，如果切到其他打包工具，可能会有变化。
> 
> 以上规则在`less` 中同样适用。