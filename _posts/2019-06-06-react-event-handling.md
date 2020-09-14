---
author: lexiao
comments: true
date: 2019-06-06 11:11:11+00:00
layout: post
link: http://localhost/blog/?p=440
published: true
slug: null
title: React 之 事件处理
wordpress_id: 440
categories:
- jsx
- react
---


# 事件处理

[Official Doc](https://zh-hans.reactjs.org/docs/handling-events.html)

* 事件处理函数
    * 当你使用 ES6 class 语法定义一个组件的时候，通常的做法是将事件处理函数声明为 **class 中的方法**。

```js
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    //
    //
    // 为了在回调中使用 `this`，这个绑定是必不可少的
    //
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```

## 向事件处理程序传递参数

* 以下两种方式都可以向事件处理函数传递参数：分别通过箭头函数和 Function.prototype.bind 来实现。

```js
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```








