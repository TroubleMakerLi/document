#### Redux学习

##### 一、什么是Redux？

Redux类似vuex，是一个js容器，用来提供可预测化的状态管理，他配合React进行使用

并不是所有项目都需要用到Redux

##### 二、安装redux

```shell
npm install --save redux
```

##### 三、使用

redux的所有state都以一个对象树的形式保存在单一的store中。

惟一改变 state 的办法是触发 *action*，一个描述发生什么的对象

```jsx
(state, action) => state   // reducer是一个形式为 (state, action) => state 的纯函数。
```

- 引入Redux

```jsx
inport { createStore } from 'redux';
```

- 定义一个方法来，使用action来更新state

```jsx
function changeState(state, action) {
  switch (action.type) {
  case 'INCREMENT':
    return state + 1;
  case 'DECREMENT':
    return state - 1;
  default:
    return state;
  }
}
```



##### 四、redux的三大原则

- 单一数据源

通过store.getState()方法可以查看所有的数据

- state是只读的，只能通过action来进行更新
- 使用纯函数来进行修改



##### 五、细节

- action根据FSA标准最好指定为一个对象
- 通过定义一个reducers函数，传入state和action参数来对state进行修改
- store将state和action联系到了一起
  - store的职责
    - 维持应用的state
    - 提供**getState()**方法来获取state
    - 提供**dispatch(action)**方法更新state
    - 通过**subscribe(listener)**来注册监听器
    - 通过**subscribe(listener)**返回的函数注销监听器

