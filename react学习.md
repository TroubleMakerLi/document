#### react学习

##### 一、了解jsx语法

jsx本质上更接近JavaScript，所以全部的属性均需要采用驼峰形式进行书写

注意：HTML中的`class` 应写作  `className` ，表单中lable的 `for` 应写作 `htmlFor` ，`tabindex` 应写作 `tabIndex`

##### 二、了解元素渲染

React渲染时，需要使用ReactDom.render方法进行渲染。

```jsx
function example() {
  const element = <div>这是一个jsx测试用例</div>;
  ReactDom.render(element, document.querySelector('#app'));
}
```

##### 三、组件

组件的两种生成方式

- 函数声明式组件（无状态组件）

```jsx
function FunComponent() {
  return (
    <div>
      <div>这是一个函数式编程组件</div>
      <span>内容一</span>
    </div>	
  )
}
```

- **开头必须使用大写，否则React无法识别其为一个组件**

使用 `括号` 来将代码包含，防止换行产生的相关影响

多元素时，必须要用一个顶级元素进行包裹，同 VUE的组件形式

- 类声明式组件（有状态时需要使用）

```jsx
class ClassComponent extends React.component {
  render() {
    return (
    	<div>
      	<div>这是一个类编程组件</div>
      	<span>内容二</span>
    	</div>	
    )
  }
}
```

- 组件之间均为独立的个体，各个组件的state互不影响

##### 四、props

props是一个 **只读 **属性，组件无论是使用函数声明还是通过 class 声明，都 **不能修改自身的props**

##### 五、state和生命周期

- state
  - state与porps类似，但是其为 **私有** 状态，在通过class创建的组件中使用。
  - 每一个state都是一个 **对象**，在类中，在 `constructor` 中设置this.state。

```jsx
class TestModule extends React.component {
  constructor(props) {
    super(props);
    this.state = {
      name: null,
      age: null
    }
  }
  
  changeName() {
    this.setState(state => {
      name: ''
    })
  }
  
  render() {
    return (
      <div>大家好，我叫{this.state.name}, 今年{this.state.age}岁了</div>
      <button onClick={this.changeName}>点击更换名称</button>	
    )
  }
}
```

在修改state时，不能直接修改state的值，应该通过React中内置的this.setState() 函数进行修改

```jsx
this.state.name = 'lgw'; // 该方法无法修改名字

this.setState({
  name: '李国维'
}) // 使用React中内置的setState方法进行修改
```

- state的更新是 **异步的**，所以尽量不要使用state的状态来作为条件进行修改。

```jsx
this.state({
  name: this.state.name + 'name'; // 尽量不要这样修改
})

// 可以将state作为参数进行传递
this.state(state => {
  return {
    name: state.name + 'name'
  }
})
```

- state是单向的数据流，自上而下，每一层的修改均只影响其后续节点树的状态

- 生命周期

同vue一样，react也有生命周期。

主要分为三个大阶段：挂载、更新、卸载

- 挂载： 
  - **constructor**：初始化state和props函数，不要在this.state中接受props传递的参数，容易引起bug
  - static getDerivedStateFromProps：
  - **render**：执行react渲染的过程
  - **componentDidMount**：已经挂载到节点中了，此时可以与dom进行交互，添加事件监听
- 更新：
  - static getDerivedStateFromProps：
  - shouldComponentUpdate：在render之前调用，应返回一个对象用来更新props的值
  - **render**：渲染函数，改变之后可更新多次
  - getSnapshotBeforeUpdate：在最近的一次渲染更新前出发，可用来记录聊天位置等信息
  - **componentDidUpdate**：组件更新完毕的操作
- 卸载：
  - **componentWillUnmount**：组件卸载前的操作，不要在此步骤中使用setState方法



##### 六、事件处理

类似Vue一样，React直接在jsx中的dom上进行事件绑定，但是与vue不用的是，React的事件绑定使用**驼峰**进行书写

```jsx
<div onclick=(() => console.log(1))>绑定元素</div>	// 普通事件绑定

<div onClick={this.eventEmmit}></div>
```

- React不能使用 `return false` 阻止默认事件，必须显式的使用`e.preventDefault`来进行
- 在React中，如果不使用()去绑定函数的执行，需要在constructor中进行bind的函数绑定

```jsx
clickEvent() {
  console.log('事件触发了');
}

// 使用()去进行函数执行
<button onClick={() => this.clickEvent()}>点击</button>

// 使用bind进行事件绑定, 为了避免函数使用props进行传递造成额外开销，建议使用此方法进行绑定
construstor() {
  this.clickEvent = this.clickEvent.bind(this);
}
<button onClick={this.clickEvent}>点击</button>
```

