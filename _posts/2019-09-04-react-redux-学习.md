---
author: lexiao
comments: true
date: 2019-09-04 11:11:11+00:00
layout: post
link: http://localhost/blog/?p=440
published: true
slug: null
title: react-redux-学习.md
wordpress_id: 440
tags: recent
categories:
- redux
---


# 如何定义 mapStateToProps

原型
```js
function mapStateToProps(state, ownProps?)
```

## 参数说明

- 第一参数： state ： （不可缺少的）
  - 整个 store state ， 等价于  store.getState()
- 第二参数： ownProps ： （可选的）
  - 如果mapStateToProps() 函数需要知道（被连接的）组件的内部数据才能准确获取 store state，那么可以使用 ownProps 来传递内部数据。（参考下面的代码例子）。
  - 尽量避免添加该参数，减少意外渲染的情况。
  - 如果不需要该参数，应该在函数signature 中声明有且仅有一个参数，避免默认值或者spread等情况意外添加第二参数。

## 返回值

- 简单 object
- 内含（被连接的）组件所需数据
- <em>返回对象中的每个属性，会成为（被连接的）组件的一个参数。</em>
- <strong>返回对象中的所有属性，会用以判断组件是否需要重新渲染。（参考下面的代码例子）</strong>
- 要避免在值不变的情况下而重新生成新object而意外导致组件重新渲染。（例如使用了 someArray.map()， Object.assign， spread operator ... )

## 其它说明

- 该函数会被作为第一个参数，传递给 connect() 函数
- 何时调用：  每次有 store state changes
- 每次调用时，所有定义了该函数的组件都会运行，所以该函数运行速度必须快。
- 如果不需要，可以传 null 或者 undefined
- 可以是函数或者箭头函数
- 应该是纯函数，不包含异步操作（如 IO ）。


ownProps 代码例子
```js

// Todo.js

function mapStateToProps(state, ownProps) {
  const { visibilityFilter } = state
  const { id } = ownProps
  const todo = getTodoById(state, id)

  // component receives additionally:
  return { todo, visibilityFilter }
};

// Later, in your application, a parent component renders:
<ConnectedTodo id={123} />
// and your component receives props.id, props.todo, and props.visibilityFilter

```

返回值的代码例子：
```js

function mapStateToProps(state) {
  return {
    a: 42,
    todos: state.todos,
    filter: state.visibilityFilter
  }
}

// component will receive: props.a, props.todos, and props.filter
```

??? Selector Functions 


---

# 如何定义 mapDispatchToProps

## 发送action 到 store 的 2 种方法

1. 被连接的组件有默认的 props.dispatch
2. 传递函数作为参数给 connect() 函数

## 第一种方法

连接组件的代码例子：

```js

connect()(MyComponent)
// which is equivalent with
connect(
  null,
  null
)(MyComponent)

// or
connect(mapStateToProps /** no second argument */)(MyComponent)
```

发送action的代码例子：

```js

function Counter({ count, dispatch }) {
  return (
    <div>
      <button onClick={() => dispatch({ type: 'DECREMENT' })}>-</button>
    </div>
  )
}
```

## 第二种方法

### 说明：
- 如果使用该方法，那么组件就不会接受 dispatch 作为参数（第一种方法）。
- 可以把dispatch的责任下放给子组件，使得子组件即使不知道 redux store，也可以往store 发送action。


## 定义 mapDispatchToProps 的 2 种形式

## 第一种形式 -- 函数

### 函数参数

- 第一个参数 ：  dispatch
- 第二个参数 ：  ownProps ( optional )

### 返回值

- 简单对象
  - 对象中每个 key 会被作为组件的参数（props）
  - 对象中每个 value 一般应该是一个 dispatch action function .
- 如果使用 action creator ， 那么就只要使用 key 就可以了。
- 使用 redux 的 bindActionCreators() ，可以简化步骤。


## 第二种形式 -- 对象 （官方推荐的形式）

一个包含多个 action creators 的对象。
connect() 会自动调用 bindActionCreators 把这些 action creators 注册到dispatch 。


## 补充说明

- 如果在 connect() 里面不提供 mapDispatchToProps ，那么组件会接收 dispatch 作为参数（反之不会有该参数）。
- 如果只想要 mapDispatchToProps 而不要 mapStateToProps ， 可以把 mapStateToProps 设置为 null 。


mapDispatchToProps 代码例子：

```js

//----------------------------------------------------------------
//----------------------------------------------------------------
//----------------------------------------------------------------

// ---------------  这是 action 的定义 ---------------------------
export const increment = () => ({ type: "INC" });
export const decrement = () => ({ type: "DEC" });
export const reset = () => ({ type: "RESET" });


// ---------------  不定义mapDispatchToProps， 直接使用 dispatch ---------------------------

import { connect } from "react-redux";
import { increment, decrement, reset } from "./actions";

function Counter({ count, dispatch }) {
  return (
    <div>
      <button onClick={() => dispatch(decrement())}>-</button>
      <span>{count}</span>
      <button onClick={() => dispatch(increment())}>+</button>
      <button onClick={() => dispatch(reset())}>reset</button>
    </div>
  );
}

const mapStateToProps = state => ({
  count: state.count
});

export default connect(mapStateToProps)(Counter);


// ---------------  mapDispatchToProps 函数形式 ---------------------------

function Counter({ count, increment, decrement, reset }) {
  return (
    <div>
      <button onClick={decrement}>-</button>
      <span>{count}</span>
      <button onClick={increment}>+</button>
      <button onClick={reset}>reset</button>
    </div>
  );
}

const mapStateToProps = state => ({
  count: state.count
});

const mapDispatchToProps = dispatch => ({
  decrement: () => dispatch(decrement()),
  increment: () => dispatch(increment()),
  reset: () => dispatch(reset())
});

export default connect(
  mapStateToProps,
  mapDispatchToProps
)(Counter);


// ---------------  mapDispatchToProps 对象形式 ---------------------------

const mapDispatchToProps = {
  decrement,
  increment,
  reset
};


// ---------------  mapDispatchToProps 使用 bindActionCreators ---------------------------

function mapDispatchToProps(dispatch) {
  return bindActionCreators({ increment, decrement, reset }, dispatch)
}


// ---------------  mapDispatchToProps 最简单形式（官方推荐） ---------------------------

export default connect(
  mapStateToProps,
  { increment, decrement, reset }
)(Counter);


```



---
---
---

# connect() 函数

原型

```js
function connect(mapStateToProps?, mapDispatchToProps?, mergeProps?, options?)
```



## 第三个参数：   mergeProps

说明：
- 提供一个途径，把 stateProps， dispatchProps， ownProps 三个参数合并成为一个参数，然后传递给组件。
- 如果不提供该 mergeProps，那么组件接收到的参数会是 { ...ownProps, ...stateProps, ...dispatchProps }
- mergeProps 的signature是 mergeProps?: (stateProps, dispatchProps, ownProps) => Object
- 






## 第四个参数：   options





































