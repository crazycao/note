# Dva学习笔记

**dva** 首先是一个基于 `redux` 和 `redux-saga` 的数据流方案。

然后为了简化开发体验，**dva** 还额外内置了 `react-router` 和 `fetch`，所以也可以理解为一个轻量级的应用框架。

## 快速上手

### 1. 安装 `dva-cli`

```
npm install dva-cli -g
```
###2. 创建新应用

```
dva new dva-quickstart
```
> 貌似这个项目已经转向 umi 了，安装时提示：
> 
> _dva-cli is deprecated, please use create-umi instead, checkout https://umijs.org/guide/create-umi-app.html for detail._
	
### 3. 启动服务
	
```
cd dva-quickstart
npm start
```

构建应用的命令是：

```
npm run build
```
	
### 4. 使用 `antd`

安装 `antd` 和 `babel-plugin-import`。`babel-plugin-import` 是用来按需加载 `antd` 的脚本和样式的。

```
npm install antd babel-plugin-import --save
```
编辑 `.webpackrc`，使 `babel-plugin-import` 插件生效。

```
{
+  "extraBabelPlugins": [
+    ["import", { "libraryName": "antd", "libraryDirectory": "es", "style": "css" }]
+  ]
}
```

### 5. 定义路由

路由可以想象成是组成应用的不同页面。
	
新建路由组件 `routes/Products.js`，内容如下：
	
```
import React from 'react';

const Products = (props) => (
  <h2>List of Products</h2>
);

export default Products;
```	

添加路由信息到路由表，编辑 `router.js` :

```
+ import Products from './routes/Products';
...
+ <Route path="/products" exact component={Products} />
```

如此就可以在浏览器访问 `http://localhost:8000/#/products` 了。

### 6. 编写 UI Component

新建 UI 组件 `components/ProductList.js` 文件：

```
import React from 'react';
import PropTypes from 'prop-types';
import { Table, Popconfirm, Button } from 'antd';

const ProductList = ({ onDelete, products }) => {
  const columns = [{
    title: 'Name',
    dataIndex: 'name',
  }, {
    title: 'Actions',
    render: (text, record) => {
      return (
        <Popconfirm title="Delete?" onConfirm={() => onDelete(record.id)}>
          <Button>Delete</Button>
        </Popconfirm>
      );
    },
  }];
  return (
    <Table
      dataSource={products}
      columns={columns}
    />
  );
};

ProductList.propTypes = {
  onDelete: PropTypes.func.isRequired,
  products: PropTypes.array.isRequired,
};

export default ProductList;
```

### 7. 定义 Model

**dva** 通过 `model` 的概念把一个领域的模型管理起来，包含同步更新 `state` 的 `reducers`，处理异步逻辑的 `effects`，订阅数据源的 `subscriptions` 。

新建 model `models/products.js` ：

```
export default {
  namespace: 'products',
  state: [],
  reducers: {
    'delete'(state, { payload: id }) {
      return state.filter(item => item.id !== id);
    },
  },
};
```

这个 model 里：

- `namespace` 表示在全局 `state` 上的 `key`
- `state` 是初始值，在这里是空数组
- `reducers` 等同于 `redux` 里的 `reducer`，接收 `action`，同步更新 `state`

然后在 index.js 里载入 model：

```
+ app.model(require('./models/products').default);
```

### 8. 将 model 和 component 串联起来 —— connect

**dva** 提供了 `connect` 方法。如果你熟悉 `redux`，这个 `connect` 就是 `react-redux` 的 `connect` 。

编辑 `routes/Products.js`，替换为以下内容：

```
import React from 'react';
import { connect } from 'dva';
import ProductList from '../components/ProductList';

const Products = ({ dispatch, products }) => {
  function handleDelete(id) {
    dispatch({
      type: 'products/delete',
      payload: id,
    });
  }
  return (
    <div>
      <h2>List of Products</h2>
      <ProductList onDelete={handleDelete} products={products} />
    </div>
  );
};

// export default Products;
export default connect(({ products }) => ({
  products,
}))(Products);
```

最后，我们还需要一些初始数据让这个应用 run 起来。编辑 `index.js`：

```
- const app = dva();
+ const app = dva({
+   initialState: {
+     products: [
+       { name: 'dva', id: 1 },
+       { name: 'antd', id: 2 },
+     ],
+   },
+ });
```