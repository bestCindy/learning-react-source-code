用法：

lazy.js
```
import React from 'react'

export default () => <p>Lazy Comp</p>

```

suspense.js
```
import React, { Suspense, lazy } from 'react'

const LazyComp = lazy(() => import('./lazy.js'));//异步加载

function SuspenseComp() {
  const data = requestData()

  return <p>{data}</p>
}

export default () => (
  <Suspense fallback="loading data">
    <SuspenseComp />
    <LazyComp />
  </Suspense>
)
```
如果在 `Suspense` 组件内部有多个组件，它会等所有的组件都 resolve，才会把 `fallback` 去掉，然后显示出里面的内容

源码：
```
Suspense: REACT_SUSPENSE_TYPE
```
`Suspense` 是一个 Symbol

ReactLazy
```
export function lazy<T, R>(ctor: () => Thenable<T, R>): LazyComponent<T> {
  return {
    $$typeof: REACT_LAZY_TYPE,
    _ctor: ctor,
    // React uses these fields to store the result.
    _status: -1,
    _result: null,
  };
}
```
- 接收的参数 `ctor` 是一个函数，返回值是一个 Thenable 对象，一般表示一个 Promise
- `_status`：标识 Promise 的状态
- `_result`：用来记录 resolve 之后返回的组件