##### 七、条件渲染

if，与（&&），三目运算符

##### 列表和key

同Vue一样，循环产生的dom需要包含key字段，用以提升虚拟diff性能

React中循环组件书写形式

```jsx
// 1.直接使用函数产生循环dom进行渲染
function cycleDom() {
  const arr = [1,2,3];
  return (arr.forEach(item => {
    return (<li key={item}>{item}</li>)
  }))
}

// 2.直接在jsx中书写
render(){
  return (arr.forEach(item => {
    return (<li key={item}>{item}</li>)
  })
} 

```

##### 八、表单 && 受控组件

- 对于受控组件来说，输入的值始终由 React 的 state 驱动
- 着重注意input，textarea，select
- 都需要将value通过onChange事件进行监听来切换值
- 多个值进行绑定时，可以通过命名name来进行监听

```jsx
handleChage(e) {
  let name = e.target.name;
  let value = e.target.value;
  
  this.setState({
    [name]: value
  })
}

<input name='name' onChange={this.handleChage()}/>
```

##### 九、状态提升

- 当遇到组件之间要共享的数据时，可将state提升至组件最近的共有父级组件之间，然后通过props进行传递

##### 十、父 <=> 子 传值

- 父 => 子 ： 使用props进行传值
- 子 => 父 ： 父级创建修改state的函数，然后通过props传递给子组件，子组件调用父级传入的函数修改相关数据





#### react的高阶用法

##### 一、context

当我们不想层层传递props时，就可以使用react的content来进行数据传递。

##### 二、Fragment

当我们创建组件的时候，如果某些dom使用div等标签包裹时不能被正确的渲染，需要使用Fragment。例如：tr、td等

```jsx
function Columns() {
  // 创建Fragment来包裹相关不能用顶级元素嵌套的dom
  return (
  	<>
    	<td>1</td>
    	<td>2</td>
    	<td>3</td>
    </>
  )
}

function Columns() {
  // 如果dom是一个循环体，需要包含key属性，使用React.Fragment来进行创建
  return (
    {
        [1,2,3].map(item => {
        	<React.Fragment>
        		<td key={item}>{item}</td>
    			<React.Fragment />
        })
    }
  )
}

class Main extends React.component {
  render() {
    return (
      <div>
        <table>
          <tr>
          	<Columns />
          </tr>
        </table>
      </div>
    )
  }
}
```

##### 三、JSX使用注意事项

- jsx不是一个表达式

```jsx
// 当我们使用jsx时，如果想动态的执行组件，可以使用下面的方法进行
function Module(props) {
  const sourceMap = {
  	name1: 'first',
  	name2: 'second'
	}

  let NameModule = sourceMap[props];
	return (
    <NameModule />
	)
}
```

- if，for等流控制语言无法在jsx中进行书写

##### 四、React支持的事件

- 普通事件：onClick、onChange、onMouseOver等（此类事件发生在 **事件冒泡阶段**）
- 要现在 **事件捕获阶段** 进行事件绑定，只需要为对应的事件添加Capture即可。

例如：onClickCapture、onChangeCapture等





#### React的Hook使用（均不可在class组件中使用）

- useState

使用useState可以为组件创建一些内部state，然后设置一种方法来修改这些状态的值

```jsx
import React, { useState } from 'react';

function Test() {
  let [count, changeCount] = useState(0); // count的默认值为0，使用changeCount来进行count参数的修改
  
  return (
    <div onClick={() => changeCount(count + 1)}>已经点击了{count}次数</div>
  )
}
```

- useEffect

等同于componentDidMount和componentDidUpdate生命周期钩子函数，在dom更新后进行触发

需要清除的可以使用return返回一个函数，不用清除的可以直接返回

```jsx
function chenge() {
  useEffect(() => {
    document.addEventListener('click', () => console.log('123'));
    
    return () => {
      document.removeEventListener('click');
    }
  })
}	
```

**注意：**当有些数据未更新时，可以使用跳过effect来进行性能优化

```jsx
function Test() {
  let [count, changeCount] = useState(0);
  useEffect(() => {
    console.log(count);
  }, [count]); // 使用方括号包裹重复值需要跳过的属性
}
```

- 自定义Hook

使用 `use` 开头

- useRequest（社区库，用于网络请求）

  属性及使用介绍：

  - manual：使用useRequest可以进行网络请求，使用{ manual： true } 即可阻止初始化请求，当配置此项时，必须使用`run`方法进行手动请求。
  - pollingInterval： 轮询使用
  - fetchKey：并行请求，用以借口维护多个请求状态
  - debounceInterval：请求防抖
  - throttleInterval： 请求节流

