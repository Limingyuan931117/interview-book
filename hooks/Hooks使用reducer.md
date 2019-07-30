## 6月14日笔记

Hook只能在最顶层使用

不可以用任何`if` 包裹hook

react是数据驱动UI

useRneducer(reducer, 默认值)

reducer是函数

这个状态和useState是一样的



const item = `<H1>Hellow Word</H1>` 

ReactDom.render(item, doucment.getElementById('root'))

```jsx
const item = {
  hists:[
      {
          title:'标题1',
          url:'www.123',
          author:'wang1',
          objectID:'1'
      },{
          title:'标题23
          url:'www.1234',
          author:'wang2',
          objectID:'2'
      },{
          title:'标题3',
          url:'www.1233',
          author:'wang3',
          objectID:'3'
      }
  ]  
}

function App(){
    	
    const [value, setValue] = useState(item.hists[0])
    const [isloading, setIsLoading] = useState(true)
    const [isFailue, setFailue] useState(false)
    
    useEffect(() => {
        
        fetch()
            .then(response => response.json())
            .then(data => setValue({value:data}))
            .catch(() => setFilure(true),setInsLoading(false))
    }, [])
    
	return <div>{isLoading === true ? '正在加载中' : ''}</div> <div>{item.hists.map(data => <div>{data.title}</div>)}</div>
}
```





## redux笔记

- 学习为什么要使用 React Context。
- 学习使用 React Context 解决跨级组件通信。
- 学习旧的 React Context 语法以及迁移指南

需要在全局共享的数据还很多，而且这些共享的数据往往还会发生变化。这些全局的变化的数据，我们统称为**全局状态**。



使用 React Context 一般分成三个部分：

- 创建 React Context
- 提供 Context 数据
- 使用 Context 数据

使用useContext()来接收上文的重定义上下文，获取上文传进来的属性

useContext, useMemo  一起使用 缓存数据，以免重绘

类组件只能使用 Context.consumer 传入参数，里面是jsx组件

做一下React.context的练习题，自己查找练习题，

主题订制，登录状态，待办列表，使用useMemo

```jsx
<button>这里也可以用到Provider的默认值</button>
<ThemeContext.Provider value={{color:'red'}}>
	<div>这里可以用到重新赋的值</div>
</ThemeContext.Provider>
```

可以通过Context.Provider的方式修改传进的值

没有Provider尝试是否一样可以生效



### 全局共享状态

在开发 React 应用程序时，往往需要在整个应用级别共享一些数据，如：

- 当前登录人信息
- UI 主题定制信息
- 购物车里的物品
- 全局的消息提示



## 第一步

### 定义状态和动作的数据结构

确认完需求后，就可以分析出该应用程序有哪些状态和动作，并且用 TypeScript 类型定义出来

* 待办列表的每列
* 状态中有什么数据{待办列表整个数据， 显示过滤}
* 添加动作{如： 点击添加待办数据}

## 第二步

### 创建动作

添加动作

```typescript
function AddTodo(item: TODO):AddTodoAction {
    return {
        type: 'ADD_TODO',
        payload: item,  // 添加的新的数据
    }
}

export {AddTodo}
```

## 第三步

### reducers , 分工明确， 如果待办列表状态和 过滤分页分开管理

```typescript
function todos(state: Todo[] = [] ,action: AddTodoAction | 其他){ // state记得判空
    switch(action.type){
        case '':
            return state,
        default:
            return state
    }
}
```

```typescript
function filters(state: Todo[] = [] ,action: AddTodoAction | 其他){ // state记得判空
    switch(action.type){
        case '':
            return state,
        default:
            return state
    }
}
```

#### 合并操作：

```typescript
import {combineReducers} from 'redux';

const reducer = combineReducers({
    todos,
    filters
})

export default reducer
```



## 初始化程序

类似于  connect()方式关联想要管理状态的表单

```typescript
import React from 'react';
import {createStore} from 'redux';
import {Provider} from 'react-redux';
import App from './App';
import reducer from './reducer';

const store = createStore(reducer); // 创建store， 传入整个reducer

function AppContainer(){
    return <Provider store={store}>
        	<App />
        </Provider>
}
```

## 触发Action

### useDispatch

```typescript
import { useDispatch } from 'react-redux';

const dispatch = useDispatch();

const handleClick = () => {
	dispatch(AddTodo(item)) // dispatch里添加action， action中添加需要传入的参数即可触发Action
}
```

### useSelector用来获取由 React Redux 托管的 Redux Store 的状态。用来获取由 React Redux 托管的 Redux Store 的状态。

```typescript
import { useSelector } from 'react-redux';

const getVisivibleTodoList = (todos, type) =>{
    switch(type){
        case "SHOW_COMPLETED"：
            return todos.filter(todo => todo.completed)
        default:
            return todos;
    }
}

export default function UseSelector () {
	const todos = useSelector(state => getVisivibleTodoList(state.todos, state.visibilistyType)); // 这里的useSelector中的装填通过函数返回新的需要的数据
}
```



## Immer

### 用法1

produce(localState,(draft) => {})  localState初始数据

draft 原始数据上做任何操作都没关系，返回一个新的值

任何一个需要做复杂的不可变数据的操作的时候使用

### 用法2

与Reducer结合使用

```typescript
import {useReducer} from 'react';
import { produce } from 'immer';

const todoReducer = immer((draft,action) => {
    switch(action.type){
        case:
            return;
        default:
            return;
    }
})

const [todos, dispatch] =  useReducer(todoReducer, [])

export default useReducer(todoReducer, [])
```



直接把Reducer 放到上下文当中，直接从useContext中引用状态

书籍维护demo， useRestTable

