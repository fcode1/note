## React学习笔记

创建

```
$ cnpm install -g create-react-app
$ create-react-app my-app
$ cd my-app/
$ npm start
```

### 1.JSX

JSX是Javascript和XML结合的一种格式。React发明了JSX，利用HTML语法来创建虚拟DOM。当遇到<，JSX就当HTML解析，遇到{就当JavaScript解析。

js写法：

```javascript
var child1 = React.createElement('li', null, 'First Text Content');
var child2 = React.createElement('li', null, 'Second Text Content');
var root = React.createElement('ul', { className: 'my-list' }, child1, child2); 
```

JSX写法：

```javascript
var root =(
  <ul className="my-list">
    <li>First Text Content</li>
    <li>Second Text Content</li>
  </ul>
);
```

#### 1.1虚拟DOM

1.用JavaScript对象结构表示DOM树的结构，然后用这个树构建一个真正的DOM树，插到文档中。 

2.当状态变化的时候，重新构建一颗新的对象树，然后用新的树和旧的树进行比较，记录两颗树的差异 。diff-只更新所改变的内容

3.把2所记录的差异应用到步骤1所构建的真正的DOM树上，视图就更新了 。

Virtual DOM实质上是在JS和DOM之间做了一个缓存 

### 2.组件

- ##### 函数组件(fn name)：主要用于 展示视图，内部状态不需要更新时选用，即没有调用过`this.setState`。内部也没有自己的this，也没有生命周期 （16.7版本钩子函数除外） 

   1.格式为大写的函数名称，调用时在render中传入,给组件传入参数只需在组件内定义(传入的topList是数组)

   ```javascript
   render(<Component list={ topList } ></Component>,document.getElementById('root'))
   ```

    //组件以props形式接收上级传过来的参数

   ```javascript
   function Component (props) {
       ....
   }
   ```

   

- ##### 类组件(class name)：主要用于逻辑处理与状态变化--this

   生命周期继承自React.Component

   ```javascript
   class Demo extends React.Component {
       render() {
           return ()
       }
   }
   ```

   组件间属性传递

   ```javascript
   const data = {name,age}
   
   1.命名传递方式
   <Demo list={data}><Demo> //组件传递
       
   render(){
       const {list} =this.props //接受传递属性
   }
   //使用：{list.name} {list.age}
   
   2.结构赋值传递
   <Demo {...data}><Demo>
       
   render(){
       const {name,age} = this.props
   }
   //使用：{name} {age}
   ```

   

- ##### 受控组件(一般加有value属性)

  一般用在需要动态设置其初始值的情况；例如某些form表单信息编辑时，input表单元素需要初始显示服务器返回的某个值然后进行编辑。 

- ##### 非受控组件(defaultValue)

   一般用于无任何动态初始值信息的情况； 例如form表单创建信息时，input表单元素都没有初始值，需要用户输入的情况 

### 3.事件绑定

exa(1)： 

一般情况下加bind(this)绑定 事件函数 (加bind绑定每触发一次函数都要生成一个新函数)

```javascript
<button onClick={this.handleClick.bind(this)}>添加</button> 
```

 定义函数功能时直接这样写：

```javascript
handleClick(){
        this.setState({
            list:[...this.state.list,this.state.inpVal],
            inpVal:''
        })
    }
```

exa(2)：

不加bind绑定时

```javascript
 <input type="text" value={this.state.inpVal} onChange={this.handleChange} />
```

定义函数功能这样写，箭头函数方式-

```javascript
handleChange = e => {//箭头函数不用bind绑定this
        this.setState({//用setState设置
            inpVal:e.target.value
        })
    }
```

exa(3)：

事件绑定函数需要传多个参数时

```javascript
<button onClick={this.handleDelete.bind(this,index)}>X</button>
```

如果定义函数功能时也用箭头函数的方式 可用匿名函数包裹传多个参

```javascript
<button onClick={()=> { this.handleDelete(index) }}>X</button>
```

定义函数时，将参数传入

```javascript
handleDelete = index => {
        const list = this.state.list;
        list.splice(index,1);
        this.setState({
            list
        })
    }
```

### 4.属性校验

类组件传值时，用解构赋值方式写在模板内

```javascript
ReactDOM.render(<Person {...person}></Person>, document.getElementById('root'));
```

接收时只需要用   this.props 就可以

属性传过来是无法更改的，可以解构一下

 const { name, age, sex, figure, hobby, salary } = this.props

对传入的属性进行校验，需要加载额外的包，GitHub-**prop-types**   npm install --save prop-types

```javascript
 static propTypes = {
        name: PropTypes.string,
        age: PropTypes.number,
        sex: PropTypes.oneOf(['男','女']),
        figure: PropTypes.objectOf(PropTypes.number),
        hobby: PropTypes.arrayOf(PropTypes.string),
        salary (props,propsName,componentName) {
            if(props[propsName] < 10000){
                return new Error (
                    `${componentName}组件传过来的${propsName}属性的值太小了，应该大于1万`
                );
            }
        }
    }
```

### 5.组件交互

父组件传递给子组件->list={this.state.list} 

孙组件想要操作父组件内容，需要父组件先定义操作方法，再传给子组件，子组件接收，再传给孙组件，孙组件接收才能再操作

- 1. 连续传递非常麻烦，在react16版本之后，新增Context  API，方便两个组件之间相互传递

  需要创建context.js文件

  ```javascript
  import React from 'react'
  
  let {Provider,Consumer} = React.createContext()
  
  export {
      Provider,
      Consumer
  }
  ```

  - 在需要使用的父组件中引入：import {Provider} from './context'; 

  然后在用到的地方最外层包裹一下，并且传一个value<Provider value={ {handleDelete: this.handleDelete} }>  ..........</Provider> 

  2.子组件在使用时也需要引用一下  ：import { Consumer } from './context' 

  包裹一下：里边写一个函数，参数为父元素的方法对象，

  ```javascript
   <Consumer>
       {
           ({handleDelete})=>
               <li>
                   {task}
              <button onClick={ () => { handleDelete(index) }}>X</button>
              </li>
  	}
   </Consumer>
  ```


### 6.生命周期

![](D:\doc\MD\React生命周期.png)





```javascript
import React from 'react';
import ChildLifeCycle from './ChildLifeCycle'

//16.3版本之后移除了componentWillMount、componentWillReceiveProps、componentWillUpdate
class LifeCycle extends React.Component{

    static defaultProps = {}
    static propTypes = {}

    constructor () {//constructor中的内容首先加载
        console.log('1.constructor--constructor中的内容首先加载');
        super();
        this.state = {
            count:0
        }
    }

    componentWillMount () {//组件将要被挂载
        console.log('2.willMount--组件将要被挂载');
    }

    shouldComponentUpdate (nextProps,nextState) {//组件运行，state更改之后——>组件要不要更新？
        //可以用来检测更新的数据与上次数据是否相同，相同则没必要更新,节省性能
        
        console.log('5.shouldUpdate--组件运行，state更改之后——>组件要不要更新？')

        console.log(nextState.count === this.state.count);
        return !(nextState.count === this.state.count)
    }

    componentWillUpdate () {//组件将要更新了
        console.log('6.willUpdate--组件将要更新了')
    }

    componentDidUpdate () {//组件已经更新完毕
        console.log('7.didUpdate--组件已经更新完毕')
    }

    render(){//开始渲染页面
        console.log('3.render--开始渲染页面');
        return(
            <>
              <div>
                  count:{this.state.count}
                  <button onClick={this.handleClick}>+</button>
                  {
                      this.state.count ===0
                        ?<ChildLifeCycle n= {this.state.count}></ChildLifeCycle>
                        :''
                  }
                  
              </div>
            </>
        )
    }

    handleClick = () =>{
        this.setState({
            count:this.state.count + 1
            // count:this.state.count + 0 相加的数据相同，不会做更新
        })
    }


    componentDidMount () {//组件已经被挂载到页面
        //要发送ajax请求最好在这里边 
        console.log('4.didMount--组件已经被挂载到页面');
    }
}

export default LifeCycle;
```

### 7.基础路由

1. 安装路由：cnpm install react-router-dom

2. HashRouter(带#的哈希路由) 和 BrowserRouter(浏览器路由)

3. index.js的配置

4. ```JavaScript
   import React from 'react';
   import ReactDOM from 'react-dom';
   import './styles/index.css'
   import './styles/reset.css'
   import {
      BrowserRouter as Router,//引用浏览器路由并取名Router
      Route, //启用路由路径
      Switch,//暂停向下匹配路由,配合exact使用-》严格匹配，包含的就不会在匹配，除非完全包含
      Redirect,//重定向
      Link,//路由跳转
      NavLink,//带样式的路由跳转
     } from 'react-router-dom';
   import Home from './pages/Home';
   import Activities from './pages/Activities';
   import Topics from './pages/Topics';
   import Login from './pages/Login';
   
   
   ReactDOM.render(
     <Router>
       <>
         <div className="nav">
           <NavLink to='/' exact>首页</NavLink>
           <NavLink to='/activities'>动态</NavLink>
           <NavLink to='/topics'>话题</NavLink>
           <NavLink to='/login'>登录</NavLink>
         </div>
         <div className="content">
           <Switch>
             <Route path = '/' exact component = { Home }></Route>
             <Route path = '/activities' component = {Activities}></Route>
             <Route path = '/topics' component = {Topics}></Route>
             <Route path = '/login' component = {Login}></Route>
             <Redirect to='/'></Redirect>
           </Switch>
         </div>
         
       </>
     </Router>,
     document.getElementById('root')
    );
   
   ```

#### 7.1withRouter

提供给非路由元素使用路由元素参数的能力

```javascript
import React, { Component } from 'react';
import {NavLink,withRouter} from 'react-router-dom'
import './nav.css'
class Nav extends Component {

  render () {
    return (
      <div className="nav">
        <span className="logo" onClick={ this.handleClick }>前端教育</span>
        <NavLink to='/' exact>首页</NavLink>
        <NavLink to='/activities'>动态</NavLink>
        <NavLink to='/topics'>话题</NavLink>
        <NavLink to='/login'>登录</NavLink>
      </div>
    )
  }

  handleClick = () => {
    this.props.history.push('/');
  }

}

export default withRouter(Nav);//把不是通过路由切换过来的组件中，将react-router 的 history、location、match 三个对象传入props对象上，需要包裹一下
```

#### 7.2路由权限控制

路由守卫功能不同于vue，需要自己编写，嵌套一层组件，内部包裹要保护的组件

```javascript
import React from 'react';
import {Route,Redirect} from 'react-router-dom';

//导航属性校验
const PrivateRoute = ({component:Component, ...props}) => {//将component重命名为组件Component，方便下边使用
  return (
    <Route {...props} render={()=>{
      const isLogin = document.cookie.includes('login=true');
      if(isLogin) {
        return <Component></Component>
      }else{
        alert('您还没有登录，请在登录页进行登录')
        return <Redirect to="./login"></Redirect>
      }
    }}></Route> 
  )
}

export default PrivateRoute;
```

路由中使用嵌套的组件

```javascript
 <PrivateRoute path = '/topics' component = {Topics}></PrivateRoute>
```

#### 7.3customlink

Prompt组件应用，提示用户是否离开当前页面

```javascript
import {Switch,Route,Redirect,Prompt} from 'react-router-dom';

 render () {
    return (
      <>
        <Prompt message={(location)=>{
          console.log(location)//通过此属性，可查看当前访问的路径，然后在判断二级路由是否包含在内，包含在内则不跳转
          if(!location.pathname.includes('/activities')){
            return window.confirm('确定要离开吗?')
          }
          return true;
        }}/>
        <div>
          <ActivitiesNav></ActivitiesNav>
          <div className="acContent">
            <Switch>
                <Route path="/activities/recommended" component={ Recommended }></Route>
                <Route path="/activities/all" component={ All }></Route>
                <Route path="/activities/articles" component={ Articles }></Route>
                <Route path="/activities/pins" component={ Pins }></Route>
                <Redirect to="/activities/recommended"></Redirect>
            </Switch>
          </div>
        </div>
      </>
    )
  }
```

Route 渲染组件的三种方式：

- component 最常用，只有匹配location才会加载component对应的React组件
- render 渲染render函数所返回的组件 （只有路径匹配时函数才 调用 ）
- children 不管路由是否匹配都会渲染对应组件
  1. **<Route component>的优先级要比<Route render>高** 

------

NavLink 在页面中只渲染成<a>标签，如需转换为其它标签，需自己封装组件转换

```javascript
import React from 'react';
import { Route } from 'react-router-dom';

//component
//render
//children

const MenuLink = ({to, ...props})=>{//props是Nav中涉及到的所有参数，此写法是单独将to拿出来使用
  return (//将props传入Route中，拿到exact属性
    <Route path={to} {...props} children={(p)=>{//只有route组件才可以使用其下三个属性match history location,故用其包裹一下
      console.log(p.match)
      return (
        <span onClick={()=>{p.history.push(to)}}
              className={p.match ? 'active' : ''}
        >
          {props.children}

        </span>
      )
    }}>
    </Route>
  )
}

export default MenuLink;
```

替换navLink

```javascript
import MenuLink from '../MenuLink';
class Nav extends Component {

  render () {
    return (
      <div className="nav">
        <span className="logo" onClick={ this.handleClick }>前端教育</span>
        {/* <NavLink to='/' exact>首页</NavLink>
        <NavLink to='/activities'>动态</NavLink>
        <NavLink to='/topics'>话题</NavLink>
        <NavLink to='/login'>登录</NavLink> */}
      {
        // MenuLink代替NavLink
      }
        <MenuLink to='/' exact>首页</MenuLink>
        <MenuLink to='/activities'>动态</MenuLink>
        <MenuLink to='/topics'>话题</MenuLink>
        <MenuLink to='/login'>登录</MenuLink>
      </div>
    )
  }

  handleClick = () => {
    this.props.history.push('/');
  }

}
```

### 8.Redux

npm install redux --save

1.创建store文件，引用createStore组件

```javascript
import { createStore } from 'redux';
import reducer from './reducer'

const store = createStore(
  reducer,
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()//加上此句才可以在浏览器中调试Redux
  );

export default store;
```

2.创建reducer文件

```javascript
const initState ={
  inpVal:'',
  list:[]
}

export default (state=initState ,action) =>{
  const newState = JSON.parse(JSON.stringify(state));

  switch(action.type){
    case 'CHANGE_INPUT_VAL':
      newState.inpVal = action.value;
      return newState;
    case 'ADD_TODO_ITEM':
        newState.list.push(action.value);
        newState.inpVal = '';
        return newState;
    case 'DELETE_TODO_ITEM':
        newState.list.splice(action.index,1)
        return newState;
  }

  return state;
}
```

#### 8.1Redux执行顺序：

![](D:\doc\MD\Redux执行过程.png)

1.首先通过getState()获取到state中的数据

```javascript
class TodoList extends Component {

  state = store.getState()//获取reducer中state的数据

}
```

2.当组件要进行更改数据时，首先要执行 action方法，传入对应的类型与修改内容，然后再触发 store.dispatch（）方法

```javascript
handleChange = (e) =>{
    const action = {
      type:'CHANGE_INPUT_VAL',
      value:e.target.value
    }
    store.dispatch(action)
  }

  handleAdd = () =>{
    const action = {
      type:'ADD_TODO_ITEM',
      value:this.state.inpVal
    }
    store.dispatch(action)
  }

  handleDelete = (index) =>{
    const action = {
      type:'DEL_TODO_ITEM',
      index
    }
    store.dispatch(action)
  }
```

3.然后在store中的reducer中进行更改并且是另外复制一份更改，不能直接更改原数据  (更改数据借助reducer)

```javascript
//在reducer中定义state
const initState = {
  inpVal:'',
  list:[]
}

export default (state=initState,action) =>{
  const newState = JSON.parse(JSON.stringify(state));//利用序列化与反序列化快速深度克隆

  switch (action.type) {
    case 'CHANGE_INPUT_VAL':
      newState.inpVal = action.value;
      return newState;
    case 'ADD_TODO_ITEM':
      newState.list.push(action.value);
      newState.inpVal = '';
      return newState;
    case 'DEL_TODO_ITEM':
      newState.list.splice(action.index,1)
      return newState;
  }
  return state;
}
```

4.更改完状态之后，组件通过store.subscribe (fn) 订阅 state中的状态变化，以确保组件知道数据发生了更改，并传入组件中定义的函数作为参数，执行相关指定操作

```javascript
class TodoList extends Component { 
    componentDidMount () {//订阅reducer中的数据变化-->写在constructor内也可以
        store.subscribe(this.handleStoreChange)
      }

      handleStoreChange = () => {  //更新数据函数，在subscribe中订阅
        this.setState(store.getState())
      }
}
```

5.抽离action type

```javascript
//单独建立action.js文件
export const CHANGE_INPUT_VAL ='CHANGE_INPUT_VAL';
export const ADD_TODO_ITEM = 'ADD_TODO_ITEM';
export const DELETE_TODO_ITEM = 'DELETE_TODO_ITEM';
```

```javascript
import {CHANGE_INPUT_VAL,ADD_TODO_ITEM,DELETE_TODO_ITEM} from './action';//导入使用

export default (state=initState ,action) =>{
  const newState = JSON.parse(JSON.stringify(state));

  switch(action.type){
    case CHANGE_INPUT_VAL:
      newState.inpVal = action.value;
      return newState;
    case ADD_TODO_ITEM:
        newState.list.push(action.value);
        newState.inpVal = '';
        return newState;
    case DELETE_TODO_ITEM:
        newState.list.splice(action.index,1)
        return newState;
  }

  return state;
}
```

6.抽离 创建action函数

```javascript
//新建 actionCreators.js 文件，抽离出action函数
import * as Types from './action';

export const getTodoChangeInputValAction = (value) =>{
  return {
    type:Types.CHANGE_INPUT_VAL,
    value
  }
}

export const getTodoAddItemAction = (value) =>{
  return {
    type:Types.ADD_TODO_ITEM,
    value
  }
}

export const getTodoDeleteItemAction = (index) =>{
  return {
    type:Types.DELETE_TODO_ITEM,
    index
  }
}
```

TodoList中传入参数调用

```javascript
handleChange=(e)=>{
    // const action ={
    //   type:CHANGE_INPUT_VAL,
    //   value:e.target.value
    // }
    const action = Action.getTodoChangeInputValAction(e.target.value)
    store.dispatch(action);
  }

  handleAdd=()=>{
    // const action ={
    //   type:ADD_TODO_ITEM,
    //   value:this.state.inpVal
    // }
    const action = Action.getTodoAddItemAction(this.state.inpVal)
    store.dispatch(action);
  }

  handleDelete=(index)=>{
    // const action ={
    //   type:DELETE_TODO_ITEM,
    //   index
    // }
    const action = Action.getTodoDeleteItemAction(index)    
    store.dispatch(action);
  }

```

#### 8.2分别导出reducer

借助redux提供的combineReducers



### 9.React-redux

1.安装：npm install react-redux --save  /  yarn add react-redux

#### 9.1Provider 

Provider 组件 提供store功能，代替store ，在需要使用的组件外进行包裹

```javascript
import React from 'react';
import {render} from 'react-dom';
import {Provider} from 'react-redux';
import store from './store';

import TodoList from './components/TodoList'
import AddCount from './components/AddCount'

// Provider组件 提供store功能，代替store
render(
<Provider store = {store}>
  <AddCount/> 
  <TodoList />
</Provider>, 
  document.getElementById('root')); 
```

#### 9.2 connect

connect方法用于连接，对组件进行包装，替代store.getState() 与 store.subscribe()

Exp1:

```javascript
import React, { Component } from 'react';
import {AddCountAction} from '../store/actionCreators';
// import store from '../store';
import {connect} from 'react-redux';

class Counter extends Component {

  // state = {
  //  count: store.getState().count
  // }

  // componentDidMount(){
  //   store.subscribe(()=>{
  //     this.setState({
  //       count:store.getState().count
  //     })
  //   })
  // }

  render () {
    return (
      <div>
        {/* { this.state.count } */}
        {this.props.count}
        <button onClick={this.AddCount}>add</button>
      </div>
    )
  }

  AddCount=()=>{
    // const action = AddCountAction(1)
    // store.dispatch(action)
    this.props.add(1)//调用props中的add函数
  }
}

const  mapStateToProps = (state) =>({//store中的state-->将store中的state转变到props中
  count: state.count
})

const mapDispatchToProps = (dispatch) =>({//store中的dispatch
  add: (val) => {
    dispatch(AddCountAction(val))
  }
})

export default connect(mapStateToProps,mapDispatchToProps)(Counter);//connect的两个参数
```

Exp2:

```javascript
import React, { Component } from 'react';
import {CHANGE_INPUT_VAL,ADD_TODO_ITEM,DELETE_TODO_ITEM} from '../store/action';
import * as Action from '../store/actionCreators'
import {connect} from 'react-redux';

class TodoList extends Component {

  render () {
    const {inpVal,list} = this.props;
    return (
      <>
        <div>
          <input value={inpVal} onChange={this.handleChange}></input>
          <button onClick={this.handleAdd}>添加</button>
        </div>
        <div>
          <ul>
            {
              list.map((item,index)=>(
                <li key={item+index}>
                  {item}<button onClick={()=>{this.handleDelete(index)}}>X</button>
                </li>
              ))
            }
          </ul>
        </div>
      </>
    )
  }
  handleChange=(e)=>{
    this.props.changeVal(e.target.value)
  }

  handleAdd=()=>{
    this.props.addItem(this.props.inpVal)
  }

  handleDelete=(index)=>{
    this.props.deleteItem(index)
  }


  handleStoreChange=()=>{  //更新数据函数，在subscribe中订阅
    this.setState(store.getState())
  }

}

  const mapStateToProps = (state) => ({
    inpVal:state.inpVal,
    list:state.list
  })
  const mapDispatchToProps = (dispatch) => ({
    changeVal:(val) => {
      dispatch(Action.getTodoChangeInputValAction(val))
    },
    addItem:(val) =>{
      dispatch(Action.getTodoAddItemAction(val))
    },
    deleteItem:(index) =>{
      dispatch(Action.getTodoDeleteItemAction(index))
    }
  })

export default connect(mapStateToProps, mapDispatchToProps)(TodoList);
```

- **简化代码，省掉mapDispatchToProps**

connect（）第二个参数直接传action，react会自动识别我们的操作，前提是要引入：action函数--->

```javascript
import * as action from '../store/actionCreators';
```

```javascript
import React, { Component } from 'react';
import * as action from '../store/actionCreators';
// import store from '../store';
import {connect} from 'react-redux';

class Counter extends Component {
  render () {
    return (
      <div>
        {/* { this.state.count } */}
        {this.props.count}
        <button onClick={this.AddCount}>add</button>
      </div>
    )
  }

  AddCount=()=>{
    this.props.add(1)
  }
}

const  mapStateToProps = (state) =>({//store中的state-->将store中的state转变到props中
  count: state.count
})

// const mapDispatchToProps = (dispatch) =>({//store中的dispatch
//   add: (val) => {
//     dispatch(AddCountAction(val))
//   }
// })

export default connect(mapStateToProps,action)(Counter);
```

### 10.axios

安装：npm install axios --save / yarn add axios

请求接口数据，在componentDidMount中。`这个方法获取数据时比较麻烦，本质上是redux在获取数据，但是需要通过react进行获取，然后在一步步推送到reducer中，-->接下来可以通过redux-thunk中间件直接获取`

```javascript
import React, { Component } from 'react';
import store from '../store'
import {CHANGE_INPUT_VAL,ADD_TODO_ITEM,DELETE_TODO_ITEM} from '../store/action';
import * as action from '../store/actionCreators'
import {connect} from 'react-redux';
import axios from 'axios';

axios.interceptors.response.use(res =>{
  if(res.data.code === 0){
    return res.data.data;
  }
  return Promise.reject('请求出错');
})

class TodoList extends Component {

  componentDidMount () {
    axios.get('./list.json').then(res =>{
      this.props.getInitList(res);//触发dispatch
    })
  }

  render () {
    const {inpVal,list} = this.props;
    return (
      <>
        <div>
          <input value={inpVal} onChange={this.handleChange}></input>
          <button onClick={this.handleAdd}>添加</button>
        </div>
        <div>
          <ul>
            {
              list.map((item,index)=>(
                <li key={item+index}>
                  {item}<button onClick={()=>{this.handleDelete(index)}}>X</button>
                </li>
              ))
            }
          </ul>
        </div>
      </>
    )
  }
  handleChange=(e)=>{
    this.props.changeVal(e.target.value)
  }

  handleAdd=()=>{
    this.props.addItem(this.props.inpVal)
  }

  handleDelete=(index)=>{
    this.props.deleteItem(index)
  }

  handleStoreChange=()=>{  //更新数据函数，在subscribe中订阅
    this.setState(store.getState())
  }

}
  const mapStateToProps = (state) => ({
    inpVal:state.inpVal,
    list:state.list
  })

export default connect(mapStateToProps, action)(TodoList);

```

利用axios提前对请求的数据进行过滤

```javascript
axios.interceptors.response.use(res =>{
  if(res.data.code === 0){
    return res.data.data;
  }
  return Promise.reject('请求出错');
})
```

#### 10.1 redux-thunk中间件

- 可以直接在action中进行数据请求-->axios.get()

1.安装：npm install redux-thunk --save / yarn add redux-thunk

2.在store文件中引入，thunk需要借助redux中的applyMiddleware组件包裹使用，并将applyMiddleware(thunk)传入到createStore内作为第二个参数

~~~javascript
import { createStore, applyMiddleware } from 'redux';
import reducer from './reducer';
import thunk from 'redux-thunk';

const store = createStore(
  reducer,
  applyMiddleware(thunk)
  // window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
  )

export default store;
~~~

3.actionCreators中直接发起axios.get()请求，不用在componentDidMount()进行请求了，只需要执行以下函数即可

~~~javascript
export const getInitList = (list) =>{
  // return {
  //   type:Types.GET_INIT_LIST,
  //   list
  // }

  return dispatch => {
    Axios.get('list.json').then(res =>{
      dispatch({
        type:Types.GET_INIT_LIST,
        list:res
      })
    })
  }
}
~~~

~~~javascript
 componentDidMount () {
    this.props.getInitList();
     
    // axios.get('./list.json').then(res =>{
    //   this.props.getInitList(res);
    // })
  }
~~~

#### 10.2 redux-promise 

- 在action中进行异步请求数据

1.安装：yarn add redux-promise

2.使用引入、包裹

~~~javascript
import promise from 'redux-promise';

const store = createStore(
  reducer,
  applyMiddleware(thunk,promise)
  )

export default store;
~~~

3.actionCreators中发起axios.get()请求

~~~javascript

~~~

#### 10.3 redux-logger

- 可以打印请求前与请求后的state变化

  1.安装：yarn add redux-logger

  2.使用：直接引入包裹使用

  ~~~javascript
  import logger from 'redux-logger';
  
  const store = createStore(
    reducer,
    applyMiddleware(thunk,promise,logger)
  ~~~

#### 10.4使用中间件时继续使用Redux调试工具

在redux中引入compose组件，并定义composeEnhancers，最后在createStore中包裹使用composeEnhancers（）

~~~javascript
import { createStore, applyMiddleware ,compose} from 'redux';
import reducer from './reducer';
import thunk from 'redux-thunk';
import promise from 'redux-promise';
import logger from 'redux-logger';

const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__|| compose;

const store = createStore(
  reducer,
  composeEnhancers(applyMiddleware(thunk,promise,logger))
  )

export default store;
~~~

