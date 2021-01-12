### memo
memo 用来给 functional component 提供类似 pure component 的功能

```
export default function memo<Props>(type: React$ElementType, compare?: (oldProps: Props, newProps: Props) => boolean,
) {
  return {
    $$typeof: REACT_MEMO_TYPE,
    type,
    compare: compare === undefined ? null : compare,
  };
}
```
跟之前的 `forwardRef` `context` 差不多是一样的东西

### Fragment
在 babel 里面会认为 `<></>` 这样的空尖括号是 `React.Fragment`
React 要求多个兄弟节点外层要套一个 parent 才能返回，这个时候可以用一组 `<React.Fragment></React.Fragment>`

```
Fragment: REACT_FRAGMENT_TYPE
```

### StrictMode
它标识，它下面的子节点都要应用某一种规则来渲染

```
StrictMode: REACT_STRICT_MODE_TYPE,
```

### cloneElement
clone 一个 element

整体代码和 `ReactElement` 差不多的

```
export function cloneElement(element, config, children)
```
第一个参数是需要 clone 的 element

