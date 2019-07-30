## 渲染阶段生命周期包括以下类组件方法：

- `constructor`
- `componentWillMount`
- `componentWillReceiveProps`
- `componentWillUpdate`
- `getDerivedStateFromProps`
- `shouldComponentUpdate`
- `render`
- `setState` 更新程序功能（第一个参数



## setState()

setState 只在**合成事件**和**钩子函数中**是“异步”的，在**原生事件**和 ***setTimeout*** 中都是同步的。

合成事件：就是react 在组件中的onClick等都是属于它自定义的合成事件

原生事件：比如通过addeventListener添加的，dom中的原生事件

## React 生命周期

### componentDidMount()

```typescript
componentDidMount()
```

> 在安装组件（插入树中）后立即调用,需要DOM节点的初始化应该放在这里。如果需要从远程端点加载数据



### componentDidUpdate()

```typescript
componentDidUpdate(prevProps, prevState, snapshot[快照])
```

> 更新发生后立即调用。初始渲染不会调用此方法。

```typescript
componentDidUpdate(prevProps) {
  // Typical usage (don't forget to compare props):
  if (this.props.userID !== prevProps.userID) {
    this.fetchData(this.props.userID);
  }
}
```



### componentWillUnmount()

```typescript
componentWillUnmount()
```

> 在卸载和销毁组件之前立即调用
>
> 在此方法中执行任何必要的清理，例如使计时器无效
>
> setState() 调用也无效



### shouldComponentUpdate()

```type
shouldComponentUpdate(nextProps, nextState)
```

> 如果一个组件的输出不收到 `state` 或 `props` 变化
>
> 默认行为是在每次状态更改时重新呈现

Use `shouldComponentUpdate()` to let React know if a component’s output is not affected by the current change in state or props. The default behavior is to re-render on every state change, and in the vast majority of cases you should rely on the default behavior.

`shouldComponentUpdate()` is invoked before rendering when new props or state are being received. Defaults to `true`. This method is not called for the initial render or when `forceUpdate()` is used.

This method only exists as a **performance optimization.** Do not rely on it to “prevent” a rendering, as this can lead to bugs. **Consider using the built-in PureComponent** instead of writing `shouldComponentUpdate()` by hand. `PureComponent` performs a shallow comparison of props and state, and reduces the chance that you’ll skip a necessary update.

If you are confident you want to write it by hand, you may compare `this.props` with `nextProps` and `this.state` with `nextState` and return `false` to tell React the update can be skipped. Note that returning `false` does not prevent child components from re-rendering when *their* state changes.

We do not recommend doing deep equality checks or using `JSON.stringify()` in `shouldComponentUpdate()`. It is very inefficient and will harm performance.

