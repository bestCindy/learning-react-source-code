在 React 里面用到的一些属性和方法都是在 React.js 里面暴露出来的

其中包括 createElement

相当于找方法源码的一个入口

`createElement` 方法在 ReactElement.js 里面

`createElement` 有三个参数分别是 `type，config，children`
- `type`: 创建的组件的标签
- `config`: 组件上的 attribute
- `children`: 组件的子元素

## 以下内容基于方法 `createElement`

### （一）处理 `config` 

首先要判断下是否是内键的 props

```
for (propName in config) {
    if (hasOwnProperty.call(config, propName) && !RESERVED_PROPS.hasOwnProperty(propName)) {
        props[propName] = config[propName];
    }
}
```
```
const RESERVED_PROPS = {
  key: true,
  ref: true,
  __self: true,
  __source: true,
}
```
如果是 `RESERVED_PROPS` 里面那几个是不会出现在 `this.props` 里面的，因为根本没有放进去

### （二）处理 `children`：

```
const childrenLength = arguments.length - 2;
if (childrenLength === 1) {
    props.children = children;
} else if (childrenLength > 1) {
    const childArray = Array(childrenLength);
    for (let i = 0; i < childrenLength; i++) {
        childArray[i] = arguments[i + 2];
    }
    props.children = childArray;
}
```
把 `children` 挂到 `props` 上面，处理的时候分两种情况，一个 `children` 和多个 `children` 的时候

### （三）给 `props` 添加默认值

```
if (type && type.defaultProps) {
    const defaultProps = type.defaultProps;
    for (propName in defaultProps) {
        if (props[propName] === undefined) {
            props[propName] = defaultProps[propName];
        }
    }
}
```

### （四）`defaultProps` 用法：

```
class Comp extends React.Component {...}
Comp.defaultProps = { value: "test" }
```

### （五）返回值

```
return ReactElement(
    type,
    key,
    ref,
    self,
    source,
    ReactCurrentOwner.current,
    props,
);
```
返回了一个 ReactElement

## 以下内容基于方法 `ReactElement`

```
const ReactElement = function (type, key, ref, self, source, owner, props) {
    const element = {
        // This tag allows us to uniquely identify this as a React Element
        $$typeof: REACT_ELEMENT_TYPE,

        // Built-in properties that belong on the element
        type: type,
        key: key,
        ref: ref,
        props: props,

        // Record the component responsible for creating this element.
        _owner: owner,
    };

    return element;
};
```
- 我们在写 jsx 的时候，所有的节点都是通过 `createElement` 创建的，所以它的 `$$typeof` 都是 `REACT_ELEMENT_TYPE`



