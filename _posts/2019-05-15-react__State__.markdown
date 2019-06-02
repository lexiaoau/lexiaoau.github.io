---
author: lexiao
comments: true
date: 2019-05-15 11:11:11+00:00
layout: post
link: http://localhost/blog/?p=442
published: true
slug: null
title: react 之 State
wordpress_id: 442
categories:
- jsx
- react
- 前端
---


1. [https://zh-hans.reactjs.org/docs/state-and-lifecycle.html](https://zh-hans.reactjs.org/docs/state-and-lifecycle.html)
2. State 与 props 类似，但是 state 是私有的，并且完全受控于当前组件。
3. 使用 state 的组件应该定义为类
    1. like below

        ```js
            import React, { Component } from 'react'
            class Clock extends React.Component {
                render() {
                    return (
                        <div>
                        </div>
                    );
                };
            }
        ```

    2. 添加一个 [class 构造函数](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes#Constructor)，然后在该函数中为 `this.state` 赋初值：

        ```js
            class Clock extends React.Component {
                constructor(props) {
                super(props);
                this.state = {date: new Date()};
                // ES6 类中函数必须手动绑定
                this.handleChange = this.handleChange.bind(this);
            }
        ```

4. state 及生命周期
    1. 挂载（mount）
        - 当组件第一次被渲染到 DOM 中的时候，这在 React 中被称为“挂载（mount）”。
        - `componentDidMount()` 方法会在组件已经被渲染到 DOM     中后运行，所以，最好在这里设置计时器：            -
    2.  卸载（umount）
        - 当 DOM 中 Clock **组件被删除的时候**，应该清除计时器。这在 React     中被称为“卸载（umount）”。
        - 在 `componentWillUnmount()` 生命周期方法中清除计时器

            ```js
            componentWillUnmount() {
                    clearInterval(this.timerID);
            }
            ```

5. 更新 state 值
    1. `this.setState()` 来时刻更新组件 state

        ```js
        this.setState({
            date: new Date()
        });
        ```

6. 获取 state 的值
    1. like below

        ```js
        < FormattedDate date={this.state.date} >
        ```

7. 正确使用state 的规则
    1. **不要直接修改 State**，  而是应该使用 `setState()`
    2. **State 的更新可能是异步的**
        1. 出于性能考虑，React 可能会把多个 `setState()` 调用合并成一个调用。因为 `this.props` 和 `this.state` 可能会异步更新，所以你不要依赖他们的值来更新下一个状态。
        2. 要解决这个问题, 可以让 `setState()` 接收一个函数而不是一个对象。这个函数用上一个 `state` 作为第一个参数，将此次更新被应用时的 props 做为第二个参数

    ```js
    // Correct
    this.setState((state, props) => ({
      counter: state.counter + props.increment
    }));
    ```


  \
  \

**让我们来快速概括一下发生了什么和这些方法的调用顺序：**

1. 当  被传给 `ReactDOM.render()`的时候，React 会调用 `Clock` 组件的构造函数。 因为 `Clock` 需要显示当前的时间，所以它会用一个包含当前时间的对象来初始化 `this.state`。我们会在之后更新 `state`。

2. 之后 React 会调用组件的 `render()` 方法。这就是　React 确定该在页面上展示什么的方式。然后　React 更新 DOM 来匹配 Clock 渲染的输出。

3. 当 Clock 的输出被插入到 DOM 中后， React 就会调用 `ComponentDidMount()` 生命周期方法。在这个方法中，Clock 组件向浏览器请求设置一个计时器来每秒调用一次组件的 `tick()`方法。

4. 浏览器每秒都会调用一次 `tick()` 方法。 在这方法之中，Clock 组件会通过调用 `setState()` 来计划进行一次 UI 更新。得益于 `setState()` 的调用，React 能够知道 state 已经改变了，然后会重新调用 `render()` 方法来确定页面上该显示什么。这一次，render() 方法中的 `this.state.date` 就不一样了，如此以来就会渲染输出更新过的时间。React 也会相应的更新 DOM。

5. 一旦 Clock 组件从 DOM 中被移除，React 就会调用 `componentWillUnmount()` 生命周期方法，这样计时器就停止了。
