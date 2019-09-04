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
tags: summary
categories:
- 前端
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
- 返回对象中的每个属性，会成为（被连接的）组件的一个参数。
- 返回对象中的所有属性，会用以判断组件是否需要重新渲染。（参考下面的代码例子）
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
















