首先简单看下用法：

```
const { Provider, Consumer } = React.createContext("default");
```
```
<Provider value={this.value.newContext}></Provider>
```
```
<Consumer>{value => <p>newContext: {value}</p>}</Consumer>
```

`createContext` 方法最后返回了一个 `context`

```
const context: ReactContext<T> = {
    $$typeof: REACT_CONTEXT_TYPE,
    _calculateChangedBits: calculateChangedBits,
    _currentValue: defaultValue,
    _currentValue2: defaultValue,
    Provider: (null: any),
    Consumer: (null: any),
};
```

注意它的 `$$typeof` 是 `REACT_CONTEXT_TYPE`，这个 `$$typeof` 也是挂到reactElement 的 `type` 上

```
context.Provider = {
    $$typeof: REACT_PROVIDER_TYPE,
    _context: context,
};
```
```
context.Consumer = context;
```