Currently, if `shouldComponentUpdate()` returns `false`, then [`UNSAFE_componentWillUpdate()`](https://reactjs.org/docs/react-component.html#unsafe_componentwillupdate), [`render()`](https://reactjs.org/docs/react-component.html#render), and [`componentDidUpdate()`](https://reactjs.org/docs/react-component.html#componentdidupdate) will not be invoked. In the future React may treat `shouldComponentUpdate()` as a hint rather than a strict directive, and returning `false` may still result in a re-rendering of the component.

```type
https://reactjs.org/docs/react-component.html#componentdidmount
```



### static getDerivedStateFromProps()

```typescript
static getDerivedStateFromProps(props, state)
```

> 在调用render方法之前调用，无论是在初始安装还是后续更新。
>
> 它应返回一个对象来更新状态，或者返回null以不更新任何内容。

确保你熟悉更简单的选择:

- 如果您需要执行副作用(例如，数据获取或动画)来响应道具中的更改，则使用componentDidUpdate生命周期。
- 如果只希望在某个道具发生更改时重新计算某些数据， [use a memoization helper instead](https://reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html#what-about-memoization).
- 如果您想在道具更改时“重置”某些状态，请考虑使用键完全控制或完全不控制组件。



### getSnapshotBeforeUpdate()

```typescript
getSnapshotBeforeUpdate(prevProps, prevState)
```

> 在最近呈现的输出被提交到例如DOM之前调用
>
> 它使您的组件可以在可能更改之前从DOM捕获一些信息（例如滚动位置）
>
> 此生命周期返回的任何值都将作为参数传递给`componentDidUpdate()`

在上面的例子中，重要的是读取`scrollHeight`属性，`getSnapshotBeforeUpdate`因为“渲染”阶段生命周期（如`render`）和“提交”阶段生命周期（如`getSnapshotBeforeUpdate`和`componentDidUpdate`）之间可能存在延迟。

```typescript
class ScrollingList extends React.Component {
  constructor(props) {
    super(props);
    this.listRef = React.createRef();
  }

  getSnapshotBeforeUpdate(prevProps, prevState) {
    // Are we adding new items to the list?
    // Capture the scroll position so we can adjust scroll later.
    if (prevProps.list.length < this.props.list.length) {
      const list = this.listRef.current;
      return list.scrollHeight - list.scrollTop;
    }
    return null;
  }

  componentDidUpdate(prevProps, prevState, snapshot) {
    // If we have a snapshot value, we've just added new items.
    // Adjust scroll so these new items don't push the old ones out of view.
    // (snapshot here is the value returned from getSnapshotBeforeUpdate)
    if (snapshot !== null) {
      const list = this.listRef.current;
      list.scrollTop = list.scrollHeight - snapshot;
    }
  }

  render() {
    return (
      <div ref={this.listRef}>{/* ...contents... */}</div>
    );
  }
}
```



### Error boundaries

是React组件，它们在其子组件树中的任何位置捕获JavaScript错误，记录这些错误，并显示回退UI而不是崩溃的组件树。错误边界在渲染期间，生命周期方法以及它们下面的整个树的构造函数中捕获错误。

如果它的生命周期方法限定任一个（或两者）的类分量变成错误边界`static getDerivedStateFromError()`或`componentDidCatch()`。从这些生命周期更新状态可让您在下面的树中捕获未处理的JavaScript错误并显示回退UI。

[*Error Handling in React 16*](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html).

```typescript
# NOte

Error boundaries only catch errors in the components below them in the tree. An error boundary can’t catch an error within itself.

错误边界仅捕获树中它们下面的组件中的错误。错误边界本身无法捕获错误。
```



### static getDerivedStateFromError()

```typescript
static getDerivedStateFromError(error)
```

> 在后代组件抛出错误后调用此生命周期。它接收作为参数抛出的错误，并应返回值以更新状态。

```typescript
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // Update state so the next render will show the fallback UI.
    return { hasError: true };
  }

  render() {
    if (this.state.hasError) {
      // You can render any custom fallback UI
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children; 
  }
}
```

```typescript
# Note
getDerivedStateFromError() is called during the “render” phase, so side-effects are not permitted. For those use cases, use componentDidCatch() instead.

getDerivedStateFromError()在“渲染”阶段调用，因此不允许副作用。对于这些用例，请componentDidCatch()改用。
```



### componentDidCatch()

```typescript
componentDidCatch(error, info)
```

在后代组件抛出错误后调用此生命周期。它接收两个参数：

1. `error` - 抛出的错误。
2. `info`- 带有`componentStack`键的对象，其中包含[有关哪个组件引发错误的信息](https://reactjs.org/docs/error-boundaries.html#component-stack-traces)。

> `componentDidCatch()`在“提交”阶段被调用，因此允许副作用。它应该用于记录错误之类的事情：

```typescript
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // Update state so the next render will show the fallback UI.
    return { hasError: true };
  }

  componentDidCatch(error, info) {
    // Example "componentStack":
    //   in ComponentThatThrows (created by App)
    //   in ErrorBoundary (created by App)
    //   in div (created by App)
    //   in App
    logComponentStackToMyService(info.componentStack);
  }

  render() {
    if (this.state.hasError) {
      // You can render any custom fallback UI
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children; 
  }
}
```

```typescript
# Note

如果发生错误，您可以componentDidCatch()通过调用呈现回退UI setState，但在将来的版本中将不推荐使用。使用static getDerivedStateFromError()处理回退，而不是渲染。
```



### forceUpdate()

```typescript
component.forceUpdate(callback)
```

> 默认情况下，当组件的状态或道具发生更改时，组件将重新呈现。
>
> 如果`render()`方法依赖于其他一些数据，可以告诉React该组件需要通过调用重新呈现`forceUpdate()`。

调用`forceUpdate()`将导致`render()`在组件上调用，跳过`shouldComponentUpdate()`。

这将触发子组件的正常生命周期方法，包括`shouldComponentUpdate()`每个子组件的方法。

如果标记更改，React仍将仅更新DOM。

通常你应该尽量避免使用`forceUpdate()`和只读取`this.props`和`this.state`使用`render()`。





# Ref 的用法

### 1. 回调函数

> 回调函数就是在dom节点或组件上挂载函数，函数的入参是dom节点或组件实例，达到的效果与字符串形式是一样的

### 2. React.createRef() （React16.3提供）



#### 回调函数的触发时机：

1. 组件渲染后，即componentDidMount后
2. 组件卸载后，即componentWillMount后，此时，入参为null
3. ref改变后



##### dom节点上使用回调函数

```typescript
export default class ClassExample extends React.Component {
  private textInput: any;
  constructor() {
    super();
    this.state = {
      value: '新OA开发',
    };
    this.onSubmit = this.onSubmit.bind(this);
  }

  public onSubmit() {
    const value = this.textInput.value;
    alert(value);
  }

  public render() {
    return (
      <div>
        书名
        <input type="text" ref={(input) => (this.textInput = input)} />
        <button onClick={this.onSubmit}>提交</button>
      </div>
    );
  }
}
```

# [Strict Mode](<https://reactjs.org/docs/strict-mode.html#detecting-legacy-context-api>)  React16.9

```typescript
import React from 'react';

function ExampleApplication() {
  return (
    <div>
      <Header />
      <React.StrictMode>
        <div>
          <ComponentOne />
          <ComponentTwo />
        </div>
      </React.StrictMode>
      <Footer />
    </div>
  );
}
```

在上面的示例中，*不会*对`Header`和`Footer`组件运行严格模式检查。然而，`ComponentOne`和`ComponentTwo`，以及所有他们的后代，将有检查。

##### `StrictMode` 目前帮助：

- [识别具有不安全生命周期的组件](https://reactjs.org/docs/strict-mode.html#identifying-unsafe-lifecycles)
- [关于遗留字符串ref API使用的警告](https://reactjs.org/docs/strict-mode.html#warning-about-legacy-string-ref-api-usage)
- [关于已弃用的findDOMNode用法的警告](https://reactjs.org/docs/strict-mode.html#warning-about-deprecated-finddomnode-usage)
- [检测意外的副作用](https://reactjs.org/docs/strict-mode.html#detecting-unexpected-side-effects)
- [检测遗留上下文API](https://reactjs.org/docs/strict-mode.html#detecting-legacy-context-api)

将来的React版本将添加其他功能。



swMobile

realease_backup

CD102462  密码111111