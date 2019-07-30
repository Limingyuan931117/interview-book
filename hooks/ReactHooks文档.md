# React Hooks

它们允许您在不编写**Class**的情况下使用状态和其他React功能。



### useState 

```jsx
const [state, setState] = useState(initialState);
```

返回一个数组 

state 是当前当前的状态值

setState 是可以改变状态值的方法函数

initialState 是默认值

```jsx
import React, { useState } from 'react';

export default function FunctionHooks() {
  const [count, setCount] = useState(0);

  const handleChange = () => {
    setCount(count + 1);
  };

  return (
    <div>
      <p>点击{count}次数</p>
      <button onClick={handleChange}>点击</button>
    </div>
  );
}

```



### useEffect 副作用

```jsx
useEffect(() => {} , [])
```

我们已经可以在 React 组件中进行数据请求 或者是手动更改 DOM。这些操作都是称为 `side effects`，也就是副作用，因为它们会影响其他组件，并且在渲染过程中没有办法完成。

`useEffect` 增加了从功能组件执行副作用的功能。

它与 React 类的 `componentDidMount`、`componentDidUpdate` 和 `componentWillUnmount` 具有相同的效果.

转换规则

* componentDidMount => useEffect(() => {} , [])
* componentDidUpdate =>  useEffect(() => {}, [count（返回的状态）])  或  useEffect(() => {})
* componentWillUnmount  => useEffect(() => {  return => () =  {  }  }) 

```jsx
import React, { useState, useEffect } from 'react';

export default function FunctionHooks() {
  const [count, setCount] = useState(0);

  const handleChange = () => {
    setCount(count + 1);
  };

  useEffect(() => {
    document.title = `点击${count}次数`;
    console.log(`点击${count}次数`);
  });

  return (
    <div>
      <p>点击{count}次数</p>
      <button onClick={handleChange}>点击</button>
    </div>
  );
}

```



### useContext

```jsx
const value = useContext(MyContext);
```

接受上下文对象（从中`React.createContext`返回的值）并返回该上下文的当前上下文值。

当上方最近更新时，就会重新呈现，并将最新上下文传给MyContext

> 注意 `useContext` 必须是*上下文对象本身*的参数：
>
> - **正确：** `useContext(MyContext)`
> - **不正确：** `useContext(MyContext.Consumer)`
> - **不正确：** `useContext(MyContext.Provider)`

```jsx
import React, { useContext } from 'react';

const ThemeContext = React.createContext({
  color: 'red',
  background: 'black',
  height: '200px',
});

const LocalContext = React.createContext('链接上下文,1234');

export default function FunctionHooks() {
  const theme = useContext(ThemeContext);
  const context = useContext(LocalContext);

  return <div style={theme}>{context}</div>;
}

```





### useRef

```jsx
const refContainer = useRef(initialValue);
```

`useRef`返回一个可变的ref对象，其  **.current`属性初始化为传递的参数（`initialValue`）**,返回的对象将持续整个组件的生命周期,

您可能熟悉refs主要是作为访问DOM一种方式。如果将ref对象传递给React `<div ref={myRef} />`，则`.current`只要该节点发生更改，React就会将其属性设置为相应的DOM节点。

```jsx
import React, { useRef } from 'react';

export default function FunctionHooks() {
  const textRef = useRef<HTMLInputElement>(null);

  const handleClick = () => {
    const value = textRef.current.value;
    alert(value);
  };

  return (
    <div>
      书名
      <input type="text" ref={textRef} />
      <button onClick={handleClick}>提交</button>
    </div>
  );
}

```

### useCallBack 

并返回的一个缓存版本的回调函数

```jsx
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b],
);
```

1. 接受函数和一个数组输入，并返回的一个缓存版本的回调函数，仅当重新渲染时数组中的值发生改变时，才会返回新的函数实例

2. 当将回调传递给依赖于引用相等性的优化子组件以防止不必要的渲染（例如`shouldComponentUpdate`）

### useMemo

返回的是一个缓存的值

`useMemo` 语法与 `useEffect` 是一致的。

第一个参数是需要执行的逻辑函数，

第二个参数是这个逻辑依赖输入变量组成的数组，如果不传第二个参数，这 `useMemo` 的逻辑每次就会运行，`useMemo` 本身的意义就不存在了，**所以需要传入参数**。

所以传入**空数组**就只会运行一次，策略与 `useEffect` 是一样的，但有一点比较大的差异就是调用时机，`useEffect` 执行的是副作用，所以一定是渲染之后才执行，但 `useMemo` 是需要返回值的，而返回值可以直接参与渲染，因此 `useMemo` 是在**渲染期间**完成的




```jsx
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

`useMemo`在渲染过程中传递的函数会运行

> Pass a “create” function and an array of dependencies. `useMemo` will only recompute the memoized value when one of the dependencies has changed. This optimization helps to avoid expensive calculations on every render.



### 自定义hook

构建自己的Hook。可以将**组件逻辑**提取到**可重用**的函数中。

**自定义Hook是一个JavaScript函数，其名称以“ use” 开头，可以调用其他Hook。**

例1： 朋友是否在线还是离线

```typescript
import React, { useState, useEffect } from 'react';

function FriendStatus(props) {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
      # 类似于 componentWillUnmount
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```

例2：

```typescript
import React, { useState, useEffect } from 'react';

function FriendListItem(props) {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  return (
    <li style={{ color: isOnline ? 'green' : 'black' }}>
      {props.friend.name}
    </li>
  );
}
```

从例1和例2中可以看出他们有个公共的地方就是

```typescript
const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });
```



#### 当我们想要在两个JavaScript函数之间共享逻辑时，我们将它提取到第三个函数

例如： 新建一个自定义hook   **useFriendStatus**  以**use**开头的函数

我们可以决定它作为参数需要什么，以及它应该返回什么（如果有的话）。换句话说，它就像一个普通的功能

我们`useFriendStatus`Hook 的目的是订阅我们朋友的身份。这就是为什么它需要`friendID`作为一个参数，并返回这位朋友是否在线：

```typescript
# 里面没有任何新内容 - 逻辑是从上面的组件中复制出来的。

import React, { useState, useEffect } from 'react';

# 接收一个id
function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
      # 更改状态
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }
	
      # 阿里云API
    ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
    };
  });

  # 返回是否在线状态
  return isOnline;
}
```

#### 使用自定义Hook

可以在回头看看例1

```typescript
function FriendStatus(props) {
  const isOnline = useFriendStatus(props.friend.id);

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```

例2

```typescript
function FriendListItem(props) {
  const isOnline = useFriendStatus(props.friend.id);

  return (
    <li style={{ color: isOnline ? 'green' : 'black' }}>
      {props.friend.name}
    </li>
  );
}
```

