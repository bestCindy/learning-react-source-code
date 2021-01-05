component 分两种：
- `Component` 
- `PureComponent`：可以保证在 `props` 没有变化的情况减少不必要的更新

在 ReactBaseClasses.js 里面

### `setState`

```
Component.prototype.setState = function (partialState, callback) {
    invariant(
        typeof partialState === 'object' ||
        typeof partialState === 'function' ||
        partialState == null,
        'setState(...): takes an object of state variables to update or a ' +
        'function which returns an object of state variables.',
    );
    this.updater.enqueueSetState(this, partialState, callback, 'setState');
};
```
重点在第二个语句，可以看到当我们调用 `setState` 的时候主要就是调用了 `updater` 的 `enqueueSetState` 方法

关于 `updater`，在 `Component` 初始化的时候初始化

```
function Component(props, context, updater) {
  this.props = props;
  this.context = context;
  // If a component has string refs, we will assign a different object later.
  this.refs = emptyObject;
  // We initialize the default updater but the real one gets injected by the
  // renderer.
  this.updater = updater || ReactNoopUpdateQueue;
}
```

### `forceUpdate`

```
Component.prototype.forceUpdate = function(callback) {
  this.updater.enqueueForceUpdate(this, callback, 'forceUpdate');
};
```

### `PureComponent`

```
function ComponentDummy() {}
ComponentDummy.prototype = Component.prototype;

function PureComponent(props, context, updater) {
  this.props = props;
  this.context = context;
  // If a component has string refs, we will assign a different object later.
  this.refs = emptyObject;
  this.updater = updater || ReactNoopUpdateQueue;
}

const pureComponentPrototype = (PureComponent.prototype = new ComponentDummy());
pureComponentPrototype.constructor = PureComponent;


Object.assign(pureComponentPrototype, Component.prototype);
pureComponentPrototype.isPureReactComponent = true;
```
可以看出 `PureComponent` 是继承 `Component` 的，代码一开始就是一个继承的过程

然后用一个 `isPureReactComponent` 标识是 `PureComponent`