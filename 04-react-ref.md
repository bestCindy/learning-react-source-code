通过 `ref` 这个 attribute，我们能在组件内部获取一个子节点的实例

`ref` 的使用方式有三种
- stringRef
    - `<p ref="stringRef">span1</p>`
    - `this.refs.stringRef.textContext = "string ref got"`
- function
    - `<p ref={ele => (this.methodRef = ele)}>span2</p>`
    - `this.methodRef.textContext = "method ref got"`
- createRef
    - `this.objRef = React.createRef()`
    - 实际上是创建了一个 {current: null}
    - `<p ref={this.objRef}>span3</p>`
    - 组件渲染完成之后会把组件对应的实例挂载到 `current` 上
    - `this.objRef.current.textContext = "obj ref got"`

createRef：
```
export function createRef(): RefObject {
  const refObject = {
    current: null,
  };
  return refObject;
}
```
源码很简单