![](https://github.com/lj614418910/blog/blob/master/images/redux.png)
# Redux学习笔记
## 设计思想
### 核心概念
- 所有的状态存放在`Store`。组件每次重新渲染，都必须由状态变化引起。
- 用户在 UI 上发出`action`。
- `reducer`函数接收`action`，然后根据当前的`state`，计算出新的`state`。

![](https://github.com/lj614418910/blog/blob/master/images/redux-architecture.png)

### 动机
- 随着 JavaScript 单页应用开发日趋复杂，JavaScript 需要管理比任何时候都要多的 `state` （状态）。通过限制更新发生的时间和方式，Redux 试图让 `state` 的变化变得可预测。

### 生命周期
#### Redux 应用中数据的生命周期遵循下面 4 个步骤：

##### 调用 store.dispatch(action)。

- Action 就是一个描述“发生了什么”的普通对象。比如：

``` javascript
{ type: 'LIKE_ARTICLE', articleId: 42 };
{ type: 'FETCH_USER_SUCCESS', response: { id: 3, name: 'Mary' } };
{ type: 'ADD_TODO', text: 'Read the Redux docs.'};
```
- 可以把 `action` 理解成新闻的摘要。如 “玛丽喜欢42号文章。” 或者 “任务列表里添加了'学习 Redux 文档'”。
你可以在任何地方调用 `store.dispatch(action)`，包括组件中、XHR 回调中、甚至定时器中。

##### Redux store 调用传入的 reducer 函数。

- Store 会把两个参数传入 `reducer`： 当前的 `state` 树和 `action`。例如，在这个 `todo` 应用中，根 `reducer` 可能接收这样的数据：

``` javascript
// 当前应用的 state（todos 列表和选中的过滤器）
let previousState = {
    visibleTodoFilter: 'SHOW_ALL',
    todos: [
        {
            text: 'Read the docs.',
            complete: false
        }
    ]
}

// 将要执行的 action（添加一个 todo）
let action = {
    type: 'ADD_TODO',
    text: 'Understand the flow.'
}

// render 返回处理后的应用状态
let nextState = todoApp(previousState, action);
```
- 注意 reducer 是纯函数。它仅仅用于计算下一个 state。它应该是完全可预测的：多次传入相同的输入必须产生相同的输出。它不应做有副作用的操作，如 API 调用或路由跳转。这些应该在 `dispatch(action)` 前发生。

##### 根 reducer 应该把多个子 reducer 输出合并成一个单一的 state 树。

- 根 `reducer` 的结构完全由你决定。Redux 原生提供`combineReducers()`辅助函数，来把根 `reducer` 拆分成多个函数，用于分别处理 `state` 树的一个分支。

-下面演示 combineReducers() 如何使用。假如你有两个 reducer：一个是 todo 列表，另一个是当前选择的过滤器设置：

``` javascript
function todos(state = [], action) {
    // 省略处理逻辑...
    return nextState;
}

function visibleTodoFilter(state = 'SHOW_ALL', action) {
    // 省略处理逻辑...
    return nextState;
}

let todoApp = combineReducers({
    todos,
    visibleTodoFilter
})
```
 
- 当你触发 `action` 后，`combineReducers` 返回的 `todoApp` 会负责调用两个 reducer：

``` javascript
let nextTodos = todos(state.todos, action);
let nextVisibleTodoFilter = visibleTodoFilter(state.visibleTodoFilter, action);
```

- 然后会把两个结果集合并成一个 state 树：


``` javascript
return {
    todos: nextTodos,
    visibleTodoFilter: nextVisibleTodoFilter
};
```
- 虽然 `combineReducers()` 是一个很方便的辅助工具，你也可以选择不用；你可以自行实现自己的根 `reducer`！

##### Redux store 保存了根 reducer 返回的完整 state 树。

- 这个新的树就是应用的下一个 state！所有订阅 store.subscribe(listener) 的监听器都将被调用；监听器里可以调用 store.getState() 获得当前 state。

- 现在，可以应用新的 `state` 来更新 UI。如果你使用了 React Redux 这类的绑定库，这时就应该调用 `component.setState(newState)` 来更新。

### 三大原则

#### 单一数据源
- 整个应用的 `state` 被储存在一棵 `object tree` 中，并且这个 `object tree` 只存在于唯一一个 `store` 中。 

#### State 是只读的
- 惟一改变 `state` 的方法就是触发 `action`，`action` 是一个用于描述已发生事件的普通对象。

#### 使用纯函数来执行修改
- 为了描述 `action` 如何改变 `state tree` ，你需要编写 `reducers`。

## Redux 核心API
- Redux的核心是一个`store`，这个`store`有Redux提供的`createStore(reducers,[initialState])`方法生成。从函数签名看出，想生成`store`，必须传入`reducers`，同时也可以传入第二个可选参数初始化状态`initialState`。

### createStore

- 使用方法 `const store = createStore(reducer);`

- #### reducer

    - 在Redux里，负责响应`action`并修改数据的角色就是`reducer`。`reducer`本质上是一个纯函数，其函数签名为`reducer(previousState, action) => newState`。`reducer`在处理`action`时，需传入一个`previousState`参数。`reducer`的职责就是根据`previousState`和`action`来计算出新的`newState`。
    - 使用方法将`reducer`即下面的`todo`作为参数传入createStore(todo)中

    ``` javascript
    //以下为reducer的格式
    const todo = (state = initialState, action) => {
        switch(action.type) {
            case 'XXX':
                return //具体的业务逻辑;
            case 'XXX':
                return //具体的业务逻辑;   
            default:
                return state;
        }
    }
    ```

- #### getState()

    - 使用方法`getState()`
    - 获取`store`中的状态。

- #### dispatch(action)

    - 使用方法`store.dispatch(action)`
    - 分发一个`action`，并返回这个`action`，这是唯一能改变`store`中数据的方式。store.dispatch接受一个Action对象作为参数，将它发送出去。

- #### subscribe(listener)

    - 使用方法`store.subscribe(listenter)`
    - 注册一个监听者，它在`store`发生变化时被调用,一旦State发生了变化，就会自动执行这个函数。通过subscribe绑定了一个监听函数之后，只要dispatch了一个action，所有监听函数都会自动执行一遍。

- #### replaceReducer(nextReducer)

    - 更新当前`store`里的`reducer`，一般只会在开发者模式中调用该方法。

- #### createStore的实现

    - 包含`getState()`，`dispatch(action)`，`subscribe(listener)`；本函数近似源码，可简单实现功能与帮助理解`createStore`的原理
    
    ``` javascript
    const createStore = (reducer) => {
        let state; //声明一个变量承接状态
        let list = [];//声明一个数组用于储存监听函数
        const getState = () => {
            return state;//直接返回state;
        }
        const dispatch = (action) =>{
            state = reducer(state, action);//更新状态,且循环list数组，并执行里面的事件
            list.forEach((fn) => {
                fn();
            })
        }
        const subscribe = (fn) => {
            list.push(fn);//将函数传入list中
            return () => {
                list = list.filter(cd => cd != fn)
            }
        }
        return {
            getState,
            subscribe,
            dispatch
        }
    }
    ```

- ### combineReducers

    - 随着应用变得复杂，需要对 `reducer` 函数 进行拆分，拆分后的每一块独立负责管理 `state` 的一部分。`combineReducers` 辅助函数的作用是，把一个由多个不同 `reducer` 函数作为 `value` 的 `object`，合并成一个最终的 `reducer` 函数，然后就可以对这个 `reducer` 调用 `createStore`。

- #### 使用方法

    - 将多个不同的`reducer`作为对象的属性传入`combineReducers({})`函数中，

``` javascript
const rootReducer = combineReducers({
    reducer1,
    reducer2，
    ...
})
```

- #### 返回值

    - (Function)：一个调用 reducers 对象里所有 reducer 的 reducer，并且构造一个与 reducers 对象结构相同的 state 对象。

    ``` javascript
        let store = createStore(rootReducer)
        //store = {
            reducer1: ... ,
            reducer2: ... ,
            ...
        }
        let {reducer1, reducer2} = store; //取出
    ```

- #### 模拟实现combineReducers

``` javascript
//研究逻辑看这个
const combineReducers = (reducers) => {
    return (state = {}, action) => {
        let newState = {};
        Object.keys(reducers).forEach((key) =>{
            newState[key] = reducers[key](state[key], action);
        })
        return newState;
    }
}
    
//简写装逼看这个
const combineReducers = (reducers) => (state = {}, action) => Object.keys(reducers).reduce((newState, key) => {
    newState[key] = reducers[key](state[key], action);
    return newState;
},{})
```

## react的跨级组件通信（虫洞）帮助理解（react-redux）
- 此方法为React中的方法，随着应用变的越来越复杂，组件嵌套越来越深，有时要从最外层将一个数据一直传递到最里层。
- 理论上，通过`prop`一层层传递下去当然是没问题的。不过这也太麻烦啦，要是能在最外层和最里层之间开一个穿越空间的虫洞就好了。
- React的开发者也意识到这个问题，为我们开发出了这个空间穿越通道 —— `Context`。
- 注意：`context`一直都在React源码中，但在React0.14版本才被记录官方文档。官方并不太推荐大量使用，虽然它可以减少逐级传递，但当组件复杂时，我们并不知道`context`是从哪传来的。它就类似于全局变量。

- ### context使用方法

    - 在外层定义一个`getChildContext`方法，在父层制定`childContextTypes`。
    
    ``` javascript
    class Provider extends Component{
        getChildContext() {
            return {store: ...};
        }
        render(){
            return(
                this.props.children
            )
        }
    }
    
    Provider.childContextTypes = {
        store : React.PropTypes.object
    };
    ```

    - 在内层设置组件的`contextTypes`后，即可在组件里通过`this.context.`来访问。
  
    ``` javascript
    class child extends Component{
        render(){
            const store = this.context.store;
            return(
                <div>1</div>
            )
        }
    }
    child.contextTypes = {
        store: React.PropTypes.object
    }
    ```



## 解读react-redux

- 前面说到Redux的核心只有一个`createStore()`方法，这样还不足以让Redux在我们的react应用中发挥作用，还需要react-redux库 ———— Redux官方提供的React绑定。
- react-redux提供了一个组件和一个API帮助Redux和React进行绑定，一个是React组件`<Provider />`, 一个是`connect()`。关于它们，我们需要知道的是，`<Provider />`接受一个`store`作为`props`，它是整个Redux应用的顶层组件，而`connect()`提供了在整个React应用的任意组件中获取`store`中数据的功能。

### Provider
- 其实就是创建一个外层包裹住整个Redux应用。

- `<Provider />`主要源码

``` javascript
export default class Provider extends Component {
    getChildContext() {
        return { store: this.store }
    }

    constructor(props, context) {
        super(props, context)
        this.store = props.store
    }

    render() {
        return Children.only(this.props.children)
    }
}
```
- 用法

``` javascript
ReactDom.render(
    <Provider store = {store}>
      <App />
    </Provider>,
    document.getElementById("root")
)
```

### connect
- 使用方法`connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])(TodoApp)`

- 上面代码看似那么长，但其实理解起来不太难，前四个参数是选填属性，根据需求填入即可。`connect(...)`调用后会返回一个函数这个函数可传一个参数，即你需要绑定的组件。
- 模拟实现`connect`函数,只针对前两个关键参数。

``` javascript
const connect = (mapStateToProps, mapDispatchToProps) => {
    return (WrapperComponent) => {
    class Connect extends Component {
        componentDidMount() {
            const store = this.context.store;
            this.unsubscribe = store.subscribe(() => {
                this.forceUpdate();
            })
        }
        componentWillUnmount() {
            this.unsubscribe();
        }
        render (){
            const store = this.context.store;
            const stateProps = mapStateToProps(store.getState());
            const dispatchProps = mapDispatchToProps(store.dispatch);
            const props = Object.assign({}, stateProps, dispatchProps);

            // return <WrapperComponent {...props} />;
            return React.createElement(WrapperComponent, props);
        }
    }
    Connect.contextTypes = {
        store: React.PropTypes.object
    };
        return Connect;
    }
}
```

#### mapStateToProps
- 官方解释： 如果定义该参数，组件将会监听 Redux `store` 的变化。任何时候，只要 Redux `store` 发生改变，`mapStateToProps` 函数就会被调用。该回调函数必须返回一个纯对象，这个对象会与组件的 `props` 合并。如果你省略了这个参数，你的组件将不会监听 Redux `store`。如果指定了该回调函数中的第二个参数 `ownProps`，则该参数的值为传递到组件的 `props`，而且只要组件接收到新的 `props`，`mapStateToProps` 也会被调用。

- 使用方法(其实里面第一个参数就是最早在 `<Provider store = {store}>`传入的`store`，于是可以在子组件上访问`store`里的属性)

``` javascript
const mapStateToProps = (state, [ownProps]) => {
    return {
        todos : state.todos
    }
}
```

#### mapDispatchToProps
- 官方解释： 如果传递的是一个对象，那么每个定义在该对象的函数都将被当作 Redux `action creator`，而且这个对象会与 Redux `store` 绑定在一起，其中所定义的方法名将作为属性名，合并到组件的 `props` 中。如果传递的是一个函数，该函数将接收一个 `dispatch` 函数，然后由你来决定如何返回一个对象，这个对象通过 `dispatch` 函数与 `action creator` 以某种方式绑定在一起（提示：你也许会用到 Redux 的辅助函数 bindActionCreators()）。如果你省略这个 `mapDispatchToProps` 参数，默认情况下，`dispatch` 会注入到你的组件 `props` 中。如果指定了该回调函数中第二个参数 `ownProps`，该参数的值为传递到组件的 `props`，而且只要组件接收到新 `props`，`mapDispatchToProps` 也会被调用。
- 使用方法（用于传递方法）。其实总而言之，`mapStateToProps`是用来传递属性状态的，而`mapDispatchToProps`是用来传递改变的方法的。

``` javascript
const mapDispatchToProps = (dispatch, [ownProps]) => {
    return{
        ... : () => {
            dispatch(...)
        }
    }
}
```

#### mergeProps
- `mergeProps(stateProps, dispatchProps, ownProps)`可以接受stateProps, dispatchProps, ownProps三个参数。
- `stateProps`就是传给`connect`的第一个参数`mapStateToProps`最终返回的`props`。
- `dispatchProps`就是传给`connect`的第二个参数`mapDispatchToProps`最终返回的`props`。
- 而`ownProps`则为组件自己的`props`。

#### options
- 如果指定这个参数，可以定制 connector 的行为。

    - [pure = true] (Boolean): 如果为 `true`，`connector` 将执行 `shouldComponentUpdate` 并且浅对比 `mergeProps` 的结果，避免不必要的更新，前提是当前组件是一个“纯”组件，它不依赖于任何的输入或 `state` 而只依赖于 `props` 和 Redux `store` 的 `state`。默认值为 `true`。
    
    - [withRef = false] (Boolean): 如果为 `true`，`connector` 会保存一个对被包装组件实例的引用，该引用通过 `getWrappedInstance()` 方法获得。默认值为 `false`。







