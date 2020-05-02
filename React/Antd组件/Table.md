# Table 表格

展示行列数据。

## 何时使用

- 当有大量结构化的数据需要展现时；
- 当需要对数据进行排序、搜索、分页、自定义操作等复杂行为时。

## 如何使用

指定表格的数据源 `dataSource` 和 表头 `columns`，传入组件即可。

```
const dataSource = [
  {
    key: '1',
    name: '胡彦斌',
    age: 32,
    address: '西湖区湖底公园1号',
  },
  {
    key: '2',
    name: '胡彦祖',
    age: 42,
    address: '西湖区湖底公园1号',
  },
];

const columns = [
  {
    title: '姓名',
    dataIndex: 'name',
    key: 'name',
  },
  {
    title: '年龄',
    dataIndex: 'age',
    key: 'age',
  },
  {
    title: '住址',
    dataIndex: 'address',
    key: 'address',
  },
];

<Table dataSource={dataSource} columns={columns} />;
```

## dataSource

数据源。对象数组。

数组里的每一个对象是一条数据。

列表会将对象各字段填入到表格相应的位置。

按照 `React` 的规范，所有的数组组件必须绑定 `key`。在 Table 中，`dataSource` 和 `columns` 里的数据值都需要指定 `key` 值。

对于 `dataSource` 默认将每列数据的 key 属性作为唯一的标识。

## columns

表格列的配置描述。对象数组。

数组里的每一个对象是对表格的一列的描述。

通过 `dataIndex` 的值找到数据源相应的字段值。

## scroll

支持表格滚动。

```
const scroll = {
    x: 'max-content',
    y: 400,
    scrollToFirstRowOnChange: true
}

<Table dataSource={dataSource} columns={columns} scroll={scroll}/>;
```

### x

设置横向滚动，也可用于指定滚动区域的宽，可以设置为像素值，百分比，true 和 'max-content'。

### y

number 类型。设置纵向滚动，也可用于指定滚动区域的高，可以设置为像素值。

### scrollToFirstRowOnChange

boolean 类型。当分页、排序、筛选变化后是否滚动到表格顶部。
