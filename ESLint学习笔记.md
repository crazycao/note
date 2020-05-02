# ESLint学习笔记

## 环境准备
操作系统：支持 macOS，Linux，Windows

运行环境：Node >= 6.14 和 npm >= 3

## 安装 ESLint
使用 npm 安装 ESLint

```
npm install eslint --save-dev
```
然后要创建一个配置文件

```
./node_modules/.bin/eslint --init
```
脚本会让你做出选择

```
How would you like to use ESLint? (Use arrow keys)
  To check syntax only 
❯ To check syntax and find problems 
  To check syntax, find problems, and enforce code style
```
```
How would you like to use ESLint? (Use arrow keys)
What type of modules does your project use? (Use arrow keys)
❯ JavaScript modules (import/export) 
  CommonJS (require/exports) 
  None of these 
```
```
Which framework does your project use? (Use arrow keys)
❯ React 
  Vue.js 
  None of these 
```
```
Where does your code run? (Press <space> to select, <a> to toggle all, <i> to invert selection)
❯◉ Browser
 ◯ Node
```
```
How would you like to define a style for your project? (Use arrow keys)
❯ Use a popular style guide 
  Answer questions about your style 
  Inspect your JavaScript file(s) 
```
```
Which style guide do you want to follow? (Use arrow keys)
❯ Airbnb (https://github.com/airbnb/javascript) 
  Standard (https://github.com/standard/standard) 
  Google (https://github.com/google/eslint-config-google) 
```
```
 What format do you want your config file to be in? (Use arrow keys)
❯ JavaScript 
  YAML 
  JSON
```

于是就会在当前文件夹生成一个 `.eslintrc` 文件。

## `.eslintrc` 文件


```
{
    "parserOptions": { /* 解析器选项 */
        "ecmaVersion": 6, /* 使用的 ECMAScript 版本 */
        "sourceType": "module", /* 设置为 "script" (默认) 或 "module"（如果你的代码是 ECMAScript 模块) */
        "ecmaFeatures": { /* 额外的语言特性 */
            "globalReturn": true, /* 允许在全局作用域下使用 return 语句 */
            "impliedStrict": true, /* 启用全局 strict mode */
            "jsx": true /* 启用 JSX */
        }
    },
    "parser": "esprima", /* 指定解析器 */
    "plugins": [
        "eslint-plugin-plugin1", /* 使用插件 */
        "plugin2" /* 插件名称可以省略 eslint-plugin- 前缀 */
    ], 
    "processor": "a-plugin/a-processor", /* 启用插件 a-plugin 提供的处理器 a-processor。 */
    /* 处理器可以从另一种文件中提取 JavaScript 代码，然后让 ESLint 检测 JavaScript 代码。或者处理器可以在预处理中转换 JavaScript 代码。 */
    "extends": [ /* 继承规则 */
        "eslint:recommended", /* 若使用 standard 规则，则写为 extends: standard */
        "plugin:react/recommended"  /* 继承插件中的规则，react 是包名 */
    ],
    "rules": { /* 改变规则设置 */
        "semi": "off", /* "off" 或 0 - 关闭规则 */
        "curly": "warn", /* "warn" 或 1 - 开启规则，使用警告级别的错误：warn (不会导致程序退出) */
        "quotes": "error" /* 开启规则，使用错误级别的错误：error (当被触发的时候，程序会退出) */
    },
    "overrides": [
        {
            "files": ["*.md"],
            "processor": "a-plugin/markdown" /* 对指定类型的文件使用处理器 */
        },
        {
            "files": ["**/*.md/*.js"],
            "rules": { /* 对指定类型的文件应用规则 */
                "strict": "off"
            }
        }
    ]
}
```

## `.eslintignore` 文件

在项目根目录创建一个 `.eslintignore` 文件告诉 ESLint 去忽略特定的文件和目录。

`.eslintignore` 文件是一个纯文本文件，其中的每一行都是一个 glob 模式表明哪些路径应该忽略检测。

例如，以下将忽略所有的 JavaScript 文件：

```
**/*.js
```

支持忽略某一路径下的文件

```
# 忽略 src 目录下的 js 文件
/root/src/*.js
# 但是 app.js 不能被忽略
!/root/src/app.js
```
- 以 # 开头的行被当作注释，不影响忽略模式。
- 路径是相对于 .eslintignore 的位置或当前工作目录。
- 以 ! 开头的行是否定模式，它将会重新包含一个之前被忽略的模式。