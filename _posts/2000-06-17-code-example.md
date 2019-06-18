---
author: lexiao
comments: true
date: 2019-06-17 11:11:11+00:00
layout: post
link: http://localhost/blog/?p=440
published: true
slug: null
title: React 之 code example
wordpress_id: 440
categories:
- jsx
- react
- 前端
---

# code example



## event , class, callback

```js

class Toggle extends React.Component {  //////////////
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // 为了在回调中使用 `this`，这个绑定是必不可少的
    this.handleClick = this.handleClick.bind(this);     //////
  }

  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
    }));
  }

  render() {
    return (
        ////////////
      <button onClick={this.handleClick}>       
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>

      //  or 
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>


      // send value to event handler
      <button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
      <button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
    );
  }
}
```

---

## Fragments 允许你将子列表分组，而无需向 DOM 添加额外节点。

```js

class Columns extends React.Component {
  render() {
    return (
      <React.Fragment>
        <td>Hello</td>
        <td>World</td>
      </React.Fragment>
    );
  }
}
```

---






















