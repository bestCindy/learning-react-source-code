`forwardRef` 是为了帮助我们在函数组件里面拿到 `ref`

如果一个函数是 pure component，那么在函数内部是没有 `this` 的，而 `ref` 不能通过 `props` 拿到（详见 02），所以我们需要一个媒介帮助我们拿到 `ref`，这个媒介就是 `forwardRef`

举个例子：
```
const TargetComponent = React.forwardRef((props, ref) => (
    <input type="text" ref={ref}></input>
))

render() {
    return <TargetComponent ref={this.ref}>
}
```

源码：
```
export default function forwardRef<Props, ElementType: React$ElementType>(
  render: (props: Props, ref: React$Ref<ElementType>) => React$Node,
) {
  return {
    $$typeof: REACT_FORWARD_REF_TYPE,
    render,
  };
}
```

注意虽然返回的对象中 `$$typeof` 的值是 `REACT_FORWARD_REF_TYPE`，但是编译出来的 elemnt 的 `$$typeof` 仍然是 `REACT_ELEMENT_TYPE`

return 的这个对象整体是挂到 element 的 `type` 上面的（详见 02）