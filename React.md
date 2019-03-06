## React学习日志

#### 1.JSX语法

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

### 2.组件

- ##### 函数组件(fn name)：主要用于 展示视图，内部状态不需要更新时选用，即没有调用过`this.setState` 

- ##### 类组件(class name)：主要用于逻辑处理与状态变化

- ##### 受控组件(一般加有value属性)

  一般用在需要动态设置其初始值的情况；例如某些form表单信息编辑时，input表单元素需要初始显示服务器返回的某个值然后进行编辑。 

- ##### 非受控组件(defaultValue)

   一般用于无任何动态初始值信息的情况； 例如form表单创建信息时，input表单元素都没有初始值，需要用户输入的情况 

### 3.事件绑定

exa(1)：

一般情况下加bind(this)绑定 事件函数

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

